# `craft.app.config`

You can access your config settings from your templates with `craft.config`.

## Properties

You can access any of your config settings in craft/config/general.php by treating them as properties of `craft.config.general`:

```twig
{% if craft.app.config.general.devMode %}
    <p>Craft is running in Dev Mode.</p>
{% endif %}
```
