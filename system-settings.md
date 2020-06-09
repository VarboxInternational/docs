# System Settings

- [Admin Interface](#admin-interface)
- [Custom Configs](#custom-configs)
    - [How Configs Work](#how-configs-work)
    - [Specify Available Config Keys](#specify-available-config-keys)
    - [Overwrite Config Values](#overwrite-config-values)
- [Application Redirects](#application-redirects)
    - [How Redirects Work](#how-redirects-work)
    - [Handle Redirects Via Middleware](#handle-redirects-via-middleware)
    - [Handle Redirects Via PHP File](#handle-redirects-via-php-file)
- [Application Errors](#application-errors)
    - [How Errors Work](#how-errors-work)
    - [Enable Error Logging](#enable-error-logging)
    - [Ignore Specific Errors](#ignore-specific-errors)
    - [Error Email Notification](#error-email-notification)
    - [Delete Old Errors](#delete-old-errors)
- [Application Backups](#application-backups)
    - [How Backups Work](#how-backups-work)
    - [Create Backups Periodically](#create-backups-periodically)
    - [Include / Exclude Files For Backup](#include-exclude-files-for-backup)
    - [Specify Databases For Backup](#specify-databases-for-backup)
    - [Save Backups In Multiple Locations](#save-backups-in-multiple-locations)
    - [Delete Old Backups](#delete-old-backups)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The system settings module is a powerful tool that offers you the following features out of the box:
- dynamically customize values for your configuration keys
- manage your redirects with a smart cascade update logic
- supervize your application's errors and receive email notifications
- backup your application files and database

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your settings.

<a name="configs-interface"></a>
#### Configs Interface

You can find the configs section inside **Admin -> System Settings -> Configs**.   
Feel free to explore all available options this section offers.

![Configs List](/docs/{{version}}/configs-list.png)

<a name="redirects-interface"></a>
#### Redirects Interface

You can find the redirects section inside **Admin -> System Settings -> Redirects**.   
Feel free to explore all available options this section offers.

![Redirects List](/docs/{{version}}/redirects-list.png)

<a name="errors-interface"></a>
#### Errors Interface

You can find the errors section inside **Admin -> System Settings -> Errors**.   
Feel free to explore all available options this section offers.

![Errors List](/docs/{{version}}/errors-list.png)

<a name="backups-interface"></a>
#### Backups Interface

You can find the backups section inside **Admin -> System Settings -> Backups**.   
Feel free to explore all available options this section offers.

![Backups List](/docs/{{version}}/backups-list.png)

<a name="custom-configs"></a>
## Custom Configs

The configs component is a very powerful functionality that allows you to quickly overwrite static config values in your application.

In order to provide you with a complex crud functionality inside the admin, the configs crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 4; -moz-column-count: 4; -webkit-column-count: 4;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-configs-work"></a>
### How Configs Work

First of all, let's understand the workflow behind configs.

<a name="the-configs-workflow"></a>
#### The Workflow

- Enable config overwrites by updating your .env variable
- Define the config keys that can be overwritten from the admin
- Create custom config values for your defined config keys
- Apply the middleware to actually modify the config values at runtime

<a name="specify-available-config-keys"></a>
### Specify Available Config Keys

By default, if you were to try to add a config from inside the admin panel, you'll notice that you don't have any values to specify for the `Key` select input, which is required.

The platform isn't capable of overwriting any config values by default. This is intentional, in order not to allow a client to accidentally overwrite a config value that isn't meant for overwriting.

> Only config keys specified by the developer will be available for overwrite inside the admin.

To allow config keys to be overwritten, you should specify them inside the `config/varbox/config.php` config file, specifically inside the `keys` section.

```php
'keys' => [
    'app.name',
    'app.url',
    'app.locale',
],
```

After you've allowed the config keys, they'll appear in the admin when trying to add a custom config.

<a name="overwrite-config-values"></a>
### Overwrite Config Values

In order for the created config values from the admin to replace your default ones, inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\OverwriteConfigs` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\OverwriteConfigs::class
    ],

    ...
];
```

That's it! Now try and access a config whose value has been dynamically modified from the admin.

```php
config('app.name'); // returns your custom value from the admin
```

<a name="application-redirects"></a>
## Application Redirects

The redirects component is a very powerful functionality that allows you to visually manage your request redirects.

In order to provide you with a complex crud functionality inside the admin, the redirects crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 4; -moz-column-count: 4; -webkit-column-count: 4;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-redirects-work"></a>
### How Redirects Work

First of all, let's understand the workflow behind redirects.

<a name="the-configs-workflow"></a>
#### The Workflow

- Create a redirect from inside the admin panel
- Make your application aware of the created redirects in order to handle them
- You can handle the redirects via a middleware or a php file

<a name="handle-redirects-via-middleware"></a>
### Handle Redirects Via Middleware

Once you have your redirects setup inside the admin, it's time to make the application aware of them and actually redirect incoming requests when needed.

Inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\RedirectRequests` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\RedirectRequests::class
    ],

    ...
];
```

<a name="handle-redirects-via-php-file"></a>
### Handle Redirects Via PHP File

If you don't like the middleware approach because for example it creates between one and two queries every request, you can still leverage the redirect capability, by hooking into the `public/index.php` file.

<a name="export-redirects-to-file"></a>
#### Export Redirects To File

First of all, inside the "Admin -> System Settings -> Redirects", click on the "Export File" button.

This will create the `bootstrap/redirects.php` file containing all you redirects in an array format. 

> If you're trying to "Export File" an empty redirect list, the actual `bootstrap/redirects.php` will be deleted, as it won't be any need for it.

<a name="add-the-redirection-code"></a>
#### Add The Redirection Code 

Once you've exported your redirects, go inside the `public/index.php` file and at the very beginning add:

```php
if (file_exists(__DIR__ . '/../bootstrap/redirects.php')) {
    foreach (require_once __DIR__ . '/../bootstrap/redirects.php' as $redirect) {
        if ($_SERVER['REQUEST_URI'] == '/' . trim($redirect['from'], '/')) {
            header('Location: ' . '/' . trim($redirect['to'], '/'), true, $redirect['status']);
            die;
        }
    }
}
```

This code fragment will handle your redirects before even hitting the Laravel application.

<a name="automate-the-process"></a>
#### Automate The Process

To automate the process of exporting the redirects every time a redirect is created, updated or deleted, set the `automatic_export` key to `true` inside the `config/varbox/redirect.php` config file.

```php
/*
|
| Export the redirects into the "bootstrap/redirects.php" on every create / update / delete.
|
*/
'automatic_export' => true
```

<a name="application-errors"></a>
## Application Errors

The errors component is a very powerful functionality that allows you to have an overview of the errors occurred inside your application, with the possibility of being notified via email.

In order to provide you with a complex crud functionality inside the admin, the errors crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)

</div>

<a name="how-errors-work"></a>
### How Errors Work

First of all, let's understand the workflow behind errors.

<a name="the-errors-workflow"></a>
#### The Workflow

- An error occurred inside your application
- The custom error handler stores a database entry for the error
- You are immediately notified about the error occurred (optional)

<a name="enable-error-logging"></a>
### Enable Error Logging

By default, no errors occurred inside your application will be logged in the admin panel.   
To enable this, modify your `.env` file with the following:

```
SAVE_ERRORS=true
``` 

<a name="ignore-specific-errors"></a>
### Ignore Specific Errors

You can ignore certain errors from being logged when they occur, by adding their fully qualified namespaces inside the `config/varbox/errors.php`, specifically in the `ignore_errors` key.

```php
/*
|
| Here you can specify any exceptions that you don't want to be saved/emailed.
| You can specify which exceptions to ignore by adding their FQNs in an array format.
|
*/
'ignore_errors' => [
    '\Symfony\Component\HttpKernel\Exception\NotFoundHttpException',
    '\Facade\Ignition\Exceptions\ViewException',
    '\Symfony\Component\ErrorHandler\Error\FatalError',
],
```

<a name="error-email-notification"></a>
### Error Email Notification

By default no email notifications are triggered when an error is logged, but you can enable this for your desired email addresses by adding them inside the `config/varbox/errors.php`, specifically in the `notification_emails` key.

```php
/*
|
| You can be notified via email when an error has occurred in the system.
| Specify any email addresses and each address will receive the notifications.
|
*/
'notification_emails' => [
    'you@mail.com',
    'other@mail.com',
]
```

Once you've done this, behind the scenes the `Varbox\Mail\ErrorSavedMail` mailable will be used for sending the emails. 
You can also customize the email's contents by modifying the `resources/views/vendor/varbox/emails/error_saved.blade.php` blade file.

<a name="delete-old-errors"></a>
### Delete Old Errors

There are two ways to delete old errors:
- from the admin, by clicking the `Delete Old Errors` button
- by running the `php artisan varbox:clean-errors` command

Either of these options are fine, but they're still manual. It's recommended to schedule the `varbox:clean-errors` command to run daily, inside your `App\Console\Kernel` class.

```php
/**
 * Define the application's command schedule.
 *
 * @param \Illuminate\Console\Scheduling\Schedule $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
    $schedule->command('varbox:clean-errors')->daily();
}
```

The way to determine what errors are old, is by using the `old_threshold` key from inside the `config/varbox/errors.php` config file, so feel free to modify that value.

```php
/*
|
| This option accepts an integer, representing the number of days.
|
| This option is used to delete error records older than the number of days supplied when:
| - executing the cli command: "php artisan varbox:clean-errors"
| - clicking the "Delete Old Errors" button from the admin panel, inside the error list view
|
| If set to "null" or "0", no past errors will be deleted whatsoever.
|
*/
'old_threshold' => 30,
```

<a name="application-backups"></a>
## Application Backups

The backups component is a very powerful and customizable functionality that allows you to quickly create backups of your application files and database.

> The backups component uses the [spatie/laravel-backup](https://packagist.org/packages/spatie/laravel-backup) behind the scenes, so make sure you read their documentation first, in order to fully understand our wrapper around it.

In order to provide you with a complex crud functionality inside the admin, the backups crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 4; -moz-column-count: 4; -webkit-column-count: 4;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-backups-work"></a>
### How Backups Work

First of all, let's understand the workflow & architecture behind backups.

<a name="the-errors-workflow"></a>
#### The Workflow

- Create a new backup from inside the admin panel
- Download your backup and store it somewhere safe

<a name="the-backup-configuration"></a>
#### The Configuration

The configuration file is located at `config/varbox/backup.php`, which actually acts as a wrapper for the [spatie/laravel-backup](https://packagist.org/packages/spatie/laravel-backup) config file.

What values you'll set inside the `config/varbox/backup.php` config file will actually overwrite the values from the [spatie/laravel-backup](https://packagist.org/packages/spatie/laravel-backup) config file.

<a name="the-backup-storage-disk"></a>
#### The Storage Disk

Any created backup will be stored using the `backups` filesystem disk. You can find the disk configuration inside the `config/filesystems.php`, specifically in the `disks` section.

<a name="create-backups-periodically"></a>
### Create Backups Periodically

The backups are created using the `php artisan backup:run` command. In order to automatically create backups periodically, you can schedule that command inside your `App\Console\Kernel` class.

```php
/**
 * Define the application's command schedule.
 *
 * @param \Illuminate\Console\Scheduling\Schedule $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
    $schedule->command('backup:run')->daily();
}
```

<a name="include-exclude-files-for-backup"></a>
### Include / Exclude Files For Backup

You can specify what files to be included or excluded when backing up the application's source code from inside the `config/vabox/backup.php` config file.

```php
/*
|
| The list of directories and files that will be included in the backup.
|
*/
'include' => [
    base_path(),
],

/*
| These directories and files will be excluded from the backup.
| Directories used by the backup process will automatically be excluded.
*/
'exclude' => [
    base_path('vendor'),
    base_path('node_modules'),
],
```

<a name="specify-databases-for-backup"></a>
### Specify Databases For Backup

You can specify what databases to be backed up with your application files from inside the `config/varbox/backup.php` config file.

```php
/*
|
| The names of the connections to the databases that should be backed up.
| MySQL, PostgreSQL, SQLite and Mongo databases are supported.
|
*/
'databases' => [
    'mysql',
],
```

You can find the database connection names inside the `config/database.php` config file, specifically in the `connections` section.

<a name="save-backups-in-multiple-locations"></a>
### Save Backups In Multiple Locations

By default all backups are stored using the `backups` disk, but you can store your backups in multiple places at once.
To do so, update the `disks` section inside the `config/varbox/backup.php` config file.

```php
/*
|
| The disk names on which the backups will be stored.
|
*/
'disks' => [
    'backups',
    'local',
    's3',
],
```

<a name="delete-old-backups"></a>
### Delete Old Backups

There are two ways to delete old backups:
- from the admin, by clicking the `Delete Old Backups` button
- by running the `php artisan varbox:clean-backups` command

Either of these options are fine, but they're still manual. It's recommended to schedule the `varbox:clean-backups` command to run daily, inside your `App\Console\Kernel` class.

```php
/**
 * Define the application's command schedule.
 *
 * @param \Illuminate\Console\Scheduling\Schedule $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
    $schedule->command('varbox:clean-backups')->daily();
}
```

The way to determine which backups are old, is by using the `old_threshold` key from inside the `config/varbox/backup.php` config file, so feel free to modify that value.

```php
/*
|
| This option accepts an integer, representing the number of days.
|
| This option is used to delete backups older than the number of days supplied when:
| - executing the cli command: "php artisan varbox:clean-backups"
| - clicking the "Delete Old Backups" button from the admin, inside the backups list view
|
| If set to "null" or "0", no past activities will be deleted whatsoever.
|
*/
'old_threshold' => 30,
```

<a name="configuration"></a>
## Configuration

For more information on how you can customize the system settings components, please read the comments from their configuration files.

<a name="configs-configuration"></a>
#### Configs Configuration

The config configuration file is located at `config/varbox/config.php`.

<a name="redirects-configuration"></a>
#### Redirects Configuration

The redirect configuration file is located at `config/varbox/redirect.php`.

<a name="errors-configuration"></a>
#### Errors Configuration

The error configuration file is located at `config/varbox/errors.php`.

<a name="backups-configuration"></a>
#### Backups Configuration

The backup configuration file is located at `config/varbox/backup.php`.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
Varbox makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

<style>
    p.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 15px;
        margin: 0;
    }
</style>

<a name="config-bindings"></a>
### Config Bindings

The config classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Config</p>

Found in `config/varbox/bindings.php` at `models.config_model` key.   
This class represents the config model.

<p class="overwrite-class">Varbox\Controllers\ConfigsController</p>

Found in `config/varbox/bindings.php` at `controllers.configs_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Configs" section.

<p class="overwrite-class">Varbox\Requests\ConfigRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.config_form_request` key.   
This class is used for validating any config when creating or updating.

<p class="overwrite-class">Varbox\Middleware\OverwriteConfigs</p>

Found in `config/varbox/bindings.php` at `middleware.overwrite_configs_middleware` key.   
This is the middleware modifying your config keys with the values assigned from the admin.

<p class="overwrite-class">Varbox\Filters\ConfigFilter</p>

Found in `config/varbox/bindings.php` at `filters.config_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\ConfigSort</p>

Found in `config/varbox/bindings.php` at `sorts.config_sort` key.   
This class is used for applying the sorting logic.

<a name="redirect-bindings"></a>
### Redirect Bindings

The redirect classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Redirect</p>

Found in `config/varbox/bindings.php` at `models.redirect_model` key.   
This class represents the redirect model.

<p class="overwrite-class">Varbox\Controllers\RedirectsController</p>

Found in `config/varbox/bindings.php` at `controllers.redirects_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Redirects" section.

<p class="overwrite-class">Varbox\Requests\RedirectRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.redirect_form_request` key.   
This class is used for validating any redirect when creating or updating.

<p class="overwrite-class">Varbox\Middleware\RedirectRequests</p>

Found in `config/varbox/bindings.php` at `middleware.redirect_requests_middleware` key.   
This middleware can be used by you to manage your redirects at application level.

<p class="overwrite-class">Varbox\Filters\RedirectFilter</p>

Found in `config/varbox/bindings.php` at `filters.redirect_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\RedirectSort</p>

Found in `config/varbox/bindings.php` at `sorts.redirect_sort` key.   
This class is used for applying the sorting logic.

<a name="block-bindings"></a>
### Error Bindings

The error classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Error</p>

Found in `config/varbox/bindings.php` at `models.error_model` key.   
This class represents the error model.

<p class="overwrite-class">Varbox\Controllers\ErrorsController</p>

Found in `config/varbox/bindings.php` at `controllers.errors_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Errors" section.

<p class="overwrite-class">Varbox\Filters\ErrorFilter</p>

Found in `config/varbox/bindings.php` at `filters.error_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\ErrorSort</p>

Found in `config/varbox/bindings.php` at `sorts.error_sort` key.   
This class is used for applying the sorting logic.

<a name="backup-bindings"></a>
### Backup Bindings

The backup classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Error</p>

Found in `config/varbox/bindings.php` at `models.backup_model` key.   
This class represents the backup model.

<p class="overwrite-class">Varbox\Controllers\BackupsController</p>

Found in `config/varbox/bindings.php` at `controllers.backups_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Backups" section.

<p class="overwrite-class">Varbox\Filters\BackupFilter</p>

Found in `config/varbox/bindings.php` at `filters.backup_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\BackupSort</p>

Found in `config/varbox/bindings.php` at `sorts.backup_sort` key.   
This class is used for applying the sorting logic.
