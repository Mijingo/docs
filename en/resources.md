Front-End Resources
===================

Plugins, like Craft, are supposed to be installed above the web root, which ensures that their files can’t be accessed directly via HTTP requests. Generally that’s a Very Good Thing, because it protects Craft sites from a whole host of security vulnerabilities.

There’s one case where it would be nice if HTTP requests *could* access Craft/plugin files directly though: front-end resources, such as CSS and JS files.

Between Craft and Yii, there are a couple ways you can address this.

- [Asset Bundles](#asset-bundles)
  - [Setting it Up](#setting-it-up)
  - [Registering the Asset Bundle](#registering-the-asset-bundle)
  - [Getting Published File URLs](#getting-published-file-urls)
- [Dynamic Resources](#dynamic-resources)
  - [Linking to Dynamic Resources](#linking-to-dynamic-resources)

## Asset Bundles

[Asset bundles](http://www.yiiframework.com/doc-2.0/guide-structure-assets.html) do two things:

- They publish an inaccessible directory into a directory below the web root, making it available for front-end pages to consume via HTTP requests.
- They can register specific CSS and JS files within the directory as `<link>` and `<script>` tags in the currently-rendered page.

### Setting it Up

First establish where you want your web-publishable files to live within your plugin. Give them a directory that’s just for them. This will be the asset bundle’s **source directory**. For this example, we’ll go with `resources/`.

Then, create a file that will hold your asset bundle class. This can be located and named whatever you want. We’ll go with `FooBundle` here.

Here’s what your plugin’s structure should look like:

```
base_dir/
  src/
    FooBundle.php
    resources/
      script.js
      styles.css
      ...
```

Use this template as a starting point for your asset bundle class:

```php
<?php
namespace ns\prefix;

use craft\web\AssetBundle;
use craft\web\assets\cp\CpAsset;

class MyPluginAsset extends AssetBundle
{
    public function init()
    {
        // define the path that your publishable resources live
        $this->sourcePath = '@ns/prefix/resources';

        // define the dependencies
        $this->depends = [
            CpAsset::class,
        ];

        // define the relative path to CSS/JS files that should be registered with the page
        // when this asset bundle is registered
        $this->js = [
            'script.js',
        ];

        $this->css = [
            'styles.css',
        ];

        parent::init();
    }
}
```

### Registering the Asset Bundle

With that in place, all that is left is to register the asset bundle wherever its JS/CSS files are needed.

You can register it from a template using this code:

```twig
{% do view.registerAssetBundle("ns\\prefix\\FooBundle") %}
```

Or if the request is getting routed to a custom controller action, you could register it from there, before your template gets rendered:

```php
use ns\prefix\FooBundle;

public function actionFoo()
{
    $this->view->registerAssetBundle(FooBundle::class);

    return $this->renderTemplate('plugin-handle/foo');
}
```

### Getting Published File URLs

If you have a one-off file that you need to get the published URL for, but it doesn’t need to be registered as a CSS or JS file on the current page, you can use `craft\web\AssetManager::getPublishedUrl()`:

```php
$url = \Craft::$app->assetManager->getPublishedUrl('@ns/prefix/path/to/file.svg', true);
```

Craft will automatically publish the file for you (if it’s not published already), and return its URL.

If the file lives within an asset bundle’s source directory, then only pass the source directory’s path into `getPublishedUrl()` and append the relative path after it. That way Craft won’t have to publish the file twice – (here, in addition to being published along with the rest of the asset bundle).

```php
$url = \Craft::$app->assetManager->getPublishedUrl('@ns/prefix/resources', true).'/path/to/file.svg';
```

## Dynamic Resources

Asset bundles are great for static files. If you need to make dynamically-generated files available on the front-end, Craft has the concept of “resource requests”.

Resource requests have URIs that begin with `admin/resources/` in the Control Panel, or the `resourceTrigger` config setting value for front-end request. They get routed to the Resources service (`craft\services\Resources::sendResource()`), which will attempt to map the URI to a file path, and send the file back to the client.

The Resources service has a number of URI checks it will run through. If each of those checks fail, it will fire a `resolveResourcePath` event, giving plugins an opportunity to do their own URI-file path matching.

Here’s how your plugin can listen to that event from its `init()` method:

```php
use craft\events\ResolveResourcePathEvent;
use craft\services\Resources;
use yii\base\Event;

public function init()
{
    Event::on(Resources::class, Resources::EVENT_RESOLVE_RESOURCE_PATH, function(ResolveResourcePathEvent $event) {
        if ($event->uri === 'foo/bar') {
            $event->path = \Craft::getAlias('@ns/prefix/path/to/foo/bar.ext');
            
            // Prevent other event listeners from getting invoked
            $event->handled = true;
        }
    });
}
```

### Linking to Dynamic Resources

To link to a dynamic resource, use `craft\helpers\UrlHelper::resourceUrl()`:

#### PHP

```php
use craft\helpers\UrlHelper;

$url = UrlHelper::resourceUrl('foo/bar'); 
```

#### Twig

```twig
{% set url = resourceUrl('foo/bar') %}
```

#### Control Panel JavaScript

```js
var url = Craft.getResourceUrl('foo/bar');
```