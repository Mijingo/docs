# EntryModel

Whenever you’re dealing with an [entry](en/sections-and-entries) in your template, you’re actually working with an EntryModel object.

## Simple Output

Outputting an EntryModel object without attaching a property or method will return the entry’s Title:

```twig
<h1>{{ entry }}</h1>
```


## Properties

EntryModel objects have the following properties:

### `ancestors`

Alias of [getAncestors()](#getAncestors).

### `author`

Alias of [getAuthor()](#getAuthor).

### `authorId`

The entry’s author’s ID.

### `children`

Alias of [getChildren()](#getChildren).

### `cpEditUrl`

Alias of [getCpEditUrl()](#getCpEditUrl).

### `dateCreated`

A [DateTime](en/templating/variables/datetime.md) object of the date the entry was created.

### `dateUpdated`

A [DateTime](en/templating/variables/datetime.md) object of the date the entry was last updated.

### `descendants`

Alias of [getDescendants()](#getDescendants).

### `enabled`

Whether the entry is enabled.

### `expiryDate`

A [DateTime](en/templating/variables/datetime.md) object of the entry’s Expiration Date, if any.

### `hasDescendants`

Whether the entry has any descendants.

Tip: `hasDescendants` will return `true` even if you disable all of the descendants. If you want to determine if the entry has any enabled descendants, you can do this instead:

```twig
{% set hasDescendants = entry.getDescendants().total() != 0 %}
```

### `id`

The entry’s ID.

### `level`

The entry’s level (if it’s in a Structure section).

### `link`

Alias of [getLink()](#getLink).

### `site`

The site the entry was fetched in.

### `next`

Alias of [getNext()](#getNext).

### `nextSibling`

Alias of [getNextSibling()](#getNextSibling).

### `parent`

Alias of [getParent()](#getParent).

### `postDate`

A [DateTime]({entry:templating/datetime}) object of the entry’s Post Date, if any.

### `prev`

Alias of [getPrev()](#getPrev).

### `prevSibling`

Alias of [getPrevSibling()](#getPrevSibling).

### `section`

Alias of [getSection()](#getSection).

### `sectionId`

The ID of the entry’s [section]({entry:docs/sections-and-entries}#sections).

### `siblings`

Alias of [getSiblings()](#getSiblings).

### `slug`

The entry’s slug.

### `status`

The entry’s status (‘live’, ‘pending’, ‘expired’, or ‘disabled’).

### `title`

The entry’s title.

### `type`

Alias of [getType()](#getType).

### `uri`

The entry’s URI.

### `url`

Alias of [getUrl()](#getUrl)


## Methods

EntryModel objects have the following methods:

### `getAncestors( distance )`

Returns the entry’s ancestors (if it lives in a Structure section). You can limit it to only return ancestors that are up to a certain distance away by passing the distance as an argument.

### `getAuthor()`

Returns a [UserModel](en/templating/variables/usermodel.md) object representing the entry’s author, if there is one.

### `getChildren()`

Returns the entry’s children (if it lives in a Structure section). (This is an alias for `getDescendants(1)`)

### `getCpEditUrl()`

Returns the URL to the entry’s edit page within the control panel.

### `getDescendants( distance )`

Returns an {entry:templating/elementcriteriamodel:link} prepped to return the entry’s descendants (if it lives in a Structure section). You can limit it to only return descendants that are up to a certain distance away by passing the distance as an argument.

### `getLink()`

Returns an `<a>` tag, set to the entry’s URL, and using the entry’s title as the text.

### `getNext( params )`

Returns the next entry that should show up in a list based on the parameters entered. This function accepts either a `craft.entries` variable (sans output function), or a parameter array. If you use this within a `craft.entries` loop, it will return the next entry in that loop by default.

### `getNextSibling()`

Returns a Structured entry’s next sibling, if there is one.

Tip: `getNextSibling()` will return the next sibling whether or not it’s enabled. If you want to get the closest enabled sibling, you can do this instead:

```twig
{% set nextSibling = craft.entries.positionedAfter(entry).order('lft asc').one() %}
```

### `getParent()`

Returns a Structured entry’s parent, if it’s not a top-level entry.

Tip: `getParent()` will return the parent whether or not it’s enabled. If you want to get the closest enabled ancestor, you can do this instead:

```twig
{% set parent = craft.entries.ancestorOf(entry).order('lft desc').one() %}
```

### `getPrev( params )`

Returns the previous entry that would have shown up in a list based on the parameters entered. This function accepts either a `craft.entries` variable (sans output function), or a parameter array. If you use this within a `craft.entries` loop, it will return the previous entry in that loop by default.

### `getPrevSibling()`

Returns a Structured entry’s previous sibling, if there is one.

Tip: `getPrevSibling()` will return the previous sibling whether or not it’s enabled. If you want to get the closest enabled sibling, you can do this instead:

```twig
{% set prevSibling = craft.entries.positionedBefore(entry).order('lft desc').one() %}
```

### `getSection()`

Returns a [SectionModel](en/templating/variables/sectionmodel.md) object representing the entry’s section.

### `getSiblings()`

Returns the entry’s siblings (if it lives in a Structure section).

### `getType()`

Returns an [EntryTypeModel](en/templating/variables/entrytypemodel.md) object representing the entry’s type.

### `getUrl()`

Returns the entry’s URL, if any.

### `hasDescendants()`

Returns whether the entry has any descendants (if it lives in a Structure section).

### `isAncestorOf( entry )`

Returns whether the entry is an ancestor of another entry.

```twig
{% nav page in craft.entries.section('pages').all() %}
    {% set expanded = entry is defined and page.isAncestorOf(entry) %}
    <li{% if expanded %} class="expanded"{% endif %}>
        {{ page.getLink() }}
        {% ifchildren %}
            <ul>
                {% children %}
            </ul>
        {% endifchildren %}
    </li>
{% endnav %}
```

### `isChildOf( entry )`

Returns whether the entry is a direct child of another entry.

### `isDescendantOf( entry )`

Returns whether the entry is a descendant of another entry.

### `isNextSiblingOf( entry )`

Returns whether the entry is the next sibling of another entry.

### `isParentOf( entry )`

Returns whether the entry is a direct parent of another entry.

### `isPrevSiblingOf( entry )`

Returns whether the entry is the previous sibling of another entry.

### `isSiblingOf( entry )`

Returns whether the entry is a sibling of another entry.

Here’s an example of `getNext()` and `getPrev()` in action:

```twig
{% set params = {
    section: 'cocktails',
    order:   'title'
} %}

{% set prevCocktail = entry.getPrev(params) %}
{% set nextCocktail = entry.getNext(params) %}

{% if prevCocktail %}
    <p>Previous: <a href="{{ prevCocktail.url }}">{{ prevCocktail.title }}</a></p>
{% endif %}

{% if nextCocktail %}
    <p>Next: <a href="{{ nextCocktail.url }}">{{ nextCocktail.title }}</a></p>
{% endif %}
```

