<h1>Custom Configs <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
- [Usage](#usage)
    - [Set Available Config Keys](#set-available-config-keys)
    - [Overwrite Config Values](#overwrite-config-values)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The configs component is a very powerful functionality that allows you to quickly overwrite static config values in your application.
</p>

In order to provide you with a complex crud functionality inside the admin, the configs crud implements the following out of the box:

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your settings.

You can find the configs section inside **Admin -> System Settings -> Configs**.   
Feel free to explore all available options this section offers.

![Configs List](/docs/{{version}}/configs-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the workflow behind configs.

<a name="the-configs-workflow"></a>
#### The Workflow

- Enable config overwrites by updating your .env variable
- Define the config keys that can be overwritten from the admin
- Create custom config values for your defined config keys
- Apply the middleware to actually modify the config values at runtime

<a name="usage"></a>
## Usage

Let's see how you can setup custom config values for your configuration keys.

<a name="set-available-config-keys"></a>
#### Set Available Config Keys

By default, if you were to try to add a config from inside the admin panel, you'll notice that you don't have any values to specify for the `Key` select input, which is required.

The platform isn't capable of overwriting any config values by default. This is intentional, in order not to allow a client to accidentally overwrite a config value that isn't meant for overwriting.

> Only config keys specified by the developer will be available for overwrite inside the admin.

To allow config keys to be overwritten, you should specify them inside the `config/varbox/config.php` config file, specifically inside the `keys` section.

```php
'keys' => [
    'app.name',
    'app.url',
    'app.locale',
],
```

After you've allowed the config keys, they'll appear in the admin when trying to add a custom config.

<a name="overwrite-config-values"></a>
#### Overwrite Config Values

In order for the created config values from the admin to replace your default ones, inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\OverwriteConfigs` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\OverwriteConfigs::class
    ],

    ...
];
```

That's it! Now try and access a config whose value has been dynamically modified from the admin.

```php
config('app.name'); // returns your custom value from the admin
```

<a name="configuration"></a>
## Configuration

The configs configuration file is located at `config/varbox/config.php`.

For more information on how you can customize the system settings components, please read the comments from their configuration files.

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

The config classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Config</p>

Found in `config/varbox/bindings.php` at `models.config_model` key.   
This class represents the config model.

<p class="overwrite-class">Varbox\Controllers\ConfigsController</p>

Found in `config/varbox/bindings.php` at `controllers.configs_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Configs" section.

<p class="overwrite-class">Varbox\Requests\ConfigRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.config_form_request` key.   
This class is used for validating any config when creating or updating.

<p class="overwrite-class">Varbox\Middleware\OverwriteConfigs</p>

Found in `config/varbox/bindings.php` at `middleware.overwrite_configs_middleware` key.   
This is the middleware modifying your config keys with the values assigned from the admin.

<p class="overwrite-class">Varbox\Filters\ConfigFilter</p>

Found in `config/varbox/bindings.php` at `filters.config_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\ConfigSort</p>

Found in `config/varbox/bindings.php` at `sorts.config_sort` key.   
This class is used for applying the sorting logic.
