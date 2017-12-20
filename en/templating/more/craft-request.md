# `craft.app.request`

You can get all sorts of info about the current request from `craft.app.request`.

## Properties

The following properties are available:

### `segments`

Returns all segments. Use a filter to isolate a particular segment:

```twig

{{ craft.app.request.segments | first }}
{{ craft.app.request.segments | last }}

```


### `isAjax`

Whether the current request is an Ajax request.

### `isLivePreview`

Whether the current request is a Live Preview request.

```twig
{% if not craft.app.request.isLivePreview %}
    <script type="text/javascript">
        // Google Analytics tracking code
    </script>
{% endif %}
```

### `isSecureConnection`

Whether the current request is over SSL. Returns a boolean.

### `pageNum`

Alias of [`getPageNum()`](#getPageNum).

### `path`

Alias of [`getPath()`](#getPath).

### `queryString`

Alias of [`getQueryString()`](#getQueryString).

### `queryStringWithoutPath`

Alias of [`getQueryStringWithoutPath()`](#getQueryStringWithoutPath).

### `segments`

Alias of [`getSegments()`](#getSegments).

### `serverName`

Alias of [`getServerName()`](#getServerName).

### `url`

Alias of [`getUrl()`](#getUrl).

### `urlReferrer`

Alias of [`getUrlReferrer()`](#getUrlReferrer).



## Methods

The following methods are available:

### `isMobileBrowser()`

Whether the current request is coming from a mobile browser. Pass in `true` if you want to consider tablets as mobile.

### `getCookie( name )`

Returns a cookie with the given name if it exists. If the cookie was set in JavaScript, this method will not work because all cookies in Craft go through some validation to ensure they weren’t tampered with.

### `getFirstSegment()`

Returns the first path segment in the URL.

### `getLastSegment()`

Returns the last path segment in the URL.


### `getParam( name )`

Returns a parameter from either the query string or POST data.

### `getPath()`

Returns the full path in the URL.

### `getPost( name )`

Returns a parameter from the POST data.

### `getSegment( n )`

Returns the *nth* path segment in the URL. If you pass a negative number, the *nth*-to-last segment will be returned instead.

```twig
The second URL segment is {{ craft.request.getSegment(2) }}.
The second-to-last URL segment is {{ craft.request.getSegment(-2) }}.
```

### `getSegments()`

Returns an array of the path segments in the URL.

### `getServerName()`

Returns the server/domain name.

### `getUserAgent()`

Returns the user agent string or null if not present.

### `getQuery( name )`

Returns a parameter from the query string.

### `getQueryString()`

Returns the full query string.

### `getQueryStringWithoutPath()`

Returns the query string, except for the `p=` param (which was probably added by your .htaccess redirect).

```twig
<a href="{{ paginate.nextUrl }}?{{ craft.request.getQueryStringWithoutPath() }}">Next Page</a>
```

### `getUrl()`

Returns the full URL for the current request.

Warning: By the time the request makes it to Craft, the _actual_ URL will be whatever your .htaccess file has redirected the request to behind the scenes, e.g. http://example.com/index.php?p=some/path. So rather than returning the actual URL, `getUrl()` returns what the URL probably looks like to the browser. It’s really just a shortcut for calling [`url()`]({entry:templating/functions}#url) and passing in [`craft.request.path`](#path).

```twig
{{ url(craft.request.path) }}
```

### `getUrlReferrer()`

Returns the request’s HTTP_REFERER header, if there was one.