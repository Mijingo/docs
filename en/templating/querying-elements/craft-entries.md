# `craft.entries`

You can access your site’s [entries](en/sections-and-entries) from your templates via `craft.entries`.

```twig
{% for entry in craft.entries.section('news').limit(10).all() %}
    <article>
        <h1><a href="{{ entry.url }}">{{ entry.title }}</a></h1>
        {{ entry.summary }}
        <a href="{{ entry.url }}">Continue reading</a>
    </article>
{% endfor %}
```

## Parameters

`craft.entries` supports the following parameters:

### `after`

Only fetch entries with a Post Date that is on or after the given date.

{entry:snippets/date-param-values:body}

### `ancestorOf`

Only fetch entries that are an ancestor of a given entry within a Structure section. Accepts an [EntryModel]({entry:templating/entrymodel}) object.

### `ancestorDist`

Only fetch entries that are a given number of levels above the entry specified by the `ancestorOf` param.

### `archived`

Only fetch entries that have been archived by setting this to `true`.

### `authorGroup`

Only fetch entries that were authored by users who belong to the group with the given handle.

### `authorGroupId`

Only fetch entries that were authored by users who belong to the group with the given ID.

### `authorId`

Only fetch entries that were authored by the user with the given ID.

### `before`

Only fetch entries with a Post Date that is before the given date.

{entry:snippets/date-param-values:body}

### `level`

Only fetch entries at a certain level within a Structure section.

### `enabledForSite`

Set to `false` to fetch entries that aren’t enabled for the current site. (By default they won’t show up.)

### `descendantOf`

Only fetch entries that are a descendant of a given entry within a Structure section. Accepts an [EntryModel]({entry:templating/entrymodel}) object.

### `descendantDist`

Only fetch entries that are a given number of levels below the entry specified by the `descendantOf` param.

### `fixedOrder`

If set to `true`, entries will be returned in the same order as the IDs entered in the [`id`](#id) param.

### `id`

Only fetch the entry with the given ID.

### `indexBy`

Indexes the results by a given property. Possible values include `'id'` and `'title'`.

### `limit`

Limits the results to *X* entries.

### `site`

The site the entries should be returned in. Defaults to the current site.

### `nextSiblingOf`

Only fetch the entry which is the next sibling of the given entry within a Structure section. Accepts either an [EntryModel]({entry:templating/entrymodel}) object or an entry’s ID.

### `offset`

Skips the first *X* entries.

For example, if you set `offset(1)`, the would-be second entry returned becomes the first.

### `orderBy`

The order Craft should return the entries. Possible values include `'title'`, `'id'`, `'authorId'`, `'sectionId'`, `'slug'`, `'uri'`, `'postDate'`, `'expiryDate'`, `'dateCreated'`, and `'dateUpdated'`, as well as any textual custom field handles. If you want the entries to be sorted in descending order, add “`desc`” after the property name (ex: `'postDate desc'`). The default value is `'postDate desc'`.

### `positionedAfter`

Only fetch entries which are positioned after the given entry within a Structure section. Accepts either an [EntryModel]({entry:templating/entrymodel}) object or an entry’s ID.

### `positionedBefore`

Only fetch entries which are positioned before the given entry within a Structure section. Accepts either an [EntryModel]({entry:templating/entrymodel}) object or an entry’s ID.

### `postDate`

Fetch entries based on their Post Date.

### `prevSiblingOf`

Only fetch the entry which is the previous sibling of the given entry within a Structure section. Accepts either an [EntryModel]({entry:templating/entrymodel}) object or an entry’s ID.

### `relatedTo`

Only fetch entries that are related to certain other elements. (See {entry:docs/relations:link} for the syntax options.)

### `search`

Only fetch entries that match a given search query. (See {entry:docs/searching:link} for the syntax and available search attributes.)

### `section`

Only fetch entries that belong to a given section(s). Accepted values include a section handle, an array of section handles, or a [SectionModel]({entry:templating/sectionmodel}) object.

### `sectionId`

Only fetch entries that belong to a given section(s), referenced by its ID.

### `siblingOf`

Only fetch entries which are siblings of the given entry within a Structure section. Accepts either an [EntryModel]({entry:templating/entrymodel}) object or an entry’s ID.

### `slug`

Only fetch the entry with the given slug.

### `status`

Only fetch entries with the given status. Possible values are `'live'`, `'pending'`, `'expired'`, `'disabled'`, and `null`. The default value is `'live'`. `null` will return all entries regardless of status.

An entry is `'live'` if it is enabled, has a Post Date in the past and an Expiration Date in the future. An entry is `'pending'` if it is enabled and has Post and Expiration Dates in the future. An entry is `'expired'` if it is enabled and has Post and Expiration Dates in the past.

### `title`

Only fetch entries with the given title.

### `type`

Only fetch entries of the given [entry type]({entry:docs/sections-and-entries}#entry-types). This parameter accepts an entry type handle.

### `uri`

Only fetch the entry with the given URI.