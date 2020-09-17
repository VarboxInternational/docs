<h1>Full Backups</h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Configuration File](#configuration-file)
    - [Storage Disk](#storage-disk)
- [Usage](#usage)
    - [Create Backups Periodically](#create-backups-periodically)
    - [Include / Exclude Files For Backup](#include-exclude-files-for-backup)
    - [Specify Databases For Backup](#specify-databases-for-backup)
    - [Save Backups In Multiple Locations](#save-backups-in-multiple-locations)
    - [Delete Old Backups](#delete-old-backups)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The backups component is a very powerful and customizable functionality that allows you to quickly create backups of your application files and database.
</p>

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your settings.

You can find the backups section inside **Admin -> System Settings -> Backups**.   
Feel free to explore all available options this section offers.

![Backups List](/docs/{{version}}/backups-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the workflow & architecture behind backups.

<a name="the-workflow"></a>
#### The Workflow

- Create a new backup from inside the admin panel
- Download your backup and store it somewhere safe

<a name="configuration-file"></a>
#### Configuration File

The configuration file is located at `config/varbox/backup.php`, which actually acts as a wrapper for the [spatie/laravel-backup](https://packagist.org/packages/spatie/laravel-backup) config file.

What values you'll set inside the `config/varbox/backup.php` config file will actually overwrite the values from the [spatie/laravel-backup](https://packagist.org/packages/spatie/laravel-backup) config file.

<a name="storage-disk"></a>
#### Storage Disk

Any created backup will be stored using the `backups` filesystem disk. You can find the disk configuration inside the `config/filesystems.php`, specifically in the `disks` section.

<a name="usage"></a>
## Usage

Let's see how you can create backup checkpoints and leverage their power.

<a name="create-backups-periodically"></a>
#### Create Backups Periodically

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
#### Include / Exclude Files For Backup

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
#### Specify Databases For Backup

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
#### Save Backups In Multiple Locations

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
#### Delete Old Backups

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

The backup configuration file is located at `config/varbox/backup.php`.

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
