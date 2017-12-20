# `craft.app.fields`

You can access your site’s fields with `craft.app.fields`.

## Methods

The following methods are available:

### `getFieldByHandle( handle )`

Returns a [FieldModel](/classreference/models/FieldModel) object representing a field by its handle.

```twig
{% set body = craft.app.fields.getFieldByHandle('body') %}
{{ body.instructions }}
```