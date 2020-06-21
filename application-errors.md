<h1>Application Errors <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
- [Usage](#usage)
    - [Enable Error Logging](#enable-error-logging)
    - [Ignore Specific Errors](#ignore-specific-errors)
    - [Error Email Notification](#error-email-notification)
    - [Delete Old Errors](#delete-old-errors)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your settings.

You can find the errors section inside **Admin -> System Settings -> Errors**.   
Feel free to explore all available options this section offers.

![Errors List](/docs/{{version}}/errors-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the workflow behind errors.

<a name="the-workflow"></a>
#### The Workflow

- An error occurred inside your application
- The custom error handler stores a database entry for the error
- You are immediately notified about the error occurred (optional)

<a name="usage"></a>
## Usage

Let's see how you can enable error logging and leverage its power.

<a name="enable-error-logging"></a>
#### Enable Error Logging

By default, no errors occurred inside your application will be logged in the admin panel.   
To enable this, modify your `.env` file with the following:

```
SAVE_ERRORS=true
``` 

<a name="ignore-specific-errors"></a>
#### Ignore Specific Errors

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
#### Error Email Notification

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
#### Delete Old Errors

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

<a name="configuration"></a>
## Configuration

The error configuration file is located at `config/varbox/errors.php`.

For more information on how you can customize the system settings components, please read the comments from their configuration files.

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
