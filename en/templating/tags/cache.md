# `{% cache %}`

This tag will cache a portion of your template. Using `cache` can improve performance for subsequent requests of the same page, as they will have less work to do.

```twig
{% cache %}
    {% for block in entry.myMatrixField %}
        <p>{{ block.text }}</p>
    {% endfor %}
{% endcache %}
```

Warning: If you’re suffering from abnormal page load times, you may be experiencing a suboptimal hosting environment. Please consult a specialist before trying `{% cache %}`. `{% cache %}` is not a substitute for fast database connections, efficient templates, or moderate query counts. Possible side effects include stale content, excessively long-running background tasks, stuck tasks, and in rare cases, death. Ask your hosting provider if `{% cache %}` is right for you.

## Parameters

The `{% cache %}` tag supports the following parameters:

### `globally`

Caches the output globally (for the current site), rather than on a per-URL basis.

```twig
{% cache globally %}
```

### `using key`

Specifies the name of the key the cache should use. If you do not provide this, Craft will generate a random key when Twig first parses the template.

```twig
{% cache using key "page-header" %}
```

Tip: You can combine this parameter with [`globally`](#globally) to cache templates on a per-page basis, without letting any query string variables get included in the path:

```twig
{% cache globally using key craft.app.request.pathInfo %}
```

Warning: If you change the template code within a `{% cache %}` that uses a custom key, Craft will not automatically purge any existing template caches. You will either need to assign the tag a new key, or clear your existing template caches manually using the Clear Caches tool in Utilities.

### `for`

The amount of time it should take for the cache to expire.

```twig
{% cache for 3 weeks %}
```

The accepted duration units are:

- `sec`(`s`)
- `second`(`s`)
- `min`(`s`)
- `minute`(`s`)
- `hour`(`s`)
- `day`(`s`)
- `fortnight`(`s`)
- `forthnight`(`s`)
- `month`(`s`)
- `year`(`s`)
- `week`(`s`)

Tip: If you omit this parameter, Craft will use your [cacheDuration](config-settings.md#cacheDuration) config setting to define the default duration.

### `until`

A [DateTime](templating/variables/datetime.md) object defining when the cache should expire.

```twig
{% cache until entry.eventDate %}
```

Tip: You can only use [`for`](#for) **_or_** [`until`](#until) in a single `{% cache %}` tag.

### `if`

Only activates the `{% cache %}` tag if the request meets a certain condition.

```twig
{# Only cache if this is a mobile browser #}
{% cache if craft.request.isMobileBrowser() %}
```

### `unless`

Prevents the `{% cache %}` tag from activating if the request meets a certain condition.

```twig
{# Don't cache if someone is logged in #}
{% cache unless currentUser %}
```

Tip: You can only use [`if`](#if) **_or_** [`unless`](#unless) in a single `{% cache %}` tag.

## Cache clearing

Craft will automatically clear your caches when you save or delete any elements (entries, assets, etc.) within the tags.

If you have any element _queries_ within the tags (e.g. a [craft.entries](templating/craft-entries.md) call), and you create a new element that one of the queries should return, Craft will also be able to figure that out and clear the cache.

You can also manually clear all of your template caches from the Settings page, using the “Clear Caches” tool.

## When to use `{% cache %}` tags

You should use `{% cache %}` tags any time you have a template that’s causing a lot of database queries, or you’re doing something computationally expensive with Twig.

Here are some examples of when to use them:

* A big list of entries
* A Matrix field loop, where some of the blocks have relational fields on them, adding their own additional database queries to the page
* Whenever you’re pulling in data from another site

There are also some cases where it’s _not_ a good idea to use them:

* Don’t use them to cache static text; that will be more expensive than simply outputting the text.
* You can’t use them outside of top-level `{% block %}` tags within a template that extends another.
* The `{% cache %}` tag will only cache HTML, so using tags like [{% css %}](templating/css.md) and [{% js %}](templating/js.md) inside of it doesn’t make sense because they don’t actually output HTML therefore Craft won't cache their output.

    ```twig
    {# Bad: #}

    {% extends "_layout" %}
    {% cache %}
        {% block "content" %}
            ...
        {% endblock %}
    {% endcache %}

    {# Good: #}

    {% extends "_layout" %}
    {% block "content" %}
        {% cache %}
            ...
        {% endcache %}
    {% endblock %}
    ```


Tip: The `{% cache %}` tag will detect if there are any ungenerated [image transform](image-transforms.md) URLs within it. If there are, it will hold off on caching the template until the next request, so those temporary image URLs won’t get cached.
