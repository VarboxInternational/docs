# Dynamic Menus

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Menu Locations](#menu-locations)
    - [Url Type Menus](#url-type-menus)
    - [Route Type Menus](#route-type-menus)
    - [Model Type Menus](#model-type-menus)
- [Usage](#usage)
    - [Display Menu Items](#display-menu-items)
    - [Create New Menu Location](#create-new-menu-location)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The `menus` component is a very powerful content management functionality that allows you to quickly create menu items for different page locations and assign them dynamically .

> The menu component uses [kalnoy/nestedset](https://github.com/lazychaser/laravel-nestedset) as an underlying dependency for working with the tree structure, so you should also inspect their documentation in order to see how you can leverage different methods or query scopes they expose.

In order to provide you with a complex crud functionality inside the admin, the menus crud implements the following out of the box:

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

Before you learn about using your menus inside a [VarBox](/) application, you should know that there's already a section in the admin from where you can manage all your menus.

You can find the menus section inside [Admin -> Manage Content -> Menus](/docs/{{version}}/menus-interface).   
Feel free to explore all available options this section offers.

![Menus List](/docs/menus-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the architecture behind dynamic menus and their workflow.

<a name="the-workflow"></a>
#### The Workflow

- Select a menu location from `Admin -> Manage Content -> Menus` 
- Create a menu item from `Admin -> Manage Content -> Menus (Location)`
- Fetch your location menu items (eg. in a view composer)
- Render the proper html looping through your menu items (eg. in your view)

<a name="menu-locations"></a>
#### Menu Locations

Menu locations represent sections from your actual frontend layout where menu items can exist.   
By default, the [VarBox](/) platform exposes two menu locations: `header` and `footer`

If you want to customize this, you can do so by modifying the `config/varbox/menus.php` config file, specifically the `locations` section.

<a name="url-type-menus"></a>
#### Url Type Menus

You can specify the type of your menu item when creating or updating it.

The `url` type is the simplest type, meaning you only have to hardcode a value in the url field from the admin. It can be either internal or external.

<a name="route-type-menus"></a>
#### Route Type Menus

You can specify the type of your menu item when creating or updating it.

If you select the `route` type you'll be able to assign to your url field, a route that:
- has a name
- supports the `get` http verb
- implements the `web` middleware
- is not an `admin` route
- doesn't have any custom parameters

<a name="route-type-menus"></a>
#### Model Type Menus

You can specify the type of your menu item when creating or updating it.

Besides the `url` and `route` types, you can also assign a `urlable model` to your menu item, to fetch the url from the model record itself.

> This will only work for models that implement the `Varbox\Traits\HasUrl` trait.

To make the menus component aware of your `urlable models`, thus populating the select from the admin, you can add your classes inside the `config/varbox/menus.php`, specifically inside the `types` section.

<a name="usage"></a>
## Usage

<a name="display-menu-items"></a>
#### Display Menu Items

Let's say you want to build your header menu in your frontend. There's a number of ways to go about it, but this example will illustrate achieving that by using a `view composer` and a `blade file`

Create a `App\Http\Composers\HeaderMenuComposer` view composer:

```php
<?php

namespace App\Http\Composers;

use Illuminate\View\View;

class HeaderMenuComposer
{
    /**
     * @param View $view
     */
    public function compose(View $view)
    {
        $menus = Menu::whereLocation('header')
            ->onlyActive()
            ->whereIsRoot()
            ->defaultOrder()
            ->get();

        $view->with([
            'menus' => $menus,
        ]);
    }
}
```

Create a `partials/menus/header.blade.php` blade file:

```php
<ul>
    @foreach($menus as $menu)
        <li>
            <a href="{{ $menu->url }}" @if($menu->data['new_window'] ?? null) target="_blank" @endif>
                {{ $menu->name }}
            </a>
            
            @if($menu->children->isNotEmpty())
                <ul>
                    @foreach($menu->children as $child)
                        <li>
                            <a href="{{ $child->url }}" @if($child->data['new_window'] ?? null) target="_blank" @endif>
                                {{ $child->name }}
                            </a>
                        </li>
                    @endforeach
                </ul>
            @endif
        </li>
    @endforeach
</ul>
```

Register the view composer in your service provider:

```php
View::composer('partials.menus.header', 'App\Http\Composers\HeaderMenuComposer');
```

<a name="create-new-menu-location"></a>
#### Create New Menu Location

To create a new menu location, go to `config/varbox/menus.php` and under the `locations` section, add your new location. 
The newly created menu location will then be available inside the admin, when creating / updating a menu.

You should create a new menu location only if you need to display menu items in a different location in your frontend.

```php
'locations' => [
    ..., 'your-location',
],
```

<a name="configuration"></a>
## Configuration

The menus configuration file is located at `config/varbox/menus.php`.

For more information on how you can customize the menus functionality, please read the comments from that file.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
[VarBox](/) makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

The `menu` classes available for binding overwrites are:

<style>
    span.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
    }
</style>

<span class="overwrite-class">Varbox\Models\Menu</span>

Found in `config/varbox/bindings.php` at `models.menu_model` key.   
This class represents the menu model.

<span class="overwrite-class">Varbox\Controllers\MenusController</span>

Found in `config/varbox/bindings.php` at `controllers.menus_controller` key.   
This class is used for interactions with the `Admin -> Manage Content -> Menus` section.

<span class="overwrite-class">Varbox\Controllers\MenusTreeController</span>

Found in `config/varbox/bindings.php` at `controllers.menus_tree_controller` key.   
Used for interactions with the `menus tree` from the `Admin -> Manage Content -> Menus` section.

<span class="overwrite-class">Varbox\Requests\MenuRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.menu_form_request` key.   
This class is used for validating any menu when creating or updating.
