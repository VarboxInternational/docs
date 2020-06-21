# World Countries

- [Admin Interface](#admin-interface)
- [Seeder Class](#seeder-class)
- [Usage](#usage)
    - [Fetch Country States](#fetch-country-states)
    - [Fetch Country Cities](#fetch-country-cities)
    - [Sort Countries Alphabetically](#sort-countries-alphabetically)
- [Overwrite Bindings](#overwrite-bindings)

The countries component provides you with an interface to manage your countries.

> As a bonus, 257 countries are already seeded for you when installing the platform.

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your countries, states and cities.

You can find the countries section inside **Admin -> Geo Location -> Countries**.   
Feel free to explore all available options this section offers.

![Countries List](/docs/{{version}}/countries-list.png)

<a name="seeder-class"></a>
## Seeder Class

The countries component provides a seeder class for seeding all available countries into your `countries` database table. 
The seeder only works with an empty `countries` database table.

If you've successfully installed the Varbox platform, you should find the `CountriesSeeder.php` file inside your `database/seeds` directory and the `countries.sql` file inside your `database/sql` directory.

You can seed your countries, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="CountriesSeeder"
```

<a name="usage"></a>
## Usage

Let's see the most common things you can do using countries.

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
