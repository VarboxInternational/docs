# Upgrade Guide

- [Upgrade From 1.x To 2.x](#upgrade-from-1x-to-2x)
- [Upgrade From 2.x Free To 2.x Paid](#upgrade-from-2x-free-to-2x-paid)

<a name="upgrade-from-1x-to-2x"></a>
## Upgrade From 1x To 2x

Estimated time: **5 minutes**

> **Important!**   
> First, apply the [Laravel upgrades from 7.x to 8.x](https://laravel.com/docs/8.x/upgrade)

<a name="download-2x-version"></a>
#### Download 2.x Version

<p id="first-p">
Go to the <a href="/releases">downloads</a> section and download the 2.x version of the platform (free or paid).   
</p>   

<a name="replace-2x-source-code"></a>
#### Replace Source Code

Inside your project, delete everything you have inside the `varbox` directory. After that, unzip the downloaded archive for the `2x` version and place its contents inside your empty `varbox` directory within your application's root directory.

> When deleting or unzipping Varbox into your application's `varbox` directory, please make sure all of Varbox's "hidden" files (such as `.gitignore`) are included.

<a name="install-varbox-2x"></a>
#### Install Varbox 2.x

Install the Varbox platform by running one simple command:

```
php artisan varbox:install
```

<a name="upgrade-from-2x-free-to-2x-paid"></a>
## Upgrade From 2.x Free To 2.x Paid

Estimated time: **15 minutes**

<a name="download-paid-version"></a>
#### Download Paid Version

<p id="first-p">
Go to the site and <a href="/buy">buy</a> a Varbox license.<br />
Go to the <a href="/releases">downloads</a> section and download the paid 1.x version of the platform.   
</p>    

<a name="replace-source-code"></a>
#### Replace Source Code

Inside your project, delete everything you have inside the `varbox` directory. After that, unzip the downloaded archive for the `paid` version and place its contents inside your empty `varbox` directory within your application's root directory.

> When deleting or unzipping Varbox into your application's `varbox` directory, please make sure all of Varbox's "hidden" files (such as `.gitignore`) are included.

<a name="replace-seeder-classes"></a>
#### Replace Seeder Classes

Delete your already existing `UsersSeeder.php`, `PermissionsSeeder.php`, `RolesSeeder.php` and `VarboxSeeder.php` files from inside your `database/seeders` directory. 
The reason for this is that new seeder classes will be generated for the `paid` version.

> If you've already modified these seeder classes while using the `free` version, then you should make a backup of the files and paste your code inside the newly generated seeder classes for the `paid` version.

<a name="update-admin-menu"></a>
#### Update Admin Menu

Delete your already existing `AdminMenuComposer.php` file from inside your `app/Http/Composers` directory. 
The reason for this is that a new menu will be generated containing the menu items for the `paid` version.

> If you've already modified your `AdminMenuComposer.php` while using the `free` version, then you should make a backup of this file and paste your code inside the newly generated file for the `paid` version.

<a name="update-users-blade"></a>
#### Update Users Blade

Modify the `resources/views/vendor/varbox/admin/users/_form.blade.php` file and add the following just above the action buttons from the bottom:

```php
...

@if($item->exists)
    @permission('addresses-list')
        <div class="col-12">
            <div class="card">
                <div class="card-status bg-yellow"></div>
                <div class="card-header border-0 py-5">
                    <h3 class="card-title">Addresses</h3>

                    <a href="{{ route('admin.addresses.index', $item->getKey()) }}" class="btn btn-yellow btn-square float-right ml-auto">
                        <i class="fe fe-eye mr-2"></i>View User's Addresses
                    </a>
                </div>
            </div>
        </div>
    @endpermission
@endif

...
```

<a name="install-varbox"></a>
#### Install Varbox

Install the Varbox platform by running one simple command:

```
php artisan varbox:install
```

Migrate your database with the newly created Varbox migration file:

```
php artisan migrate
```

Seed your database with Varbox specific data:

```
php artisan db:seed --class="VarboxSeeder"
```



