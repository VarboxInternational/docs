# Users & Admins

- [Admin Interface](#admin-interface)
- [Users & Admins](#users-and-admins)
    - [Fetch Active / Inactive Users](#fetch-active-inactive-users)
    - [Verify Active / Inactive User](#verify-active-inactive-user)
    - [Filter Admin Users](#filter-admin-users)
    - [Check Admin User](#check-admin-user)
- [User Addresses](#user-addresses)
    - [Fetch User Addresses](#fetch-user-addresses)
    - [Get Address User](#get-address-user)
    - [Get Address Country](#get-address-country)
    - [Get Address State](#get-address-state)
    - [Get Address City](#get-address-city)
    - [Filter Addresses By User](#filter-addresses-by-user)
    - [Filter Addresses By Country](#filter-addresses-by-country)
    - [Filter Addresses By State](#filter-addresses-by-state)
    - [Filter Addresses By City](#filter-addresses-by-city)
- [Overwrite Bindings](#overwrite-bindings)

Manage your users & admins directly from the Varbox admin panel, with no extra setup.

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your users & admins.

<a name="users-interface"></a>
#### Users Interface

You can find the users section inside **Admin -> Access Control -> Users**.   
Feel free to explore all available options this section offers.

![Users List](/docs/{{version}}/users-list.png)

<a name="admins-interface"></a>
#### Admins Interface

You can find the admins section inside **Admin -> Access Control -> Admins**.   
Feel free to explore all available options this section offers.

![Admins List](/docs/{{version}}/admins-list.png)

<a name="addresses-interface"></a>
#### Addresses Interface

You can find the addresses section inside **Admin -> Access Control -> Users -> Addresses**.   
Feel free to explore all available options this section offers.

![Addresses List](/docs/{{version}}/addresses-list.png)

<a name="users-and-admins"></a>
## Users & Admins

The users & admins component is a powerful access control functionality that allows you to quickly manage your users for your application.

In order to provide you with complex crud functionalities inside the admin, the users & admins cruds implement the following out of the box:

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

> The `App\User` model now extends the `Varbox\Models\User` class, for extra functionalities.   
> The `App\User` model is bound to the `user_model` key inside `config/varbox/bindings.php`

<a name="fetch-active-inactive-users"></a>
#### Fetch Active / Inactive Users

You can get only active users by applying the `onlyActive` query scope.

```php
$users = \App\User::onlyActive()->get();
```

Alternatively, you can get only inactive users by applying the `onlyInactive` query scope.

```php
$users = \App\User::onlyInactive()->get();
```

<a name="verify-active-inactive-user"></a>
#### Verify Active / Inactive User

You can verify if a user is active by applying the `isActive` method.

```php
$user = \App\User::find($id);

if ($user->isActive()) ...
```

Alternatively, you can verify if a user is inactive by applying the `isInactive` method.

```php
$user = \App\User::find($id);

if ($user->isInactive()) ...
```

<a name="filter-admin-users"></a>
#### Filter Admin Users

You can get only admin users by applying the `onlyAdmins` query scope.

```php
$admins = \App\User::onlyAdmins()->get();
```

Alternatively, you can get exclude admin users by applying the `excludingAdmins` query scope.

```php
$users = \App\User::excludingAdmins()->get();
````

<a name="check-admin-user"></a>
#### Check Admin User

You can check if a user is an admin by applying the `isAdmin` method.

```php
$user = \App\User::find($id);

if ($user->isAdmin()) ...
```

<a name="user-addresses"></a>
## User Addresses

Now that your `App\User` model extends the `Varbox\Models\User` model, it's also possible to assign addresses to your users. 
You can also do this from inside the admin panel, when editing a user.

<a name="fetch-user-addresses"></a>
#### Fetch User Addresses

You can get a user's addresses by using the `addresses` has many relation present on the `Varbox\Traits\HasAddresses` trait.

```php
$user = \App\User::find($id);
$addresses = $user->addresses;
```

<a name="get-address-user"></a>
#### Get Address User

You can get the belonging user of an address by using the `user` belongs to relation present on the `Varbox\Models\Address` model.

```php
$address = \Varbox\Models\Address::find($id);
$user = $address->user;
```

<a name="get-address-country"></a>
#### Get Address Country

You can get the belonging country of an address by using the `country` belongs to relation present on the `Varbox\Models\Address` model.

```php
$address = \Varbox\Models\Address::find($id);
$country = $address->country;
```

<a name="get-address-state"></a>
#### Get Address State

You can get the belonging state of an address by using the `state` belongs to relation present on the `Varbox\Models\Address` model.

```php
$address = \Varbox\Models\Address::find($id);
$state = $address->state;
```

<a name="get-address-city"></a>
#### Get Address City

You can get the belonging city of an address by using the `city` belongs to relation present on the `Varbox\Models\Address` model.

```php
$address = \Varbox\Models\Address::find($id);
$city = $address->city;
```

<a name="filter-addresses-by-user"></a>
#### Filter Addresses By User

You can get only addresses belonging to a given user by using the `ofUser` query scope present on the `Varbox\Models\Address` model.

```php
// by passing the user model
$addresses = \Varbox\Models\Address::ofUser($userModel)->get();

// or by passing the user id
$addresses = \Varbox\Models\Address::ofUser($userId)->get();
```

<a name="filter-addresses-by-country"></a>
#### Filter Addresses By Country

You can get only addresses belonging to a given country by using the `fromCountry` query scope present on the `Varbox\Models\Address` model.

```php
// by passing the country model
$addresses = \Varbox\Models\Address::fromCountry($countryModel)->get();

// or by passing the country id
$addresses = \Varbox\Models\Address::fromCountry($countryId)->get();
```

<a name="filter-addresses-by-state"></a>
#### Filter Addresses By State

You can get only addresses belonging to a given state by using the `fromState` query scope present on the `Varbox\Models\Address` model.

```php
// by passing the state model
$addresses = \Varbox\Models\Address::fromState($stateModel)->get();

// or by passing the state id
$addresses = \Varbox\Models\Address::fromState($stateId)->get();
```

<a name="filter-addresses-by-city"></a>
#### Filter Addresses By City

You can get only addresses belonging to a given city by using the `fromCity` query scope present on the `Varbox\Models\Address` model.

```php
// by passing the city model
$addresses = \Varbox\Models\Address::fromCity($cityModel)->get();

// or by passing the city id
$addresses = \Varbox\Models\Address::fromCity($cityId)->get();
```

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

The classes available for binding overwrites are:

<a name="user-bindings"></a>
### User Bindings

The user specific classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\User</p>

Found in `config/varbox/bindings.php` at `models.user_model` key.   
This class represents the user model.

<p class="overwrite-class">Varbox\Controllers\UsersController</p>

Found in `config/varbox/bindings.php` at `controllers.users_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Users" section.

<p class="overwrite-class">Varbox\Requests\UserRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.user_form_request` key.   
This class is used for validating any user when creating or updating.

<a name="admin-bindings"></a>
### Admin Bindings

The admin specific classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Controllers\AdminsController</p>

Found in `config/varbox/bindings.php` at `controllers.admins_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Admins" section.

<p class="overwrite-class">Varbox\Requests\AdminRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.admin_form_request` key.   
This class is used for validating any admin when creating or updating.

<a name="address-bindings"></a>
### Address Bindings

The address specific classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Address</p>

Found in `config/varbox/bindings.php` at `models.address_model` key.   
This class represents the address model.

<p class="overwrite-class">Varbox\Controllers\AddressesController</p>

Found in `config/varbox/bindings.php` at `controllers.addresses_controller` key.   
This class is used for interactions with the "Admin -> Access Control -> Users -> Addresses" section.

<p class="overwrite-class">Varbox\Requests\AddressRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.address_form_request` key.   
This class is used for validating any address when creating or updating.
