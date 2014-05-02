League of Legends PHP API Client
================================

**This client use the API server provided by : https://github.com/EloGank/lol-php-api**

## Installation

### From Composer

In the `require` key :

``` json
{
    "require": {
        "elogank/php-lol-api-client": "1.0.*@dev"
    }
}
```

Then, in a CLI window :

    php composer.phar update "elogank/php-lol-api"

#### Composer what ?

Ok, here a fast lesson about Composer.  
Composer is a PHP CLI dependency manager. Download Composer in your project root directory, here : https://getcomposer.org/download/

Then, you'll have a new file : `composer.phar`.  
Now create a `composer.json`, and read this brief documentation : https://getcomposer.org/doc/00-intro.md  
Finally, install this repository !

## How to use

The `Client` object need three parameters, and one optionnal :
* `$host` the server host IP address
* `$port` the server port
* `$format` the default output format
* `$throwException` (optionnal, default: true) if true, an `ApiException` will be throw on error and the response won't contain the first array level which contain "success" & "result"/"error" keys.

In short :

``` php
// Declare your client and the configuration
$client = new \EloGank\ApiClient\Client('127.0.0.1', 8080, 'json');

// Do your API request
try {
    $response = $client->send('EUW', 'summoner.summoner_existence', ['Foobar']);
} catch (\EloGank\ApiClient\Exception\ApiException $e) {
    // error
    var_dump($e->getCause(), $e->getMessage());
}
```

#### Example 

This example is available in the file `examples/index.php`.

``` php
// examples/index.php

<?php

require __DIR__ . '/../vendor/autoload.php';

// NOTE: use 192.168.100.10 instead of 127.0.0.1 if you using Virtual Machine for the API server

// Good response, no error, with exception
$client = new \EloGank\ApiClient\Client('127.0.0.1', 8080, 'json');
$response = $client->send('EUW', 'summoner.summoner_existence', ['Foobar']);

var_dump($response);

// Error response, with exception
$client = null;
$client = new \EloGank\ApiClient\Client('127.0.0.1', 8080, 'json');

try {
    $response = $client->send('EUW', 'summoner.summoner_existence', ['Not_found_summoner']);
}
catch (\EloGank\ApiClient\Exception\ApiException $e) {
    var_dump($e->getCause(), $e->getMessage());
}

// Good response, without exception
$client = null;
$client = new \EloGank\ApiClient\Client('127.0.0.1', 8080, 'json', false);
$response = $client->send('EUW', 'summoner.summoner_existence', ['Foobar']);

if ($response['success']) {
    var_dump($response['result']);
}
else {
    // catch error
}

// Error response, without exception
// Good response, without exception
$client = null;
$client = new \EloGank\ApiClient\Client('127.0.0.1', 8080, 'json', false);
$response = $client->send('EUW', 'summoner.summoner_existence', ['Not_found_summoner']);

if ($response['success']) {
    // do some process
}
else {
    var_dump($response['error']);
}
```