# Dynamic Redirects

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
- [Redirect Via Middleware](#redirect-via-middleware)
    - [Attach Middleware](#attach-middleware)
- [Redirect Via PHP File](#redirect-via-php-file)
    - [Export Redirects To File](#export-redirects-to-file)
    - [Add Redirection Code](#add-redirection-code)
    - [Automate The Process](#automate-the-process)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The redirects component is a very powerful functionality that allows you to visually manage your request redirects.

In order to provide you with a complex crud functionality inside the admin, the redirects crud implements the following out of the box:

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

You can find the redirects section inside **Admin -> System Settings -> Redirects**.   
Feel free to explore all available options this section offers.

![Redirects List](/docs/{{version}}/redirects-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the workflow behind redirects.

<a name="the-workflow"></a>
#### The Workflow

- Create a redirect from inside the admin panel
- Make your application aware of the created redirects in order to handle them
- You can handle the redirects via a middleware or a php file

<a name="redirect-via-middleware"></a>
## Redirect Via Middleware

Once you have your redirects setup inside the admin, it's time to make the application aware of them and actually redirect incoming requests when needed.

<a name="attach-middleware"></a>
#### Attach Middleware

Inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\RedirectRequests` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\RedirectRequests::class
    ],

    ...
];
```

<a name="redirect-via-php-file"></a>
## Redirect Via PHP File

If you don't like the middleware approach because for example it creates between one and two queries every request, you can still leverage the redirect capability, by hooking into the `public/index.php` file.

<a name="export-redirects-to-file"></a>
#### Export Redirects To File

First of all, inside the "Admin -> System Settings -> Redirects", click on the "Export File" button.

This will create the `bootstrap/redirects.php` file containing all you redirects in an array format. 

> If you're trying to "Export File" an empty redirect list, the actual `bootstrap/redirects.php` will be deleted, as it won't be any need for it.

<a name="add-redirection-code"></a>
#### Add Redirection Code 

Once you've exported your redirects, go inside the `public/index.php` file and at the very beginning add:

```php
if (file_exists(__DIR__ . '/../bootstrap/redirects.php')) {
    foreach (require_once __DIR__ . '/../bootstrap/redirects.php' as $redirect) {
        if ($_SERVER['REQUEST_URI'] == '/' . trim($redirect['from'], '/')) {
            header('Location: ' . '/' . trim($redirect['to'], '/'), true, $redirect['status']);
            die;
        }
    }
}
```

This code fragment will handle your redirects before even hitting the Laravel application.

<a name="automate-the-process"></a>
#### Automate The Process

To automate the process of exporting the redirects every time a redirect is created, updated or deleted, set the `automatic_export` key to `true` inside the `config/varbox/redirect.php` config file.

```php
/*
|
| Export the redirects into the "bootstrap/redirects.php" on every create / update / delete.
|
*/
'automatic_export' => true
```

<a name="configuration"></a>
## Configuration

The redirect configuration file is located at `config/varbox/redirect.php`.

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

The redirect classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Redirect</p>

Found in `config/varbox/bindings.php` at `models.redirect_model` key.   
This class represents the redirect model.

<p class="overwrite-class">Varbox\Controllers\RedirectsController</p>

Found in `config/varbox/bindings.php` at `controllers.redirects_controller` key.   
This class is used for interactions with the "Admin -> System Settings -> Redirects" section.

<p class="overwrite-class">Varbox\Requests\RedirectRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.redirect_form_request` key.   
This class is used for validating any redirect when creating or updating.

<p class="overwrite-class">Varbox\Middleware\RedirectRequests</p>

Found in `config/varbox/bindings.php` at `middleware.redirect_requests_middleware` key.   
This middleware can be used by you to manage your redirects at application level.

<p class="overwrite-class">Varbox\Filters\RedirectFilter</p>

Found in `config/varbox/bindings.php` at `filters.redirect_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\RedirectSort</p>

Found in `config/varbox/bindings.php` at `sorts.redirect_sort` key.   
This class is used for applying the sorting logic.
