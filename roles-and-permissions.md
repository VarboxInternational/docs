<h1>Roles & Permissions</h1>

- [Admin Interface](#admin-interface)
    - [Roles Interface](#roles-interface)
    - [Permissions Interface](#permissions-interface)
- [Manage Access](#manage-access)
    - [Role Based Access](#role-based-access)
    - [Permission Based Access](#permission-based-access)
- [Roles Usage](#roles-usage)
    - [Fetch Roles](#fetch-roles)
    - [Save Roles](#Save-roles)
    - [Query By Roles](#query-by-roles)
    - [Check For Roles](#check-for-roles)
- [Permissions Usage](#permissions-usage)
    - [Fetch Permissions](#fetch-permissions)
    - [Save Permissions](#Save-permissions)
    - [Query By Permissions](#query-by-permissions)
    - [Check For Permissions](#check-for-permissions)
- [Seeder Classes](#seeder-classes)
    - [Roles Seeder](#roles-seeder)
    - [Permissions Seeder](#permissions-seeder)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
This functionality allows you to manage roles & permissions for your users.
</p>

The Varbox platform comes out of the box with a solution for role based permissions already integrated on your `App\User` model.

If you want to attach the role based permission functionality on other models, all you have to do is use the `Varbox\Traits\HasRolesAndPermissions` trait on your models.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\HasRolesAndPermissions;

class YourModel extends Model
{
    use HasRolesAndPermissions;
}
```

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your roles and permissions.

<a name="roles-interface"></a>
#### Roles Interface

You can find the roles section inside **Admin -> Access Control -> Roles**.   
Feel free to explore all available options this section offers.

![Roles List](/docs/{{version}}/roles-list.png)

<a name="permissions-interface"></a>
#### Permissions Interface

You can find the permissions section inside **Admin -> Access Control -> Permissions**.   
Feel free to explore all available options this section offers.

![Roles List](/docs/{{version}}/permissions-list.png)

<a name="manage-access"></a>
## Manage Access

The Varbox admin panel already supports role based permissions for the logged in user.

<a name="role-based-access"></a>
#### Role Based Access

You can restrict access to users based on their roles by using the `varbox.check.roles` middleware.

```php
Route::get('your-url')->name('your-name')
    ->middleware('varbox.check.roles:role_1,role_2');
```

You can even specify the middleware at `route group` level and then pass a `roles` parameter to individual routes.

```php
Route::group([
    'middleware' => 'varbox.check.roles'
], funnction () {
    Route::get('your-url-1', ['roles' => 'role_1']);
    Route::get('your-url-2', ['roles' => 'role_2']);
});
```

<a name="permission-based-access"></a>
#### Permission Based Access

You can restrict access to users based on their permissions by using the `varbox.check.permissions` middleware.

```php
Route::get('your-url')->name('your-name')
    ->middleware('varbox.check.permissions:permission_1,permission_2');
```

You can even specify the middleware at `route group` level and then pass a `permissions` parameter to individual routes.

```php
Route::group([
    'middleware' => 'varbox.check.permissions'
], funnction () {
    Route::get('your-url-1', ['permissions' => 'permission_1']);
    Route::get('your-url-2', ['permissions' => 'permission_2']);
});
```

<a name="roles-usage"></a>
## Roles Usage

When working with your `App\User` model you have a few options when it comes to managing your roles.

<a name="fetch-roles"></a>
#### Fetch Roles

You can get a user's assigned roles, by using the `roles()` many to many relation.

```php
$roles = $user->roles;
```

<a name="save-roles"></a>
#### Save Roles

You can assign one or multiple roles to a user by using the `assignRoles()` method.   

```php
// by role name
$user->assignRoles('owner');

// by array
$user->assignRoles(['owner', 'editor']);

// by collection
$user->assignRoles(Role::all());
```

You can remove one or multiple roles from a user by using the `removeRoles()` method.   

```php
// by role name
$user->removeRoles('owner');

// by array
$user->removeRoles(['owner', 'editor']);

// by collection
$user->removeRoles(Role::all());
```

You can sync the roles for a user by using the `syncRoles()` method. 
This method detaches all assigned roles and then it `assigns` the specified roles for the user.   

```php
// by role name
$user->syncRoles('owner');

// by array
$user->syncRoles(['owner', 'editor']);

// by collection
$user->syncRoles(Role::all());
```

<a name="query-by-roles"></a>
#### Query By Roles

You can get users that have certain roles by using the `withRoles` query scope.

```php
// by role name
$users = User::withRoles('owner')->get();

// by array
$users = User::withRoles(['owner', 'editor'])->get();

// by collection
$users = User::withRoles(Role::all())->get();
```

You can get users that don't have certain roles by using the `withoutRoles` query scope.

```php
// by role name
$users = User::withoutRoles('owner')->get();

// by array
$users = User::withoutRoles(['owner', 'editor'])->get();

// by collection
$users = User::withoutRoles(Role::all())->get();
```

<a name="check-for-roles"></a>
#### Check For Roles

You can check if a user has a certain role by using the `hasRole()` method.

```php
$user->hasRole('owner');
```

You can check if a user has at least one of the specified roles by using the `hasAnyRole()` method.

```php
$user->hasAnyRole(['owner', 'editor']);
```

You can check if a user has all of the specified roles by using the `hasAllRoles()` method.

```php
$user->hasAllRoles(['owner', 'editor']);
```

<a name="permissions-usage"></a>
## Permissions Usage

When working with your `App\User` model you have a few options when it comes to managing your permissions.

<a name="fetch-permissions"></a>
#### Fetch Permissions

You can get a user's assigned permissions, by using the `permissions()` many to many relation.

```php
$permissions = $user->permissions;
```

<a name="save-permissions"></a>
#### Save Permissions

You can grant one or multiple permissions to a user by using the `grantPermission()` method.   

```php
// by permission name
$user->grantPermission('view-users');

// by array
$user->grantPermission(['view-users', 'edit-users']);

// by collection
$user->grantPermission(Permission::all());
```

You can revoke one or multiple permissions from a user by using the `revokePermission()` method.   

```php
// by peermission name
$user->revokePermission('view-users');

// by array
$user->revokePermission(['view-users', 'edit-users']);

// by collection
$user->revokePermission(Permission::all());
```

You can sync the permissions for a user by using the `syncPermissions()` method. 
This method detaches all assigned permissions and then it `grants` the specified permissions for the user.   

```php
// by permission name
$user->syncPermissions('view-users');

// by array
$user->syncPermissions(['view-users', 'editor']);

// by collection
$user->syncPermissions(Permission::all());
```

<a name="query-by-permissions"></a>
#### Query By Permissions

You can get users that have certain permissions by using the `withPermissions` query scope.

```php
// by permission name
$users = User::withPermissions('view-users')->get();

// by array
$users = User::withPermissions(['view-users', 'edit-users'])->get();

// by collection
$users = User::withPermissions(Role::all())->get();
```

You can get users that don't have certain permissions by using the `withoutPermissions` query scope.

```php
// by permission name
$users = User::withoutPermissions('view-users')->get();

// by array
$users = User::withoutPermissions(['view-users', 'edit-users'])->get();

// by collection
$users = User::withoutPermissions(Role::all())->get();
```

<a name="check-for-permissions"></a>
#### Check For Permissions

You can check if a user has a certain permission by using the `hasPermission()` method.

```php
$user->hasPermission('view-users');
```

You can check if a user has at least one of the specified permissions by using the `hasAnyPermission()` method.

```php
$user->hasAnyPermission(['view-users', 'edit-users']);
```

You can check if a user has all of the specified permissions by using the `hasAllPermissions()` method.

```php
$user->hasAllPermissions(['view-users', 'edit-users']);
```

<a name="seeder-classes"></a>
## Seeder Classes

The Varbox platform publishes a few seeder classes inside you `database/seeds` directory.

<a name="roles-seeder"></a>
#### Roles Seeder

The `database/seeds/RolesSeeder.php` is your seeder class for roles.   
To add a new role to the seeder, just append your role to the `$roles` property:

```php
/**
 * Mapping structure of admin roles.
 *
 * @var array
 */
protected $roles = [
    ...

    'Editor' => [
        'name' => 'Editor',
        'guard' => 'admin',
    ],
];
``` 

Then simply seed your roles by running the following artisan command:

```
php artisan db:seed --class="RolesSeeeder"
```

When running the seeder, a check is performed for each role to verify if it already exists, so you won't end up with duplicated roles on subsequential executions of the seeder.

<hr>

By default, the following roles are already present for you to use:
- `Admin` - a user having this role will be able to access the admin panel
- `Super` - a user having this role will have full access inside the admin panel

<a name="permissions-seeder"></a>
#### Permissions Seeder

The `database/seeds/PermissionsSeeder.php` is your seeder class for permissions.   
To add new permissions to the seeder, just append your permissions to the `$permissions` property:

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        'List' => [
            'group' => 'Posts',
            'label' => 'List',
            'guard' => 'admin',
            'name' => 'posts-list',
        ],
        'Add' => [
            'group' => 'Posts',
            'label' => 'Add',
            'guard' => 'admin',
            'name' => 'posts-add',
        ],
        'Edit' => [
            'group' => 'Posts',
            'label' => 'Edit',
            'guard' => 'admin',
            'name' => 'posts-edit',
        ],
        'Delete' => [
            'group' => 'Posts',
            'label' => 'Delete',
            'guard' => 'admin',
            'name' => 'posts-delete',
        ],
    ],
];
```

Then simply seed your permissions by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeeder"
```

When running the seeder, a check is performed for each permission to verify if it already exists, so you won't end up with duplicated permissions on subsequential executions of the seeder.

<hr>

By default, all necessary permissions for each platform specific functionality are already seeded.

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

<p class="overwrite-class">Varbox\Models\Role</p>

Found in `config/varbox/bindings.php` at `models.role_model` key.   
This class represents the role model.

<p class="overwrite-class">Varbox\Models\Permission</p>

Found in `config/varbox/bindings.php` at `models.permission_model` key.   
This class represents the permission model.

<p class="overwrite-class">Varbox\Controllers\RolesController</p>

Found in `config/varbox/bindings.php` at `controllers.roles_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Roles" section.

<p class="overwrite-class">Varbox\Controllers\PermissionsController</p>

Found in `config/varbox/bindings.php` at `controllers.permissions_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Permissions" section.

<p class="overwrite-class">Varbox\Requests\RoleRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.role_form_request` key.   
This class is used for validating any role when inserting into the database.

<p class="overwrite-class">Varbox\Requests\PermissionRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.permission_form_request` key.   
This class is used for validating any permission when inserting into the database.

<p class="overwrite-class">Varbox\Middleware\CheckRoles</p>

Found in `config/varbox/bindings.php` at `middleware.check_roles_middleware` key.   
This class is used for restricting access to users based on the given roles.

<p class="overwrite-class">Varbox\Middleware\CheckPermissions</p>

Found in `config/varbox/bindings.php` at `middleware.check_permissions_middleware` key.   
This class is used for restricting access to users based on the given permissions.

<p class="overwrite-class">Varbox\Filters\PermissionFilter</p>

Found in `config/varbox/bindings.php` at `filters.permission_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Filters\RoleFilter</p>

Found in `config/varbox/bindings.php` at `filters.role_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\PermissionSort</p>

Found in `config/varbox/bindings.php` at `sorts.permission_sort` key.   
This class is used for applying the sorting logic.

<p class="overwrite-class">Varbox\Sorts\RoleSort</p>

Found in `config/varbox/bindings.php` at `sorts.role_sort` key.   
This class is used for applying the sorting logic.
