# Database Notifications

- [Admin Interface](#admin-interface)
- [Database Notifications](#database-notifications)
    - [How Notifications Work](#how-notifications-work)
    - [Admin Operations](#admin-operations)
    - [Actionable Notifications](#actionable-notifications)
    - [Delete Old Notifications](#delete-old-notifications)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

Easily manage [Laravel Database Notifications](https://laravel.com/docs/7.x/notifications#database-notifications) for every user in your application.

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your users' notifications.

You can find the notifications section inside "Admin -> Access Control -> Notifications"   
Feel free to explore all available options this section offers.

![Notifications List](/docs/{{version}}/notifications-list.png)

<a name="database-notifications"></a>
## Database Notifications

<a name="how-notifications-work"></a>
#### How Notifications Work

The notifications section inside the admin panel is just a place from where an admin user can manage [Laravel Database Notifications](https://laravel.com/docs/7.x/notifications#database-notifications).

> The platform doesn't come with any extra logic for database notifications, it just offers an admin section from where to manage them.

Please note that when working with database notifications, you are no more required to follow the [Laravel Prerequisites](https://laravel.com/docs/7.x/notifications#database-prerequisites) section, as the `notifications` table would've already been created if you've installed the platform correctly.

<a name="admin-operations"></a>
#### Admin Operations

As you've probably noticed, inside the admin section for managing notifications, you also have a few available operations, such as: mark notification(s) as read or delete notification(s).

> The admin notification operations are only available when you're viewing your own notifications. Your user will have to be selected in the "Belonging To" filter section.

<a name="actionable-notifications"></a>
#### Actionable Notifications

Through the admin operations available for notifications, you might've noticed there's also an individual button for each notification called "Action". 
When clicking this button, the following will happen:
- your notification will be marked as read
- if your notification contains an `url` key in its json representation, you'll be redirected to that url


<a name="delete-old-notifications"></a>
#### Delete Old Notifications

There are two ways to delete old errors:
- from the admin, by clicking the `Delete Old Notifications` button
- by running the `php artisan varbox:clean-notifications` command

Either of these options are fine, but they're still manual. It's recommended to schedule the `varbox:clean-notifications` command to run periodically, inside your `App\Console\Kernel` class.

```php
/**
 * Define the application's command schedule.
 *
 * @param \Illuminate\Console\Scheduling\Schedule $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
    $schedule->command('varbox:clean-notifications')->monthly();
}
```

The way to determine what notifications are old, is by using the `old_threshold` key from inside the `config/varbox/notifications.php` config file, so feel free to modify that value.

```php
/*
|
| This option accepts an integer, representing the number of days.
|
| Delete notification records older than the number of days supplied when:
| - executing the cli command: "php artisan varbox:clean-notifications"
| - clicking the "Delete Old Notifications" button from the admin panel
|
| If set to "null" or "0", no past notifications will be deleted whatsoever.
|
*/
'old_threshold' => 30,
```






<a name="configuration"></a>
## Configuration

The notifications configuration file is located at `config/varbox/notifications.php`.   
For more information on how you can customize the functionality, read the comments from that file.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
[VarBox](/) makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

<style>
    span.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
    }
</style>


The `notification` classes available for binding overwrites are:

<span class="overwrite-class">Varbox\Controllers\NotificationsController</span>

Found in `config/varbox/bindings.php` at `controllers.notifications_controller` key.   
This class is used for interactions with the Admin -> Access Control -> Notifications section.

<span class="overwrite-class">Varbox\Composers\NotificationsComposer</span>

Found in `config/varbox/bindings.php` at `view_composers.notifications_view_composer` key.   
The view composer for the notification icon in the header.
