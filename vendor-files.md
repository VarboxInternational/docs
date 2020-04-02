# Vendor Files

- [Config Files](#config-files)
- [Blade Views](#blade-views)
- [Public Assets](#public-assets)
- [Migration & Seeds](#migration-seeds)

<a name="config-files"></a>
## Config Files

All of the configuration files are stored in the `config/varbox` directory. 

> You are strongly encouraged to read the comments inside those config files, as they provide useful insight on how to use the respective functionalities.

<a name="publish-configs"></a>
#### Publish Configs

All config files were automatically published when you ran:

```
php artisan varbox:install
``` 

If you didn't run the above command, you can publish your config files, by running:

```
php artisan vendor:publish --tag=varbox-config
```

<a name="configs-important-note"></a>
#### Important Note

<p style="color: #DD7467;">
    Re-publishing the config files after you already modified them, won't overwrite your changes.
</p>

<a name="blade-views"></a>
## Blade Views

All view files are stored in the `resources/views/vendor/varbox` directory.   
This directory contains all blade views used in the `admin panel`.   

> You are free to modify any of these views to match your needs.

The platform uses <a href="https://getbootstrap.com/docs/4.1" target="_blank">Bootstrap 4</a> so it should be fairly easy to modify the views.

> If you won't ever need to modify the platform views, you can also remove that directory, in which case the views will be loaded from the package.

<a name="publish-views"></a>
#### Publish Views

The view files were automatically published when you ran:

```
php artisan varbox:install
``` 

If you didn't run the above command, you can publish your view files, by running:

```
php artisan vendor:publish --tag=varbox-views
```

<a name="views-important-note"></a>
#### Important Note

<p style="color: #DD7467;">
    Re-publishing the view files after you already modified them, won't overwrite your changes.
</p>

<a name="public-assets"></a>
## Public Assets

All of the asset files are stored in the `public/vendor/varbox` directory.   

> If you're using a vcs tool you should **NOT** ignore this directory.

All assets published by the platform are only used for styling the `admin panel`. You're not required to include any css or js in your frontend.

> The asset files are already compiled using <a href="https://laravel.com/docs/7.x/mix" target="_blank">Laravel Mix</a>.   

<a name="publish-assets"></a>
#### Publish Assets

All assets were automatically published when you ran:

```
php artisan varbox:install
``` 

If you didn't run the above command, you can publish your assets, by running:

```
php artisan vendor:publish --tag=varbox-public
```

<a name="assets-important-note"></a>
#### Important Note

<p style="color: #DD7467; margin-bottom: 0;">
    If you wish to overwrite frontend behavior for the admin panel, please create your own files.
</p>

You can then include your css or js custom files inside:
- `resources/views/vendor/varbox/layouts/admin/partials/_head.blade.php`
- `resources/views/vendor/varbox/layouts/admin/partials/_footer.blade.php` 

<a name="migration-seeds"></a>
## Migration & Seeds

In order not to crowd your `database/migrations` directory, only one `_create_varbox_tables.php` migration file is published, responsible for creating all necessary tables.

> You should check that migration file and make sure you don't already use tables with the same name in your application.

<a name="publish-migration"></a>
#### Publish Migration

The migration file was automatically published when you ran:

```
php artisan varbox:install
``` 

If you didn't run the above command, you can publish your migration file, by running:

```
php artisan vendor:publish --tag=varbox-migrations
```

<a name="the-seeders"></a>
#### The Seeders

In addition to the migration file, a few database seeders are available for you out of the box.   

> No seeder will tamper with your already existing data, because a check is automatically performed to see if the record already exists, before creating it.

The seeders were automatically executed when you ran:

```
php artisan varbox:install
``` 

If you didn't run the above command, you can execute the seeders, by running:

```
// create all crud permissions
php artisan db:seed --class="Varbox\Seed\PermissionsSeeder"

// create admin & super roles
php artisan db:seed --class="Varbox\Seed\RolesSeeder"

// create the initial admin user
php artisan db:seed --class="Varbox\Seed\UsersSeeder"

// create all known countries
php artisan db:seed --class="Varbox\Seed\CountriesSeeder"

// create all known languages
php artisan db:seed --class="Varbox\Seed\LanguagesSeeder"

// create the analytics code record
php artisan db:seed --class="Varbox\Seed\AnalyticsSeeder"
```
