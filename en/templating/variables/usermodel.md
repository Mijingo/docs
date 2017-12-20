# UserModel

Whenever you’re dealing with a [user](en/users.md) in your template, you’re actually working with a UserModel object.

## Simple Output

Outputting a UserModel object without attaching a property or method will return the user’s username:

```twig
<p>Written by {{ entry.author }}</p>
```


## User Properties

UserModel objects have the following properties:

### `admin`

Whether the user is an admin.

### `dateCreated`

A [DateTime](en/templating/datetime.md) object of the date the user was created.

### `dateUpdated`

A [DateTime](en/templating/datetime.md) object of the date the user was last updated.

### `email`

The user’s email address.

### `fullName`

Alias of [getFullName()](#getFullName).

### `friendlyName`

Alias of [getFriendlyName()](#getFriendlyName).

### `groups`

Alias of [getGroups()](#getGroups).

### `firstName`

The user’s first name.

### `lastName`

The user’s last name.

### `lastLoginDate`

A [DateTime]({entry:templating/datetime}) of the last time the user logged in.

### `name`

Alias of [getName()](#getName).

### `next`

Alias of [getNext](#getNext).

### `id`

The user’s ID.

### `isCurrent`

Whether this is the currently logged-in user.

### `photoUrl`

Alias of [getPhotoUrl](#getPhotoUrl).

### `preferredLocale`

The user’s preferred locale.

### `prev`

Alias of [getPrev](#getPrev).

### `status`

The user’s status ('active', 'locked', 'suspended', 'pending', 'archived').

### `username`

The user’s username.


## Methods

UserModel objects have the following methods:

### `can( 'permission' )`

Returns whether the user has a given permission, either directly or via one of its groups.

```twig
{% if currentUser.can('accessCp') %}
    <a href="{{ cpUrl() }}">Control Panel</a>
{% endif %}
```

### `getFullName()`

Returns the user’s first and last name.

### `getFriendlyName()`

Returns the user’s first name if they’ve entered it, otherwise their username.

```twig
<p>Hello, {{ currentUser.getFriendlyName() }}</p>
```

### `getGroups()`

Returns an array of [UserGroupModel](en/templating/variables/usergroupmodel.md) objects that the user belongs to.

### `getName()`

Returns the user’s full name if they’ve entered it, otherwise their username.

### `getNext( params )`

Returns the next user that should show up in a list based on the parameters entered. This function accepts either a `craft.tags` variable (sans output function), or a parameter array.

Here’s an example of the `getPrev()` and `getNext()` methods in action:

```twig
{% set params = {
    group: 'authors',
    order: 'firstName, lastName'
} %}

{% set prevAuthor = entry.getPrev(params) %}
{% set nextAuthor = entry.getNext(params) %}

{% if prevAuthor %}
    <p>Previous: <a href="/authors/{{ prevAuthor.username }}">{{ prevAuthor.name }}</a></p>
{% endif %}

{% if nextAuthor %}
    <p>Next: <a href="/authors/{{ nextAuthor.username }}">{{ nextAuthor.name }}</a></p>
{% endif %}
```

### `getPhotoUrl( size )`

Returns a URL to the user’s photo at a given size in pixels (defaults to 100). You can also pass in `"original"` if you want to get a non-resized photo back.

### `getPrev( params )`

Returns the previous user that would have shown up in a list based on the parameters entered. This function accepts either a `craft.tags` variable (sans output function), or a parameter array.

Here’s an example of the `getPrev()` and `getNext()` methods in action:

```twig
{% set params = {
    group: 'authors',
    order: 'firstName, lastName'
} %}

{% set prevAuthor = entry.getPrev(params) %}
{% set nextAuthor = entry.getNext(params) %}

{% if prevAuthor %}
    <p>Previous: <a href="/authors/{{ prevAuthor.username }}">{{ prevAuthor.name }}</a></p>
{% endif %}

{% if nextAuthor %}
    <p>Next: <a href="/authors/{{ nextAuthor.username }}">{{ nextAuthor.name }}</a></p>
{% endif %}
```

### `isInGroup( group )`

Returns whether the user belongs to a given group. This method accepts a [UserGroupModel](en/templating/variables/usergroupmodel.md) instance, a group ID, or a group handle (string).