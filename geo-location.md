# Geo Location

- [Admin Interface](#admin-interface)
- [Countries](#countries)
    - [Seeder Class](#seeder-class)
    - [Fetch Country States](#fetch-country-states)
    - [Fetch Country Cities](#fetch-country-cities)
    - [Sort Countries Alphabetically](#sort-countries-alphabetically)
- [States](#states)
    - [Seeder Class](#seeder-class)
    - [Get State Country](#get-state-country)
    - [Fetch State Cities](#fetch-state-cities)
    - [Sort States Alphabetically](#sort-states-alphabetically)
- [Cities](#cities)
    - [Seeder Class](#seeder-class)
    - [Get City Country](#get-city-country)
    - [Get City State](#get-city-state)
    - [Sort Cities Alphabetically](#sort-cities-alphabetically)
- [Overwrite Bindings](#overwrite-bindings)

The geo location module is a powerful tool that offers you the following features out of the box:
- manage all countries in the world
- manage most states in the world
- manage most cities in the world

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your countries, states and cities.

<a name="countries-interface"></a>
#### Countries Interface

You can find the countries section inside **Admin -> Geo Location -> Countries**.   
Feel free to explore all available options this section offers.

![Countries List](/docs/{{version}}/countries-list.png)

<a name="states-interface"></a>
#### States Interface

You can find the states section inside "**Admin -> Geo Location -> States**.   
Feel free to explore all available options this section offers.

![States List](/docs/{{version}}/states-list.png)

<a name="cities-interface"></a>
#### Cities Interface

You can find the cities section inside **Admin -> Geo Location -> Cities**.   
Feel free to explore all available options this section offers.

![Cities List](/docs/{{version}}/cities-list.png)

<a name="countries"></a>
## Countries

The counties component provides you with an interface and some functionality to manage your countries.

In order to provide you with a complex crud functionality inside the admin, the countries crud implements the following out of the box:

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

<a name="seeder-class"></a>
#### Seeder Class

The countries component provides a seeder class for seeding all available countries into your `countries` database table. 
The seeder only works with an empty `countries` database table.

If you've successfully installed the Varbox platform, you should find the `CountriesSeeder.php` file inside your `database/seeds` directory and the `countries.sql` file inside your `database/sql` directory.

You can seed your countries, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="CountriesSeeder"
```

<a name="fetch-country-states"></a>
#### Fetch Country States

You can get a country's states by using the `states` has many relation present on the `Varbox\Models\Country` model.

```php
use Varbox\Models\Country;

$country = Country::find($id);
$states = $country->states;
```

<a name="fetch-country-cities"></a>
#### Fetch Country Cities

You can get a country's cities by using the `cities` has many relation present on the `Varbox\Models\Country` model.

```php
use Varbox\Models\Country;

$country = Country::find($id);
$cities = $country->cities;
```

<a name="sort-countries-alphabetically"></a>
#### Sort Countries Alphabetically

You can fetch countries in alphabetical order by using the `alphabetically` query scope present on the `Varbox\Models\Country` model.

```php
use Varbox\Models\Country;

$countries = Country::alphabetically()->get();
```

<a name="states"></a>
## States

In order to provide you with a complex crud functionality inside the admin, the states crud implements the following out of the box:

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

<a name="seeder-class"></a>
#### Seeder Class

The states component provides a seeder class for seeding most known states into your `states` database table. 
The seeder only works with an empty `states` database table.

If you've successfully installed the Varbox platform, you should find the `StatesSeeder.php` file inside your `database/seeds` directory and the `states.sql` file inside your `database/sql` directory.

You can seed your states, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="StatesSeeder"
```

<a name="get-state-country"></a>
#### Get State Country

You can get a state's country by using the `country` belongs to relation present on the `Varbox\Models\State` model.

```php
use Varbox\Models\State;

$state = State::find($id);
$country = $state->country;
```

<a name="fetch-state-cities"></a>
#### Fetch State Cities

You can get a state's cities by using the `cities` has many relation present on the `Varbox\Models\State` model.

```php
use Varbox\Models\State;

$state = State::find($id);
$cities = $state->cities;
```

<a name="sort-states-alphabetically"></a>
#### Sort States Alphabetically

You can fetch states in alphabetical order by using the `alphabetically` query scope present on the `Varbox\Models\State` model.

```php
use Varbox\Models\State;

$states = State::alphabetically()->get();
```

<a name="cities"></a>
## Cities

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

<a name="seeder-class"></a>
#### Seeder Class

The cities component provides a seeder class for seeding most known cities into your `cities` database table. 
The seeder only works with an empty `cities` database table.

If you've successfully installed the Varbox platform, you should find the `CitiesSeeder.php` file inside your `database/seeds` directory and the `cities.sql` file inside your `database/sql` directory.

You can seed your cities, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="CitiesSeeder"
```

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

<a name="country-bindings"></a>
### Country Bindings

The country classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Country</p>

Found in `config/varbox/bindings.php` at `models.country_model` key.   
This class represents the country model.

<p class="overwrite-class">Varbox\Controllers\CountriesController</p>

Found in `config/varbox/bindings.php` at `controllers.countries_controller` key.   
This class is used for interactions with the "Admin -> Geo Location -> Countries".

<p class="overwrite-class">Varbox\Requests\CountryRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.country_form_request` key.   
This class is used for validating any country when updating.

<p class="overwrite-class">Varbox\Filters\CountryFilter</p>

Found in `config/varbox/bindings.php` at `filters.country_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\CountrySort</p>

Found in `config/varbox/bindings.php` at `sorts.country_sort` key.   
This class is used for applying the sorting logic.

<a name="state-bindings"></a>
### State Bindings

The state classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\State</p>

Found in `config/varbox/bindings.php` at `models.state_model` key.   
This class represents the state model.

<p class="overwrite-class">Varbox\Controllers\StatesController</p>

Found in `config/varbox/bindings.php` at `controllers.states_controller` key.   
This class is used for interactions with the "Admin -> Geo Location -> States".

<p class="overwrite-class">Varbox\Requests\StateRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.state_form_request` key.   
This class is used for validating any state when creating or updating.

<p class="overwrite-class">Varbox\Filters\StateFilter</p>

Found in `config/varbox/bindings.php` at `filters.state_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\StateSort</p>

Found in `config/varbox/bindings.php` at `sorts.state_sort` key.   
This class is used for applying the sorting logic.

<a name="city-bindings"></a>
### City Bindings

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
