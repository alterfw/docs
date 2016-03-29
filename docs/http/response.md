---
title: Response
---

The HTTP response returned to the client will have a body. The HTTP body is the actual content of the HTTP response
delivered to the client. You can use the Response object to set the HTTP response’s body. The response object is binded to route and middlewares closures, so you can access usind `$this`:

```php
Route::get('/', function(){

  // Overwrite response body
  $this->setBody('Foo');

  // Append response body
  $this->write('Bar');

});
```

When you overwrite or append the response object’s body, the response object will automatically set the
**Content-Length** header based on the bytesize of the new response body.

You can fetch the response object’s body like this:

```php
$body = $this->getBody();
```


## Status

The HTTP response returned to the client will have a status code indicating the response’s type
(e.g. 200 OK, 400 Bad Request, or 500 Server Error). You can use the Response object to set the
HTTP response’s status like this:

```php
$this->setStatus(400);
```

You only need to set the response object’s status if you intend to return an HTTP response that *does not* have
a 200 OK status. You can just as easily fetch the response object’s current HTTP status by invoking the same
method without an argument, like this:

```php
$status = $this->getStatus();
```


## Headers

The HTTP response returned to the HTTP client will have a header. The HTTP header is a list of keys and values that
provide metadata about the HTTP response. You can use the Response object to set the HTTP
response’s header.

```php
$this->headers->set('Content-Type', 'application/json');
```

You may also fetch headers from the response object's `headers` property, too:

```php
$this->headers->get('Content-Type');
```


If a header with the given name does not exist, `null` is returned. You may specify header names with upper, lower,
or mixed case with dashes or underscores. Use the naming convention with which you are most comfortable.

# Helpers

The response object provides helper methods to inspect and interact with the underlying HTTP response.

### Finalize

The response object’s `finalize()` method returns a numeric array of `[status, header, body]`. The status is
an integer; the header is an iterable data structure; and the body is a string. Were you to create a new
`\Ampersand\Http\Response` object in your Slim application or its middleware, you would call the response object's
`finalize()` method to produce the status, header, and body for the underlying HTTP response.

```php
/**
 * Prepare new response object
 */
$res = new \Ampersand\Http\Response();
$res->setStatus(400);
$res->write('You made a bad request');
$res->headers->set('Content-Type', 'text/plain');

/**
 * Finalize
 * @return [
 *     200,
 *     ['Content-type' => 'text/plain'],
 *     'You made a bad request'
 * ]
 */
$array = $res->finalize();
```

### Redirect

The response object’s `redirect()` method will set the response status and its **Location:** header needed to
return a **3xx Redirect** response.

```php
$res->redirect('/foo', 303);
```

### Returning JSON

The response object’s `toJSON()` method will encode any parameter, set the `application/json` to `Content-Type` header and ouput to the user:

```php
$res->toJSON(['error'=>'Not found!']);
```

### Status Introspection

The response object provides other helper methods to inspect its current status. All of the following methods
return a boolean value:

```php
//Is this an informational response?
$res->isInformational();

//Is this a 200 OK response?
$res->isOk();

//Is this a 2xx successful response?
$res->isSuccessful();

//Is this a 3xx redirection response?
$res->isRedirection();

//Is this a specific redirect response? (301, 302, 303, 307)
$res->isRedirect();

//Is this a forbidden response?
$res->isForbidden();

//Is this a not found response?
$res->isNotFound();

//Is this a client error response?
$res->isClientError();

//Is this a server error response?
$res->isServerError();
```
