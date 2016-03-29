---
title: Request
---

Use the request object's `getBody()` method to fetch the raw HTTP request body sent by the HTTP client. This is
particularly useful for applications that consume JSON or XML requests.

```php
Route::get('/hello/:name', function(){
  $body = $this->request->getBody();
});
```


## How to Deal with Content-Types

By default Alter does not parse any other content-type other than the standard form data because PHP does not support it. That means if you attempt to post application/json you will not be able to access the data via `$this->request->post()`;

To solve this you must parse the content type yourself. You can either do this on a per-route basis

```php
//For application/json
$data = json_decode($this->request->getBody());
```

## Cookies

### Get Cookies

Alter will automatically parse cookies sent with the current HTTP request. You can fetch cookie values
with the request object like this:

```php
$cookies = $this->request->cookies;
```

Only Cookies sent with the current HTTP request are accessible with this method. If you set a cookie during the
current request, it will not be accessible with this method until the subsequent request.

### Cookie Encryption

You can optionally choose to encrypt all cookies stored on the HTTP client with the Slim app's `cookies.encrypt`
setting. When this setting is `true`, all cookies will be encrypted using your designated secret key and cipher.

It's really that easy.

# Headers

Alter will automatically parse all HTTP request headers. You can access the request headers using the
request object's `headers` property.

```php
// Get request headers as associative array
$headers = $this->request->headers;

// Get the ACCEPT_CHARSET header
$charset = $this->request->headers->get('ACCEPT_CHARSET');
```

The HTTP specification states that HTTP header names may be uppercase, lowercase, or mixed-case. Slim is smart enough
to parse and return header values whether you request a header value using upper, lower, or mixed case header name,
with either underscores or dashes. So use the naming convention with which you are most comfortable.

# Ajax Requests

When using a Javascript framework like MooTools or jQuery to execute an XMLHttpRequest, the XMLHttpRequest will
usually be sent with a **X-Requested-With** HTTP header. The Slim application will detect the HTTP
request’s **X-Requested-With** header and flag the request as such. If for some reason an XMLHttpRequest cannot
be sent with the **X-Requested-With** HTTP header, you can force the Alter to assume an HTTP request
is an XMLHttpRequest by setting a GET, POST, or PUT parameter in the HTTP request named “isajax” with a truthy value.

Use the request object's `isAjax()` or `isXhr()` method to tell if the current request is an XHR/Ajax request:

```php
$isXHR = $app->request->isAjax();
$isXHR = $app->request->isXhr();
```

# Helpers

The Request object provides several helper methods to fetch common HTTP request information:

### Content Type

Fetch the request's content type (e.g. "application/json;charset=utf-8"):

```php
$req->getContentType();
```

### Media Type

Fetch the request's media type (e.g. "application/json"):

```php
$req->getMediaType();
```

### Media Type Params

Fetch the request's media type parameters (e.g. [charset => "utf-8"]):

```php
$req->getMediaTypeParams();
```

### Content Charset

Fetch the request's content character set (e.g. "utf-8"):

```php
$req->getContentCharset();
```

### Content Length

Fetch the request's content length:

```php
$req->getContentLength();
```

### Host

Fetch the request's host (e.g. "slimframework.com"):

```php
$req->getHost();
```

### Host with Port

Fetch the request's host with port (e.g. "slimframework.com:80"):

```php
$req->getHostWithPort();
```

### Port

Fetch the request's port (e.g. 80):

```php
$req->getPort();
```

### Scheme

Fetch the request's scheme (e.g. "http" or "https"):

```php
$req->getScheme();
```

### Path

Fetch the request's path (root URI + resource URI):

```php
$req->getPath();
```

### URL

Fetch the request's URL (scheme + host [ + port if non-standard ]):

```php
$req->getUrl();
```

### IP Address

Fetch the request's IP address:

```php
$req->getIp();
```

### Referer

Fetch the request's referrer:

```php
$req->getReferrer();
```

### User Agent

Fetch the request's user agent string:

```php
$req->getUserAgent();
```

# Request Methods

Every HTTP request has a method (e.g. GET or POST). You can obtain the current HTTP request method via the Request object:

```php
/**
 * What is the request method?
 * @return string (e.g. GET, POST, PUT, DELETE)
 */
$req->getMethod();

/**
 * Is this a GET request?
 * @return bool
 */
$req->isGet();

/**
 * Is this a POST request?
 * @return bool
 */
$req->isPost();

/**
 * Is this a PUT request?
 * @return bool
 */
$req->isPut();

/**
 * Is this a DELETE request?
 * @return bool
 */
$req->isDelete();

/**
 * Is this a HEAD request?
 * @return bool
 * @return bool
 */
$req->isHead();

/**
 * Is this a OPTIONS request?
 * @return bool
 */
$req->isOptions();

/**
 * Is this a PATCH request?
 * @return bool
 */
$req->isPatch();

/**
 * Is this a XHR/AJAX request?
 * @return bool
 */
$req->isAjax();
```
