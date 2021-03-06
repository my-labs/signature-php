# Signature

**A PHP 5.4+ port of the [Signature](https://github.com/mloughran/signature) ruby gem**

[![Build Status](https://travis-ci.org/philipbrown/signature-php.png?branch=master)](https://travis-ci.org/philipbrown/signature-php)
[![Code Coverage](https://scrutinizer-ci.com/g/philipbrown/signature-php/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/philipbrown/signature-php/?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/philipbrown/signature-php/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/philipbrown/signature-php/?branch=master)

## Installation
Add `philipbrown/signature-php` as a requirement to `composer.json`:
```bash
$ composer require philipbrown/signature-php
```

## What is HMAC-SHA authentication?
HMAC-SHA authentication allows you to implement very simple key / secret authentication for your API using hashed signatures.

## Making a request
```php
use PhilipBrown\Signature\Token;
use PhilipBrown\Signature\Request;

$data    = ['name' => 'Philip Brown'];
$token   = new Token('abc123', 'qwerty');
$request = new Request('POST', 'users', $data);

$auth = $request->sign($token);

$http->post('users', array_merge($auth, $data));

```

## Authenticating a response
```php
use PhilipBrown\Signature\Auth;
use PhilipBrown\Signature\Token;
use PhilipBrown\Signature\Guards\CheckKey;
use PhilipBrown\Signature\Guards\CheckVersion;
use PhilipBrown\Signature\Guards\CheckTimestamp;
use PhilipBrown\Signature\Guards\CheckSignature;
use PhilipBrown\Signature\Exceptions\SignatureException;

$auth  = new Auth('POST', 'users', $_POST, [
	new CheckKey,
	new CheckVersion,
	new CheckTimestamp,
	new CheckSignature
]);
$token = new Token('abc123', 'qwerty');

try {
    $auth->attempt($token);
}

catch (SignatureException $e) {
    // return 4xx
}
```
