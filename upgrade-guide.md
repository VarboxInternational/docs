# Upgrade Guide

- [Upgrade From 1.x To 2.x](#upgrade-from-1x-to-2x)

<a name="upgrade-from-1x-to-2x"></a>
## Upgrade From 1.x To 2x.

Estimated time: **10 minutes**

> **Important!** First, apply the [Laravel upgrades from 7.x to 8.x](https://laravel.com/docs/8.x/upgrade)

#### Update Composer

Update your `composer.json` version constraint for `varbox/varbox` from `^1.0` to `^2.0`

```
"varbox/varbox": "^2.0"
```

#### User Model Directory

As per Laravel's upgrade guide to 8.x, place your `App\User` model inside the `app/Models` directory.   
Also, change the namespace from `App\User` to `App\Models\User`

#### Seeders Directory & Namespace

Rename the `database/seeds` directory into `database/seeders`.   
Also, all your seeder classes should have the `Database\Seeders` namespace.

<a name="install-varbox-2x"></a>
#### Install Varbox 2.x

Install the Varbox platform by running one simple command:

```
php artisan varbox:install
```
