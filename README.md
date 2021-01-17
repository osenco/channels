# Bonga notification channel for Laravel

[![Latest Version on Packagist](https://img.shields.io/packagist/v/laravel-notification-channels/bonga.svg?style=flat-square)](https://packagist.org/packages/laravel-notification-channels/bonga)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![StyleCI](https://styleci.io/repos/247548130/shield)](https://styleci.io/repos/209406724)
[![Quality Score](https://img.shields.io/scrutinizer/g/laravel-notification-channels/bonga.svg?style=flat-square)](https://scrutinizer-ci.com/g/laravel-notification-channels/bonga)
[![Total Downloads](https://img.shields.io/packagist/dt/laravel-notification-channels/bonga.svg?style=flat-square)](https://packagist.org/packages/laravel-notification-channels/bonga)

This package makes it easy to send notifications using [Bonga](https://build.at-labs.io/docs/sms%2Fsending) with Laravel.

## Contents

- [About](#about)
- [Installation](#installation)
- [Setting up the Bonga service](#setting-up-the-bonga-service)
- [Usage](#usage)
- [Testing](#testing)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)

## About

This package is part of the [Laravel Notification Channels](http://laravel-notification-channels.com/) project. It provides additional Laravel Notification channels to the ones given by [Laravel](https://laravel.com/docs/master/notifications) itself.

The Bonga channel makes it possible to send out Laravel notifications as a `SMS ` using Bonga API.

## Installation

You can install this package via composer:

``` bash
composer require laravel-notification-channels/bonga
```

The service provider gets loaded automatically.

### Setting up the Bonga Service

You will need to [Register](https://account.bonga.com/auth/register/) and then go to your sandbox app [Go To SandBox App](https://account.bonga.com/apps/sandbox). [Click on settings](https://account.bonga.com/apps/sandbox/settings/key) Within this page, you will generate your `Client and key`. Place them inside your `.env` file. Remember to add your Sender ID that you will be using to send the messages. 

```bash
BONGA_CLIENT=""
BONGA_KEY=""
BONGA_SECRET=""
```

To load them, add this to your `config/services.php` . This will load the Bonga  data from the `.env` file.file:

```php
'bonga' => [
    'client'        => env('BONGA_CLIENT'),
    'key'           => env('BONGA_KEY'),
    'secret'        => env('BONGA_SECRET'),
]
```

Add the `routeNotifcationForBonga` method on your notifiable Model. If this is not added,
the `phone` field will be automatically used.  

```php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * Route notifications for the Africas Talking channel.
     *
     * @param  \Illuminate\Notifications\Notification  $notification
     * @return string
     */
    public function routeNotificationForBonga($notification)
    {
        return $this->phone;
    }
}
```


## Usage


To use this package, you need to create a notification class, like `SendOtp` from the example below, in your Laravel application. Make sure to check out [Laravel's documentation](https://laravel.com/docs/master/notifications) for this process.


```php
<?php

use NotificationChannels\Bonga\BongaChannel;
use NotificationChannels\Bonga\BongaMessage;

class SendOtp extends Notification
{

    /**
     * Get the notification's delivery channels.
     *
     * @param  mixed  $notifiable
     * @return array
     */
    public function via($notifiable)
    {
        return [BongaChannel::class];
    }

    public function toBonga($notifiable)
    {
		return (new BongaMessage())
                    ->content('Your SMS message content');

    }
}
```


## Testing

``` bash
$ composer test
```

## Security

If you discover any security-related issues, please email osaigbovoemmanuel1@gmail.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Osaigbovo Emmanuel](https://github.com/ossycodes)
- [Osen Concepts](https://github.com/osenco)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## How do I say Thank you?

Leave a star and follow me on [Twitter](https://twitter.com/ossycodes) .
