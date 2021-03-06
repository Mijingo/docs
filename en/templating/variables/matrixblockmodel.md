# MatrixBlockModel

Whenever you’re dealing with a [Matrix]({entry:docs/matrix-fields}) block in your template, you’re actually working with a MatrixBlockModel object.

## Properties

MatrixBlockModel objects have the following properties:

### `dateCreated`

A [DateTime]({entry:templating/datetime}) object of the date the block was created.

### `dateUpdated`

A [DateTime]({entry:templating/datetime}) object of the date the block was last updated.

### `fieldId`

The ID of the Matrix field the block belongs to.

### `id`

The block’s ID.

### `locale`

The locale the block was fetched in.

### `next`

Alias of [`getNext()`](#getNext).

### `owner`

Alias of [`getOwner()`](#getOwner).

### `ownerId`

The ID of the element that the block’s Matrix field belongs to.

### `prev`

Alias of [`getPrev()`](#getPrev).

### `sortOrder`

The position of the block within its Matrix field.

### `type`

The block type’s handle.


## Methods

MatrixBlockModel objects have the following methods:

### `getNext( params )`

Returns the next block in the current list.

### `getOwner()`

Returns the element that the block’s Matrix field belongs to.

### `getPrev( params )`

Returns the previous block in the current list.

