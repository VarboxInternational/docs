# Installation

- [Download & Setup](#download-and-setup)
- [Automatic Installation](#automatic-installation)
- [Manual Installation](#manual-installation)
    - [Publish Vendor Files](#publish-vendor-files)
    - [Write Env Variables](#write-env-variables)
    - [Register Routes](#register-routes)
    - [Update Configurations](#update-configurations)
    - [Update Existing Files](#update-existing-files)
    - [Copy SQL Files](#copy-sql-files)
    - [Copy Seeder Classes](#copy-seeder-classes)
    - [Migrate Database](#migrate-database)
    - [Seed Database](#seed-database)
    - [Symlink Directories](#symlink-directories)
    - [Create New Files](#create-new-files)
    - [Overwrite Bindings](#overwrite-bindings)
    
> In order for the following installation guide to work, you'll need to have a working <a href="https://laravel.com/docs/7.x/installation" target="_blank" rel="noreferrer">Laravel 7</a> application and a database created and configured in your `.env` file.

<a name="download-and-setup"></a>
## Download & Setup

<p id="first-p">
After you've signed into your account, go to the <a href="/releases">Downloads</a> section of the Varbox website and download a <strong>FREE</strong> or <strong>PAID</strong> version of the software.
</p>

After downloading a zip file containing the Varbox source code, you will need to install it as a Composer "path" repository within your Laravel application's `composer.json` file. 
Unzip the contents of the Varbox release into a `varbox` directory within your application's root directory. 

> **Hidden Files**
> 
> When unzipping Varbox into your application's `varbox` directory, please make sure all of Varbox's "hidden" files (such as `.gitignore`) are included.

Once you have unzipped and placed the Varbox source code within the appropriate directory, you are ready to update your `composer.json` file. 
You should add the following configuration to the file:

```
"repositories": [
    {
        "type": "path",
        "url": "./varbox"
    }
],
```

Next, add `varbox/varbox` to the `require` section of your `composer.json` file:

```
"require": {
    "php": "^7.2.5",
    "laravel/framework": "^7.0",
    "varbox/varbox": "*"
},
```

After you've updated your `composer.json` file, run the following command in your terminal:

```
composer update
```

> **Package Stability**
> 
> If you are not able to install Varbox into your application because of your `minimum-stability` setting, consider setting your `minimum-stability` option to `dev` and your `prefer-stable` option to `true`. 
> This will allow you to install Varbox while still preferring stable package releases for your application.

<a name="automatic-installation"></a>
## Automatic Installation 

Install the Varbox platform by running  one simple command:

```
php artisan varbox:install
```

Migrate your database with the newly created Varbox migration file:

```
php artisan migrate
```

Seed your database with Varbox specific data.    
For an overview of what data is seeded, take a look inside `database/seeds/VarboxSeeder.php`

```
php artisan db:seed --class="VarboxSeeder"
```

#### That's it!

Now you can sign into your admin panel by accessing `/admin`.   
Use `admin@mail.com / admin` to authenticate.

<a name="manual-installation"></a>
## Manual Installation

> The following guide only works for the `paid` version of the software.   
> You can also follow along using the free version, but you'll have to exclude some steps.

If for some reason you don't want to automatically install the Varbox platform, you can do so manually, by following the steps below.

> The following steps also provide useful insight on what the `varbox:install` command does. 

<a name="publish-vendor-files"></a>
#### Publish Vendor Files

Run the following artisan commands to publish all platform specific files:

```
php artisan vendor:publish --tag=varbox-config   
php artisan vendor:publish --tag=varbox-migrations
php artisan vendor:publish --tag=varbox-views
php artisan vendor:publish --tag=varbox-public
```

<a name="write-env-variables"></a>
#### Write Env Variables

Append the following to your `.env` file:

```
CACHE_ALL_QUERIES=false
CACHE_DUPLICATE_QUERIES=false
LOG_ACTIVITY=false
SAVE_ERRORS=false
```

<a name="register-routes"></a>
#### Register Routes

In your `routes/web.php` file, at the very bottom, add the following:

```
// this should be the last line
Route::varbox();
```

<a name="update-configurations"></a>
#### Update Configurations

In your `config/auth.php` config file add the following to the `guards` section:

```
'guards' => [
    ...
    
    'admin' => [
        'driver' => 'session',
        'provider' => 'users',
    ],
],
```

In your `config/filesystems.php` config file add the following to the `disks` section:

```
'disks' => [
    ...
    
    'uploads' => [
        'driver' => 'local',
        'root' => storage_path('uploads'),
        'url' => env('APP_URL').'/uploads',
        'visibility' => 'public',
    ],

    'wysiwyg' => [
        'driver' => 'local',
        'root' => storage_path('wysiwyg'),
        'url' => env('APP_URL').'/wysiwyg',
        'visibility' => 'public',
    ],

    'backups' => [
        'driver' => 'local',
        'root' => storage_path('backups'),
    ],
    
],
```

<a name="update-existing-files"></a>
#### Update Existing Files

In your `app/User.php` file change the extended class to reference:

```
use Varbox\Models\User as VarboxUser;

class User extends VarboxUser {
    ...
}
```

In your `app/User.php` file append the following to the `$fillable` property:

```
protected $fillable = [
    ...
    'active',
];
```

In your `app/User.php` file append the following to the `$casts` property:

```
protected $casts = [
    ...
    'active' => 'boolean'
];
```

In your `app/Exceptions/Handler.php` file change the extended class to reference:

```
use Varbox\Exceptions\Handler as VarboxExceptionHandler;

class Handler extends VarboxExceptionHandler {
    ...
}
```

<a name="copy-sql-files"></a>
#### Copy SQL Files

Copy all sql files from inside the `varbox/database/sql` directory into your `database/sql` directory.

```
cp varbox/database/sql/countries.sql database/sql/countries.sql
cp varbox/database/sql/states.sql database/sql/states.sql
cp varbox/database/sql/cities.sql database/sql/cities.sql
```

<a name="copy-seeder-classes"></a>
#### Copy Seeder Classes

Copy all seeder classes from inside the `varbox/database/seeds` directory into your own `database/seeds` directory.
Also, don't forget to change the files' extensions from `stub` to `php`.

```
cp varbox/database/seeds/VarboxSeeder.stub database/seeds/VarboxSeeder.php
cp varbox/database/seeds/PermissionsSeeder.stub database/seeds/PermissionsSeeder.php
cp varbox/database/seeds/RolesSeeder.stub database/seeds/RolesSeeder.php
cp varbox/database/seeds/UsersSeeder.stub database/seeds/UsersSeeder.php
cp varbox/database/seeds/LanguagesSeeder.stub database/seeds/LanguagesSeeder.php
cp varbox/database/seeds/CountriesSeeder.stub database/seeds/CountriesSeeder.php
cp varbox/database/seeds/StatesSeeder.stub database/seeds/StatesSeeder.php
cp varbox/database/seeds/CitiesSeeder.stub database/seeds/CitiesSeeder.php
```

<a name="migrate-database"></a>
#### Migrate Database

Create the necessary database tables for the Varbox platform to work properly.

```
php artisan migrate
```

<a name="seed-database"></a>
#### Seed Database

Populate the database with necessary data for the Varbox platform to work properly.

```
composer dump-autoload

php artisan db:seed --class="VarboxSeeder"
```

<a name="symlink-directories"></a>
#### Symlink Directories

Run the following `artisan` commands:

```
php artisan varbox:uploads-link
php artisan varbox:wysiwyg-link
```

<a name="create-new-files"></a>
#### Create New Files

In your `storage` directory create a new folder called `uploads`.   

```
mkdir storage/uploads
```

In your `storage/uploads` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `storage` directory create a new folder called `backups`.   

```
mkdir storage/backups
```

In your `storage/backups` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `storage` directory create a new folder called `wysiwyg`. 

```
mkdir storage/wysiwyg
```
  
In your `storage/wysiwyg` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `app/Http/Controllers` directory create a new file called `PagesController.php` containing:

```php
<?php

namespace App\Http\Composers;

use Illuminate\Http\Request;
use Varbox\Contracts\PageModelContract;

class PagesController extends Controller
{
    /**
     * @var PageModelContract
     */
    protected $page;

    /**
     * @param Request $request
     * @set Page $page
     */
    public function __construct(Request $request)
    {
        $this->page = $request->route()->action['model'] ?? null;

        if (!($this->page && $this->page->exists) || $this->page->isDrafted()) {
            abort(404);
        }
    }

    /**
     * @return \Illuminate\View\View
     */
    public function show()
    {
        return view('pages.show')->with([
            'page' => $this->page
        ]);
    }
}

```

In your `app/Http/Composers` directory create a new file called `AdminMenuComposer.php` containing:

```php
<?php

namespace App\Http\Composers;

use Illuminate\Contracts\Auth\Authenticatable;
use Illuminate\Support\Collection;
use Illuminate\View\View;
use Varbox\Helpers\AdminMenuHelper;
use Varbox\Menu\MenuItem;

class AdminMenuComposer
{
    /**
     * @var Authenticatable
     */
    protected $user;

    /**
     * @var Collection
     */
    protected $permissions;

    /**
     * @param Authenticatable $user
     */
    public function __construct(Authenticatable $user)
    {
        $this->user = $user;
    }

    /**
     * Construct the admin menu.
     *
     * @param View $view
     */
    public function compose(View $view)
    {
        $menu = menu_admin()->make(function (AdminMenuHelper $menu) {
            $menu->add(function (MenuItem $item) {
                $item->name('Home')->url(route('admin'))->data('icon', 'fa-home')->active('admin');
            });

            $menu->add(function ($item) use ($menu) {
                $auth = $item->name('Access Control')->data('icon', 'fa-sign-in-alt')
                    ->permissions('users-list', 'admins-list', 'roles-list', 'permissions-list', 'activity-list', 'notifications-list')
                    ->active('admin/users/*', 'admin/admins/*', 'admin/roles/*', 'admin/permissions/*', 'admin/activity/*', 'admin/notifications/*');

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Users')->url(route('admin.users.index'))->permissions('users-list')->active('admin/users/*');
                });

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Admins')->url(route('admin.admins.index'))->permissions('admins-list')->active('admin/admins/*');
                });

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Roles')->url(route('admin.roles.index'))->permissions('roles-list')->active('admin/roles/*');
                });

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Permissions')->url(route('admin.permissions.index'))->permissions('permissions-list')->active('admin/permissions/*');
                });

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Activity')->url(route('admin.activity.index'))->permissions('activity-list')->active('admin/activity/*');
                });

                $menu->child($auth, function (MenuItem $item) {
                    $item->name('Notifications')->url(route('admin.notifications.index'))->permissions('notifications-list')->active('admin/notifications/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $cms = $item->name('Manage Content')->data('icon', 'fa-edit')
                    ->permissions('pages-list', 'menus-list', 'blocks-list', 'emails-list', 'layouts-list')
                    ->active('admin/pages/*', 'admin/menus/*', 'admin/blocks/*', 'admin/emails/*', 'admin/layouts/*');

                $menu->child($cms, function (MenuItem $item) {
                    $item->name('Pages')->url(route('admin.pages.index'))->permissions('pages-list')->active('admin/pages/*');
                });

                $menu->child($cms, function (MenuItem $item) {
                    $item->name('Menus')->url(route('admin.menus.locations'))->permissions('menus-list')->active('admin/menus/*');
                });

                $menu->child($cms, function (MenuItem $item) {
                    $item->name('Blocks')->url(route('admin.blocks.index'))->permissions('blocks-list')->active('admin/blocks/*');
                });

                $menu->child($cms, function (MenuItem $item) {
                    $item->name('Emails')->url(route('admin.emails.index'))->permissions('emails-list', 'aa')->active('admin/emails/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $media = $item->name('Media Library')->data('icon', 'fa-copy')
                    ->permissions('uploads-list')
                    ->active('admin/uploads/*');

                $menu->child($media, function (MenuItem $item) {
                    $item->name('Uploads')->url(route('admin.uploads.index'))->permissions('uploads-list')->active('admin/uploads/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $trans = $item->name('Multi Language')->data('icon', 'fa-globe-americas')
                    ->permissions('translations-list', 'languages-list')
                    ->active('admin/translations/*', 'admin/languages/*');

                $menu->child($trans, function (MenuItem $item) {
                    $item->name('Translations')->url(route('admin.translations.index'))->permissions('translations-list')->active('admin/translations/*');
                });

                $menu->child($trans, function (MenuItem $item) {
                    $item->name('Languages')->url(route('admin.languages.index'))->permissions('languages-list')->active('admin/languages/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $geo = $item->name('Geo Location')->data('icon', 'fa-map-marker-alt')
                    ->permissions('countries-list', 'states-list', 'cities-list')
                    ->active('admin/countries/*', 'admin/states/*', 'admin/cities/*');

                $menu->child($geo, function (MenuItem $item) {
                    $item->name('Countries')->url(route('admin.countries.index'))->permissions('countries-list')->active('admin/countries/*');
                });

                $menu->child($geo, function (MenuItem $item) {
                    $item->name('States')->url(route('admin.states.index'))->permissions('states-list')->active('admin/states/*');
                });

                $menu->child($geo, function (MenuItem $item) {
                    $item->name('Cities')->url(route('admin.cities.index'))->permissions('cities-list')->active('admin/cities/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $sys = $item->name('System Settings')->data('icon', 'fa-cog')
                    ->permissions('configs-list', 'redirects-list', 'errors-list', 'backups-list')
                    ->active('admin/config/*', 'admin/redirects/*', 'admin/errors/*', 'admin/backups/*');

                $menu->child($sys, function (MenuItem $item) {
                    $item->name('Configs')->url(route('admin.configs.index'))->permissions('configs-list')->active('admin/configs/*');
                });

                $menu->child($sys, function (MenuItem $item) {
                    $item->name('Redirects')->url(route('admin.redirects.index'))->permissions('redirects-list')->active('admin/redirects/*');
                });

                $menu->child($sys, function (MenuItem $item) {
                    $item->name('Errors')->url(route('admin.errors.index'))->permissions('errors-list')->active('admin/errors/*');
                });

                $menu->child($sys, function (MenuItem $item) {
                    $item->name('Backups')->url(route('admin.backups.index'))->permissions('backups-list')->active('admin/backups/*');
                });
            });
        })->filter(function (MenuItem $item) {
            return $this->user->isSuper() || $this->userHasAnyMenuPermission($item->permissions());
        });

        $view->with('menu', $menu);
    }

    /**
     * Determine if the user has any of the given permissions.
     *
     * @param array $permissions
     * @return bool
     */
    protected function userHasAnyMenuPermission(array $permissions = [])
    {
        if (empty($permissions)) {
            return true;
        }

        if (!$this->permissions) {
            $this->permissions = $this->user->getPermissions()->pluck('name');
        }

        foreach ($permissions as $permission) {
            if ($this->permissions->contains($permission)) {
                return true;
            }
        }

        return false;
    }
}
```

<a name="overwrite-bindings"></a>
#### Overwrite Bindings

Inside your `config/varbox/bindings.php` file, change the `user_model` value to `\App\User::class`

```php
/*
| --------------------------------------------------------------------------------------------------------------
| Model Class Bindings
| --------------------------------------------------------------------------------------------------------------
*/
'models' => [
    ...

    /*
    |
    | Concrete implementation for the "user model".
    | To extend or replace this functionality, change the value below with your full "user model" FQN.
    |
    | Your class will have to (first option is recommended):
    | - extend the "Varbox\Models\User" class
    | - or at least implement the "Varbox\Contracts\UserModelContract" interface.
    |
    | Regardless of the concrete implementation below, you can still use it like:
    | - app('user.model') OR app('\Varbox\Contracts\UserModelContract')
    | - or you could even use your own class as a direct implementation
    |
    */
    'user_model' => \App\User::class,
```

Inside your `config/varbox/bindings.php` file, change the `admin_menu_view_composer` value to `\App\Http\Composers\AdminMenuComposer::class`

```php
/*
| --------------------------------------------------------------------------------------------------------------
| View Composers Class Bindings
| --------------------------------------------------------------------------------------------------------------
*/
'view_composers' => [
    ...

    /*
    |
    | Concrete implementation for the "admin menu view composer".
    | To extend or replace this functionality, change the value below with your full "admin menu view composer" FQN.
    |
    | Your class will have to (first option is recommended):
    | - extend the "Varbox\Composers\AdminMenuComposer" class
    | - or at least implement the following methods: compose()
    |
    */
    'admin_menu_view_composer' => \App\Http\Composers\AdminMenuComposer::class,
```
