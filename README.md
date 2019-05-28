<p align=center><img height=150 src="https://raw.githubusercontent.com/php-protein/docs/master/assets/protein-large.png"></p>


# Protein | Response
## The response module wrap and handles the payload sended to the request agent.

### Install
---

```
composer require proteins/response
```

Require the class via :

```php
use Proteins\Response;
```

### Appending data to the response
---

Append a string to the response buffer via the `add` method.

```php
Response::add('Hello world!');
```

### Changing the content type
---

The `type` method accepts a MIME type string (or a `Response::TYPE_*` constant) for the body content type.

```php
Response::send();
```


### Adding an header to the response
---

The `header($name, $value)` method set an header for being sended to the request agent.

```php
Response::header('Authorization','Bearer mF_9.B5f-4.1JqM');
```

```
Authorization: Bearer mF_9.B5f-4.1JqM
```

### Get all defined headers
---

```php
$response_headers = Response::headers();
```

### Get response body
---

```php
$response_body = Response::body();
```

### Set the entire response body
---

You can set the entire response body by passing a parameter to the `body` method.

```php
Response::body($new_body);
```

### Set the HTTP response status
---

You can set the HTTP response status with the `status` method.

```php
Response::status(401);
```

The `error($code, $message='')` method is used to pass errors. 

> This method triggers the `core.response.error` event.

```php
Response::error(401, "User not authorized");
```


### Force download of response body
---

You can force download of response body with `download` method passing filename as parameter. 
Pass a falsy value to disable download.

```php
Response::download("export.csv");
```
Download method support also array as parameter with raw string data.

```php
Response::download([
	"filename" 	=> "export.csv",
	"charset" 	=> "utf-8",
	"mime" 		=> "text/csv",
	"body" 		=> CSV::fromSQL("SELECT * FROM users")
]);
```

### HTTP/2 Server Push
---

The HTTP/Server Push support is enabled via the `push` method.  
If you have a list of resource links to be pushed in the next `Response::send` you can pass the URI and the resource type as defined in the [W3C Preload Draft](https://www.w3.org/TR/preload/#server-push-http-2)

```php
Response::push('/assets/css/main.css');
Response::push('/assets/js/main.js');
```

If you don't pass resource type as a second parameter the code will be guess from the extension, however is better (faster) to specify the resource type (for the `as` parameter of the preload header format).

The current auto-discovered resource types are :

| Extensions | Type |
|:----------:|:----:|
| `js` | `script` |
| `css` | `style` |
| `woff`,`woff2`,`ttf`,`eof` | `font` |
| `png`,`svg`,`gif`,`jpg` | `image` |
| *other* | `text` |


```php
Response::push('/assets/css/main.css','style');
Response::push('/assets/js/main.js','script');
```

Multiple resources can be passed to a single `push` call via an array :

```php
Response::push([
  '/assets/css/main.css',
  '/assets/js/main.js',
]);
```

and as the same as the direct call version you can define resource types via array-keys :

```php
Response::push([
  'style'  => '/assets/css/main.css',
  'script' => [
    '/assets/js/vendors.js',
    '/assets/js/main.js',
  ],
]);
```

