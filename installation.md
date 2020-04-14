# Installation

- [Automatic Installation](#automatic-installation)
- [Manual Installation](#manual-installation)
    - [Publish Vendor Files](#publish-vendor-files)
    - [Write Env Variables](#write-env-variables)
    - [Register Routes](#register-routes)
    - [Update Configurations](#update-configurations)
    - [Update Existing Files](#update-existing-files)
    - [Migrate Database](#migrate-database)
    - [Seed Database](#seed-database)
    - [Symlink Directories](#symlink-directories)
    - [Create New Files](#create-new-files)

<a name="automatic-installation"></a>
## Automatic Installation

After you've successfully installed <a href="https://laravel.com/docs/7.x/installation" target="_blank">Laravel 7</a> you can install the `VarBox` platform with ease by running only two commands.

> Having a database setup in your `.env` file is mandatory!

#### Require the package

```
composer require varbox/varbox
```

#### Run the command

```
php artisan varbox:install
```

That's it!

Now you can sign into your admin panel by accessing `/admin`.   
Use `admin@mail.com / admin` to authenticate.

<a name="manual-installation"></a>
## Manual Installation

If for some reason you don't want to automatically install the `VarBox` platform, you can do so manually, by following the below steps.

The following steps also provides useful insight on what the `varbox:install` artisan command actually does. 

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

FFMPEG_PATH=ffmpeg
FFPROBE_PATH=ffprobe

ANALYTICS_VIEW_ID=
```

<a name="register-routes"></a>
#### Register Routes

In your `routes/web.php` file, at the very bottom, add the following:

```
// this should be the last line
Route::url();
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
    'backups' => [
        'driver' => 'local',
        'root' => storage_path('backups'),
    ],
    'wysiwyg' => [
        'driver' => 'local',
        'root' => storage_path('wysiwyg'),
        'url' => env('APP_URL').'/wysiwyg',
        'visibility' => 'public',
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
    'first_name', 
    'last_name',
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

<a name="migrate-database"></a>
#### Migrate Database

Create the necessary database tables for the `VarBox` platform to work properly.

```
php artisan migrate
```

<a name="seed-database"></a>
#### Seed Database

Populate the database with necessary data for the `VarBox` platform to work properly.

```
php artisan db:seed --class="Varbox\Seed\PermissionsSeeder"
php artisan db:seed --class="Varbox\Seed\RolesSeeder"
php artisan db:seed --class="Varbox\Seed\UsersSeeder"
php artisan db:seed --class="Varbox\Seed\CountriesSeeder"
php artisan db:seed --class="Varbox\Seed\LanguagesSeeder"
php artisan db:seed --class="Varbox\Seed\AnalyticsSeeder"
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
In your `storage/uploads` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `storage` directory create a new folder called `backups`.   
In your `storage/backups` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `storage` directory create a new folder called `wysiwyg`.   
In your `storage/wysiwyg` directory create a new file called `.gitignore` containing the following:

```
!.gitignore
```

In your `app/Http/Controllers` directory create a new file called `PagesController.php` containing the following:

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

In your `app/Http/Composers` directory create a new file called `AdminMenuComposer.php` containing the following:

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
                $seo = $item->name('Seo Administration')->data('icon', 'fa-chart-bar')
                    ->permissions('analytics-view', 'schema-list', 'redirects-list')
                    ->active('admin/analytics/*', 'admin/schema/*', 'admin/redirects/*');

                $menu->child($seo, function (MenuItem $item) {
                    $item->name('Analytics')->url(route('admin.analytics.show'))->permissions('analytics-view')->active('admin/analytics/*');
                });

                $menu->child($seo, function (MenuItem $item) {
                    $item->name('Schema')->url(route('admin.schema.index'))->permissions('schema-list')->active('admin/schema/*');
                });

                $menu->child($seo, function (MenuItem $item) {
                    $item->name('Redirects')->url(route('admin.redirects.index'))->permissions('redirects-list')->active('admin/redirects/*');
                });
            });

            $menu->add(function ($item) use ($menu) {
                $sys = $item->name('System Settings')->data('icon', 'fa-cog')
                    ->permissions('configs-list', 'errors-list', 'backups-list')
                    ->active('admin/config/*', 'admin/errors/*', 'admin/backups/*');

                $menu->child($sys, function (MenuItem $item) {
                    $item->name('Configs')->url(route('admin.configs.index'))->permissions('configs-list')->active('admin/configs/*');
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
