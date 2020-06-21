<h1>Activity Log <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [Usage](#usage)
    - [Log Activity Automatically](#log-activity-automatically)
    - [Log Activity Manually](#log-activity-manually)
    - [Fetch Activity Log](#fetch-activity-log)
    - [Enable / Disable Logging](#enable-disable-logging)
    - [Delete Old Activity Logs](#delete-old-activity-logs)
- [Customizations](#customizations)
    - [Attach Url To Log](#attach-url-to-log)
    - [Customize Logged Events](#customize-logged-events)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

Log activity and correlate it to users for any Eloquent model record.    
   
<a name="admin-interface"></a>
## Admin Interface

Before you learn about logging activity inside a Varbox application, you should know that there's already a section in the admin from where you can manage all your activity logs.

You can find the activity log section inside **Admin -> Access Control -> Activity**.   
Feel free to explore all available options this section offers.

![Activity List](/docs/{{version}}/activity-list.png)
   
<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\HasActivity` trait and the `Varbox\Options\ActivityOptions` class. 
The trait contains an abstract method `getActivityOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\ActivityOptions;
use Varbox\Traits\HasActivity;

class YourModel extends Model
{
    use HasActivity;

    /**
     * Set the options for the HasActivity trait.
     *
     * @return ActivityOptions
     */
    public function getActivityOptions(): ActivityOptions
    {
        return ActivityOptions::instance()
            ->withEntityType('Your entity name')
            ->withEntityName($this->name);
    }
}
```

<a name="log-activity-automatically"></a>
#### Log Activity Automatically

Once you've used the `HasActivity` trait in your eloquent models, each time you create / update / delete / etc. a model record, an activity log record will be automatically created.

The activity log record will be stored inside the `activity` database table. 

```php
$model = YourModel::create(...);
// store activity log for creating a model record

$model->update(...);
// store activity log for updating a model record

$model->delete();
// store activity log for deleting a model record
```

<a name="log-activity-manually"></a>
#### Log Activity Manually

You can also log activity manually by using the `logActivity()` method present on the `HasActivity` trait.

```php
$model = YourModel::find($id);
$model->logActivity('created');
```

<a name="fetch-activity-log"></a>
#### Fetch Activity Log

You can fetch a model record's activity log by using the `activity()` morph many relation present on the `Varbox\Traits\HasActivity` trait.

```php
$model = YourModel::find($id);
$activity = $model->activity;
```

<a name="enable-disable-logging"></a>
#### Enable Disable Logging

Logging activity is by default globally enabled. To disable logging activity for your entire application, modify the `LOG_ACTIVITY` environment variable from your `.env` file.
The `LOG_ACTIVITY` environment variable is used inside the `config/varbox/activity.php` config file.

```
LOG_ACTIVITY=false
``` 

You can also individually disable / enable logging activity for a model, in the current request lifecycle.

```php
$model = app(YourModel::class)->doNotLogActivity()->create(...);
// no activity log will be stored

$model->doLogActivity()->update(...);
// an update activity log will be stored
```

<a name="delete-old-activity-logs"></a>
#### Delete Old Activity Logs

There are two ways to delete old activity logs:
- from the admin, by clicking the `Delete Old Activity` button
- by running the `php artisan varbox:clean-activity` command

Either of these options are fine, but they're still manual. It's recommended to schedule the `varbox:clean-activity` command to run daily, inside your `App\Console\Kernel` class.

```php
/**
 * Define the application's command schedule.
 *
 * @param \Illuminate\Console\Scheduling\Schedule $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
    $schedule->command('varbox:clean-activity')->daily();
}
```

The way to determine what activity logs are old, is by using the `old_threshold` key from inside the `config/varbox/activity.php` config file, so feel free to modify that value.

```php
/*
|
| This option accepts an integer, representing the number of days.
|
| This option is used to delete activity records older than the number of days supplied when:
| - executing the cli command: "php artisan varbox:clean-activity"
| - clicking the "Delete Old Activity" button from the admin, inside the activity list view
|
| If set to "null" or "0", no past activities will be deleted whatsoever.
|
*/
'old_threshold' => 30,
```

<a name="customizations"></a>
## Customizations

The activity log functionality offers a variety of customizations to fit your needs.

<a name="attach-url-to-log"></a>
#### Attach Url To Log

You can attach an url to an activity log, in order to point to the entity itself by using the `withEntityUrl()` method in your definition of the `getActivityOptions()` method.   
   
By doing so, when viewing the activity log in the admin, you'll have the possibility to jump to the actual model record the activity was recorded for. For deleted records, the url will become obsolete and won't be available anymore.

```php
/**
 * Set the options for the HasActivity trait.
 *
 * @return ActivityOptions
 */
public function getActivityOptions(): ActivityOptions
{
    return ActivityOptions::instance()
        ->withEntityType('Your entity name')
        ->withEntityName($this->name)
        ->withEntityUrl(route('admin.your_entity.edit', $this->id));
}
```

<a name="customize-logged-events"></a>
#### Customize Logged Events

By default, the activity log will record any changes on the following events:   
`created`, `updated`, `deletd`, `restored`, `revisioned`, `duplicated`

You can change this per your liking by overwriting the `activityEventsToBeLogged()` from the `HasActivity` trait, inside your models.

```php
/**
 * Get the event names that should be recorded.
 * The script will try to record an activity log for each of these events.
 *
 * @return \Illuminate\Support\Collection
 * @throws Exception
 */
public static function activityEventsToBeLogged(): Collection
{
    $events = collect([
        'created', 'updated', 'deleted'
    ]);

    if (collect(class_uses(__CLASS__))->contains(SoftDeletes::class)) {
        $events->push('restored');
    }

    if (collect(class_uses(__CLASS__))->contains(HasRevisions::class)) {
        $events->push('revisioned');
    }

    if (collect(class_uses(__CLASS__))->contains(HasDuplicates::class)) {
        $events->push('duplicated');
    }

    return $events;
}
```

<a name="configuration"></a>
## Configuration

The activity log configuration file is located at `config/varbox/activity.php`.

For more information on how you can customize the activity log functionality, please read the comments from that file.

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

<p class="overwrite-class">Varbox\Models\Activity</p>

Found in `config/varbox/bindings.php` at `models.activity_model` key.   
This class represents the activity log model.

<p class="overwrite-class">Varbox\Controllers\ActivityController</p>

Found in `config/varbox/bindings.php` at `controllers.activity_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Activity" section.

<p class="overwrite-class">Varbox\Filters\ActivityFilter</p>

Found in `config/varbox/bindings.php` at `filters.activity_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\ActivitySort</p>

Found in `config/varbox/bindings.php` at `sorts.activity_sort` key.   
This class is used for applying the sorting logic.
