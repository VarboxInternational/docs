<h1>World Cities <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [Seeder Class](#seeder-class)
- [Usage](#usage)
    - [Get City Country](#get-city-country)
    - [Get City State](#get-city-state)
    - [Sort Cities Alphabetically](#sort-cities-alphabetically)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The cities component provides you with an interface to manage your cities.
</p>

> As a bonus, 141.488 cities are already seeded for you when installing the platform.

In order to provide you with a complex crud functionality inside the admin, the cities crud implements the following out of the box:

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
[Activity Log](/docs/{{version}}/activity-log)
[Csv Exports](/docs/{{version}}/csv-exports)

</div>

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your countries, states and cities.

You can find the cities section inside **Admin -> Geo Location -> Cities**.   
Feel free to explore all available options this section offers.

![Cities List](/docs/{{version}}/cities-list.png)

<a name="seeder-class"></a>
## Seeder Class

The cities component provides a seeder class for seeding most known cities into your `cities` database table. 
The seeder only works with an empty `cities` database table.

If you've successfully installed the Varbox platform, you should find the `CitiesSeeder.php` file inside your `database/seeds` directory and the `cities.sql` file inside your `database/sql` directory.

You can seed your cities, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="CitiesSeeder"
```

<a name="usage"></a>
## Usage

Let's see the most common things you can do using cities.

<a name="get-city-country"></a>
#### Get City Country

You can get a city's country by using the `country` belongs to relation present on the `Varbox\Models\City` model.

```php
use Varbox\Models\City;

$city = City::find($id);
$country = $city->country;
```

<a name="get-city-state"></a>
#### Get City State

You can get a city's state by using the `state` belongs to relation present on the `Varbox\Models\City` model.

```php
use Varbox\Models\City;

$city = City::find($id);
$state = $city->state;
```

<a name="sort-cities-alphabetically"></a>
#### Sort Cities Alphabetically

You can fetch cities in alphabetical order by using the `alphabetically` query scope present on the `Varbox\Models\City` model.

```php
use Varbox\Models\City;

$cities = City::alphabetically()->get();
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

The city classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\City</p>

Found in `config/varbox/bindings.php` at `models.city_model` key.   
This class represents the city model.

<p class="overwrite-class">Varbox\Controllers\CitiesController</p>

Found in `config/varbox/bindings.php` at `controllers.cities_controller` key.   
This class is used for interactions with the "Admin -> Geo Location -> Cities".

<p class="overwrite-class">Varbox\Requests\CityRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.city_form_request` key.   
This class is used for validating any city when creating or updating.

<p class="overwrite-class">Varbox\Filters\CityFilter</p>

Found in `config/varbox/bindings.php` at `filters.city_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\CitySort</p>

Found in `config/varbox/bindings.php` at `sorts.city_sort` key.   
This class is used for applying the sorting logic.
