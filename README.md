Larapi
======
A simple Laravel 5 package for handling common HTTP API responses in JSON form.

## Docs
You can find [v2.x docs here](docs/v2x-docs.md). For the lastest (v3.x) docs see below.

## Installation
Pull in Larapi via composer:

```bash
$ composer require nicklaw5/larapi
```

Then, add the following **Service Provider** to your `providers` array in your `config/app.php` file:

```php
'providers' => array(
	...
	Larapi\LarapiServiceProvider::class,
);
```

## Usage
###Succes Responses###
Available responses:
```php
Larapi::ok();           // 200 HTTP Response
Larapi::created();      // 201 HTTP Response
Larapi::accepted();     // 202 HTTP Response
Larapi::noContent();    // 204 HTTP Response
```

**Example: Return HTTP OK**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	return Larapi::ok();
});
```

will return:
```json
{
	"success": true
}
```
with these headers:
```text
HTTP/1.1 200 OK
Content-Type: application/json
```

**Example: Return HTTP OK with Response Data**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	$data = [
		['id' => 1, 'name' => 'John Doe', 'email' => 'john@doe.com'],
		['id' => 2, 'name' => 'Jane Doe', 'email' => 'jane@doe.com']
	];

	return Larapi::ok($data);
});
```

will return:
```json
{
	"success": true,
	"response": [
		{
			"id": 1,
			"name": "John Doe",
			"email": "john@doe.com"
		},
		{
			"id": 2,
			"name": "Jane Doe",
			"email": "jane@doe.com"
		}
	]
}
```
with these headers:
```text
HTTP/1.1 200 OK
Content-Type: application/json
```

**Example: Return HTTP OK with Custom Response Headers**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	$data = [
		['id' => 1, 'name' => 'John Doe', 'email' => 'john@doe.com'],
		['id' => 2, 'name' => 'Jane Doe', 'email' => 'jane@doe.com']
	];

	$headers = [
		'Header-1' => 'Header-1 Data',
		'Header-2' => 'Header-2 Data'
	];

	return Larapi::ok($data, $headers);
});
```
will return:
```json
{
	"success": true,
	"response": [
		{
			"id": 1,
			"name": "John Doe",
			"email": "john@doe.com"
		},
		{
			"id": 2,
			"name": "Jane Doe",
			"email": "jane@doe.com"
		}
	]
}
```
with these headers:
```text
HTTP/1.1 200 OK
Content-Type: application/json
Header-1: Header-1 Data
Header-2: Header-2 Data
```

###Error Responses###

Available responses:
```php
Larapi::badRequest();           // 400 HTTP Response
Larapi::unauthorized();         // 401 HTTP Response
Larapi::forbidden();            // 403 HTTP Response
Larapi::notFound();             // 404 HTTP Response
Larapi::methodNotAllowed();     // 405 HTTP Response
Larapi::conflict();             // 409 HTTP Response
Larapi::unprocessableEntity();  // 422 HTTP Response
Larapi::internalError();		// 500 HTTP Response
Larapi::notImplemented();       // 501 HTTP Response
Larapi::notAvailable();         // 503 HTTP Response
```


**Example: Return HTTP Bad Request**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	return Larapi::badRequest();
});
```
will return:
```json
{
	"success": false
}
```
with these headers:
```text
HTTP/1.1 400 Bad Request
Content-Type: application/json
```

**Example: Return HTTP Bad Request with Custom Application Error Message**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	$errorCode = 4001;
	$errorMessage = 'Invalid email address.';

	return Larapi::badRequest($errorMessage, $errorCode);
});
```
will return:
```json
{
	"success": false,
	"error_code": 4001,
	"error": "Invalid email address."
}
```
with these headers:
```text
HTTP/1.1 400 Bad Request
Content-Type: application/json
```

**Example: Return HTTP Bad Request with An Array of Errors and Custom Response Headers**

This:
```php
// app/Http/routes.php

Route::get('/', function()
{
	$errorCode = 4001;
	$errors = [
		'email' => 'Invalid email address',
		'password' => 'Not enough characters',
	];

	$headers = [
		'Header-1' => 'Header-1 Data',
		'Header-2' => 'Header-2 Data'
	];

	return Larapi::badRequest($errors, null, $headers);
});
```
will return:
```json
{
	"success": false,
	"errors": {
		"email": "Invalid email address",
		"password": "Not enough characters"
	}
}
```
with these headers:
```text
HTTP/1.1 200 OK
Content-Type: application/json
Header-1: Header-1 Data
Header-2: Header-2 Data
```

## License
Larapi is licensed under the terms of the [MIT License](LICENSE).

## TODO
- test, test, test
