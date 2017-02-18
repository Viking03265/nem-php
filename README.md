# evias/php-nem-laravel

[![Build Status](https://api.travis-ci.org/evias/php-nem-laravel.svg?branch=master)](https://travis-ci.com/evias/php-nem-laravel)
[![Latest Stable Version](https://poser.pugx.org/evias/php-nem-laravel/version)](https://packagist.org/packages/evias/php-nem-laravel)
[![Latest Unstable Version](https://poser.pugx.org/evias/php-nem-laravel/v/unstable)](//packagist.org/packages/evias/php-nem-laravel)
[![License](https://poser.pugx.org/evias/php-nem-laravel/license)](https://packagist.org/packages/evias/php-nem-laravel)

This package aims to provide with an easy-to-use PHP Laravel Namespace helping with to communicate with the NEM blockchain through its NIS API.

This package should be an aid to any developer working on Laravel applications with the NEM blockchain.

Package licensed under [MIT](LICENSE) License.

## Documentation

Reader-friendly Documentation will be added in development period and will be available on the Github Wiki at [evias/php-nem-laravel Wiki](https://github.com/evias/php-nem-laravel/wiki).

## Pot de vin

If you like the initiative, and for the sake of good mood, I recommend you take a few minutes to Donate a beer or Three [because belgians like that] by sending some XEM (or whatever Mosaic you think pays me a few beers someday!) to my Wallet:

    NB72EM6TTSX72O47T3GQFL345AB5WYKIDODKPPYW

## Installation

You can install this package with Composer. You only need to require evias/php-nem-laravel.

    $ composer require evias/php-nem-laravel

The package can also be download manually by cloning this repository or by download the packagist archive:

	$ https://packagist.org/packages/evias/php-nem-laravel

## Usage / Examples

When you have installed the evias/php-nem-laravel package you will be able to use the API class to send API requests to the configured NIS. By default, the config/nem.php file defines the localhost NIS to be used, this can be changed.

If you are using Laravel or Lumen, you will need to register the Service Provider of this package into your app:

	$app = Laravel\Lumen\Application(realpath(__DIR__));
    $app->register(evias\NEMBlockchain\NemBlockchainServiceProvider::class);

    // The configuration can be retrieved with "nem.config" IoC binding:
    $nemConfig = $app["nem.config"]

    // A pre-configured API client instance can be retrieved with "nem" IoC binding:
    // the configuration file at config/nem.php will define which host, port, protocol,
    // etc. is used according to the current APP_ENV.
    $nemAPI = $app["nem"];

    // You can also create a new instance of the API
    $myAPI = new evias\NEMBlockchain\API();
    $myAPI->setOptions([
		"use_ssl" => false,
		"host" 	  => "go.nem.ninja",
		"port"    => 7890,
		"endpoint" => "/",
    ]);

    // If you wish you can define your own RequestHandler, have a look at the
    // evias\NEMBlockchain\Contracts\RequestHandler interface.
    $myAPI->setOptions(["handler_class" => Path\To\My\Handler::class]);

    // The API wrapper class can be used to send API requests to the
    // configured NIS host with following snippet:

	$response = $nemAPI->getJSON("heartbeat", "");

	// sending JSON through POST and receiving JSON back.
	$postData = ["myField" => "hasThisValue", "yourField" => "isNotEmpty"];
	$response = $nemAPI->postJSON("post/endpoint", json_encode($postData));

	// The 3rd parameter of the get() and post() methods lets you pass
	// an options array to the RequestHandler. To add specific headers for
	// example you would do as follows:
	$response = $nemAPI->getJSON("hearbeat", "", ["headers" => ["Content-Type" => "text/xml"]]);

	// You may also define onSuccess, onError and onReject callbacks to be executed
	// when the Guzzle Promises respectively complete, encounter an error or are denied.
	// @see Psr\Http\Message\ResponseInterface
	// @see GuzzleHttp\Exception\RequestException
	$response = $nemAPI->getJSON("heartbeat", "", [
		"onSuccess" => function(ResponseInterface $response) {
			echo $response->getBody();
		},
		"onError" => function(RequestException $exception) {
			echo "This is bad: " . $exception->getMessage();
		},
		"onReject" => function($reason) {
			echo "Request could not be completed: " . $reason;
		}
	]);

## Changelog

Important versions listed below. Refer to the [Changelog](CHANGELOG.md) for a full history of the project.

- [0.2.0](CHANGELOG.md) - Ongoing
- [0.1.0](CHANGELOG.md) - 2015-02-04

## License

This software is released under the [MIT](LICENSE) License.

© 2017 Grégory Saive <greg@evias.be>, All rights reserved.
