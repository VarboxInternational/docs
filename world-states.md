<h1>World States <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [Seeder Class](#seeder-class)
- [Usage](#usage)
    - [Get State Country](#get-state-country)
    - [Fetch State Cities](#fetch-state-cities)
    - [Sort States Alphabetically](#sort-states-alphabetically)
- [Overwrite Bindings](#overwrite-bindings)

The states component provides you with an interface to manage your states.

> As a bonus, 4.854 states are already seeded for you when installing the platform.

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your countries, states and cities.

You can find the states section inside "**Admin -> Geo Location -> States**.   
Feel free to explore all available options this section offers.

![States List](/docs/{{version}}/states-list.png)

<a name="seeder-class"></a>
## Seeder Class

The states component provides a seeder class for seeding most known states into your `states` database table. 
The seeder only works with an empty `states` database table.

If you've successfully installed the Varbox platform, you should find the `StatesSeeder.php` file inside your `database/seeds` directory and the `states.sql` file inside your `database/sql` directory.

You can seed your states, if you haven't done so yet, by using the following artisan command:

```
php artisan db:seed --class="StatesSeeder"
```

<a name="usage"></a>
## Usage

Let's see the most common things you can do using states.

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
