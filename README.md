# Laravel Paddle

[![Latest Version on Packagist](https://img.shields.io/packagist/v/protonemedia/laravel-paddle.svg?style=flat-square)](https://packagist.org/packages/protonemedia/laravel-paddle)
[![Build Status](https://img.shields.io/travis/protonemedia/laravel-paddle/master.svg?style=flat-square)](https://travis-ci.org/protonemedia/laravel-paddle)
[![Quality Score](https://img.shields.io/scrutinizer/g/protonemedia/laravel-paddle.svg?style=flat-square)](https://scrutinizer-ci.com/g/protonemedia/laravel-paddle)
[![Total Downloads](https://img.shields.io/packagist/dt/protonemedia/laravel-paddle.svg?style=flat-square)](https://packagist.org/packages/protonemedia/laravel-paddle)

## Installation

You can install the package via composer:

```bash
composer require protonemedia/laravel-paddle
```

## Configuration

## Usage

``` php
// Fluent:

$paddlePayLink = Paddle::product()
    ->generatePayLink()
    ->productId($paddlePlanId)
    ->customerEmail($team->owner->email)
    ->passthrough(['team_id' => $team->id])
    ->send();

// Array with payload:

$payload = [
    'product_id' => $paddlePlanId,
    'customer_email' => $team->owner->email,
    'passthrough' => ['team_id' => $team->id],
];

$paddlePayLink = Paddle::product()
    ->generatePayLink($payload)
    ->send();

return Redirect::to($paddlePayLink['url']);

```

## Webhooks and Laravel Events

Configure your webhook URI in the `paddle.php` config file and update your settings here: https://vendors.paddle.com/alerts-webhooks.

Every webhook will be mapped to an Event, for example when the [Subscription Created](https://developer.paddle.com/webhook-reference/subscription-alerts/subscription-created) webhook is called, the request is verified and a `SubscriptionCreated` event will be fired.

Events:
* `ProtoneMedia\LaravelPaddle\NewAudienceMember`
* `ProtoneMedia\LaravelPaddle\SubscriptionCancelled`
* `ProtoneMedia\LaravelPaddle\SubscriptionCreated`
* `ProtoneMedia\LaravelPaddle\SubscriptionPaymentFailed`
* `ProtoneMedia\LaravelPaddle\SubscriptionPaymentRefunded`
* `ProtoneMedia\LaravelPaddle\SubscriptionPaymentSucceeded`
* `ProtoneMedia\LaravelPaddle\SubscriptionUpdated`

Each event contains the payload of the webhook. When you register a listener to handle the event, the payload is easily accessible:

```php
<?php

namespace App\Listeners;

use ProtoneMedia\LaravelPaddle\Events\SubscriptionCreated;

class CreateSubscriptionModel
{
    public function handle(SubscriptionCreated $event)
    {
        $status = $event->status;

        $nextBillDate = $event->next_bill_date;
    }
}
```

## Blade directive

This directive imports the Paddle JavaScript library and configures it with your Vendor ID.

```php
<body>
    {{-- your app --}}

    @paddle
</body>
```

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email pascal@protone.media instead of using the issue tracker.

## Credits

- [Pascal Baljet](https://github.com/protonemedia)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.