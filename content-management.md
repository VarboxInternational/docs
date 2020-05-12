# Content Management

- [Admin Interface](#admin-interface)
- [Content Pages](#content-pages)
    - [How Pages Work](#how-pages-work)
    - [Display Frontend Page](#display-frontend-page)
    - [Create New Page Type](#create-new-page-type)
    - [Add Extra Page Fields](#add-extra-page-fields)
    - [Manage Page Uploads](#manage-page-uploads)
    - [Render Location Blocks](#render-page-location-blocks)
- [Custom Menus](#custom-menus)
    - [How Menus Work](#how-menus-work)
    - [Display Menu Items](#display-menu-items)
    - [Create New Menu Location](#create-new-menu-location)
- [Content Blocks](#content-blocks)
    - [How Blocks Work](#how-blocks-work)
    - [Generate New Block](#generate-new-block)
    - [Enable Blocks For Custom Entities](#enable-blocks-for-custom-entities)
    - [Display Assigned Blocks](#display-assigned-blocks)
    - [Useful Block Methods](#useful-block-methods)
- [Custom Emails](#custom-emails)
    - [How Emails Work](#how-emails-work)
    - [Create New Mailable](#create-new-mailable)
    - [Use Custom Variables](#use-custom-variables)
    - [Send A Custom Email](#send-a-custom-email)
    - [Customize Email Design](#customize-email-design)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The content management module is a powerful tool that offers you the following features out of the box:
- create content pages on the fly to display your structured content
- assign and order individual pieces of content in a page
- customize your menus present in your layouts
- use mailables on steroids with the power of dynamically modify their contents

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your content.

<a name="pages-interface"></a>
#### Pages Interface

You can find the pages section inside [Admin -> Manage Content -> Pages](/docs/{{version}}/pages-interface).   
Feel free to explore all available options this section offers.

![Pages List](/docs/{{version}}/pages-list.png)

<a name="menus-interface"></a>
#### Menus Interface

You can find the menus section inside [Admin -> Manage Content -> Menus](/docs/{{version}}/menus-interface).   
Feel free to explore all available options this section offers.

![Menus List](/docs/{{version}}/menus-list.png)

<a name="blocks-interface"></a>
#### Blocks Interface

You can find the blocks section inside [Admin -> Manage Content -> Blocks](/docs/{{version}}/blocks-interface).   
Feel free to explore all available options this section offers.

![Blocks List](/docs/{{version}}/blocks-list.png)

<a name="emails-interface"></a>
#### Emails Interface

You can find the emails section inside [Admin -> Manage Content -> Emails](/docs/{{version}}/emails-interface).   
Feel free to explore all available options this section offers.

![Emails List](/docs/{{version}}/emails-list.png)

<a name="content-pages"></a>
## Content Pages

The pages component is a very powerful content management functionality that allows you to quickly convert a static Laravel page into a dynamic page that can be modified from the admin panel.

In order to provide you with a complex crud functionality inside the admin, the pages crud implements the following out of the box:

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

[Upload Files](/docs/{{version}}/upload-files)
[Model Revisions](/docs/{{version}}/model-revisions)
[Model Urls](/docs/{{version}}/model-urls)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)
[Content Blocks](/docs/{{version}}/content-blocks)
[Meta Tags](/docs/{{version}}/meta-tags)

</div>

<a name="how-pages-work"></a>
### How Pages Works

First of all, let's understand the architecture behind content pages and their workflow.

<a name="the-pages-workflow"></a>
#### The Workflow

- Create a page from `Admin -> Manage Content -> Pages` and access its url
- The route will be dispatched to `App\Http\Controllers\PagesController@show`
- Initially you'll get an error saying `pages.show` view doesn't exist
- From here it's all up to you to create the view and display your details

<a name="pages-configuration-file"></a>
#### Configuration File

The pages component comes bundled with a `config/varbox/pages.php` config file.

From here you can specify what page types your application contains and a custom upload configuration for pages only, if you happen to use file uploads.

> You should specify a new page type only if you want to dispatch the request to another controller or action, or if you have different block locations in that page.
> <br /><br />
> You should specify a custom upload config only if your pages use file uploads and you want to overwrite the default configuration.

<a name="pages-controller-dispatcher"></a>
#### Controller Dispatcher

By default the `config/varbox/pages.php` config file comes with a single page type called `default` so any pages created from the admin will point to `App\Http\Controllers\PagesController@show`

You can customize to what controller and method to dispatch the route of a page, either by modifying the `default` type configuration or by adding a new `page type`

> Specifying custom route dispatches for some of your pages might be a good idea if you have different content pages that also hold different additional logic, thus keeping your controllers and methods clean.

<a name="display-frontend-page"></a>
### Display Frontend Page

After you've created a page from inside the admin, it's time to display its contents in your frontend.

Initially if you access that page's url in your browser, you'll get an error saying that `pages.show` view doesn't exist. 
This happens because behind the scenes, when you're accessing that url, the request is dispatched to `App\Http\Controllers\PagesController` specifically to the `show` method.

For more information, you can inspect that controller in order to fully grasp what it does.

All you have to do in order to display that page's information in your frontend is to create the `resources/views/pages/show.blade.php` view file.

```php
<h1>{{ $page->data['title'] }}</h1>
<h2>{{ $page->data['subtitle'] }}</h2>

{!! $page->data['content'] !!}
```

<a name="create-new-page-type"></a>
### Create New Page Type

To create a new page type, go to `config/varbox/pages.php` and under the `types` section, add your new page type  configuration. 
The newly created page type will then be available inside the admin, when creating / updating a page.

You should create a new page type only if you want a different route dispatch or if you plan on using different or no block locations.

```php
'types' => [
    ...

    'your-type' => [
        'controller' => '\App\Http\Controllers\YourController',
        'action' => 'yourMethod',
        'locations' => [
            'yourBlockLocation1', 'yourBlockLocation2'
        ]
    ],

],
```

After this, you just have to create the specified `controller` with the specified `method` that will contain your logic.

<a name="add-extra-page-fields"></a>
### Add Extra Page Fields

More often than not, in your applications you'll want additional fields for your pages.

By default, the pages component exposes the "title", "subtitle" and "content" fields.   
Those fields are all part of the `data` column from the `pages` database table.

<a name="extra-page-fields-inside-the-admin"></a>
#### Inside The Admin

To add or remove fields for pages, you can just modify the blade view file with your extra fields (make them part of the `data` column).
The blade file you should modify is located at: `resources/views/vendor/varbox/admin/pages/_form.blade.php`

```php
<div class="col-md-12">
    {!! form_admin()->text('data[author]', 'Author') !!}
</div>
<div class="col-md-12">
    {!! form_admin()->textarea('data[intro]', 'Intro') !!}
</div>
```

<a name="extra-page-fields-in-your-frontend"></a>
#### In Your Frontend

You can then reference those fields in your `resources/views/pages/show.blade.php` like this:

```php
<span>{{ $page->data['author'] }}</span>
<p>{{ $page->data['intro'] }}</p>
```

The above method for adding extra fields was the easy one without no hassle on your part, but if for some reason you actually need additional table columns for pages, you can create a `migration` to add those fields and then [extend](#overwrite-bindings) the `Varbox\Models\Page` model with your own model to make those fields `fillable`

<a name="manage-page-uploads"></a>
### Manage Page Uploads

Besides some basic extra fields, you might need file uploads for your pages component.

> If the below code doesn't seem familiar, you might want to take a look at the [Upload Files](/docs/{{version}}/upload-files) documentation section.

<a name="page-uploads-inside-the-admin"></a>
#### Inside The Admin

To add file uploads for your pages, you can just modify the blade view file with your fields (make them part of the `data` column).
The blade file you should modify is located at: `resources/views/vendor/varbox/admin/pages/_form.blade.php`

```php
<div class="col-md-12">
    {!! uploader()->field('data[image]')->label('Image')->model($item)->manager() !!}
</div>
<div class="col-md-12">
    {!! uploader()->field('data[video]')->label('Video')->model($item)->manager() !!}
</div>
```

<a name="page-uploads-in-your-frontend"></a>
#### In Your Frontend

You can then reference those fields in your `resources/views/pages/show.blade.php` by using the `uploaded()` helper method.

```php
<img src="{{ uploaded($page->data['image'])->url() }}" />

<video controls>
    <source src="{{ uploaded($page->data['video'])->url() }}" type="video/mp4">
</video>
```

You can also cusomize the upload configuration from inside the `config/varbox/pages` config file, specifically from the `upload` section.

<a name="render-page-location-blocks"></a>
### Render Location Blocks

As you've probably noticed, the admin crud for pages also supports assigning blocks in different locations for a page.

> To learn more about blocks, please see the [Content Blocks](/docs/{{version}}/content-blocks) documentation section.

Assuming you've already created some blocks and assigned them in different locations on your page from the admin, you can render all the blocks assigned to a location by leveraging the `render()` method from the `Varbox\Helpers\BlockHelper` class.

```php
{!! block()->render($page, 'your_location') !!}
``` 

By applying this line of code you'll be iterating through your assigned blocks from a given location and display their html.

<a name="custom-menus"></a>
## Custom Menus

The menus component is a very powerful content management functionality that allows you to quickly create menu items for different page locations and assign them dynamically .

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

<a name="how-menus-work"></a>
### How Menus Work

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

<a name="display-menu-items"></a>
### Display Menu Items

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
### Create New Menu Location

To create a new menu location, go to `config/varbox/menus.php` and under the `locations` section, add your new location. 
The newly created menu location will then be available inside the admin, when creating / updating a menu.

You should create a new menu location only if you need to display menu items in a different location in your frontend.

```php
'locations' => [
    ..., 'your-location',
],
```

<a name="content-blocks"></a>
## Content Blocks

The blocks component is a very powerful content management functionality that allows you to quickly assign pieces of dynamic content to your content pages or other custom entities of yours that support block assignment.

In order to provide you with a complex crud functionality inside the admin, the blocks crud implements the following out of the box:

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

[Upload Files](/docs/{{version}}/upload-files)
[Model Revisions](/docs/{{version}}/model-revisions)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-blocks-work"></a>
### How Blocks Work

First of all, let's understand the architecture behind content blocks and their workflow.

<a name="the-workflow"></a>
#### The Workflow

- Create the block type inside `config/varbox/blocks.php` config file
- Generate the block files by running the `varbox:make-block` artisan command
- Create a block from "Admin -> Manage Content -> Blocks"
- Assign the block to a page or a custom entity
- Render the block's content in your frontend page

<a name="configuration-file"></a>
#### Configuration File

The blocks component comes bundled with a `config/varbox/blocks.php` config file.

From here you can specify what block types your application contains and a custom upload configuration for blocks only, if you happen to use file uploads.

> You should specify a new block type for any different piece of content present in your frontend, that you wish to treat as a content block.
> <br /><br />
> You should specify a custom upload config only if at least of one of your block types uses file uploads and you want to overwrite the default configuration.

<a name="the-block-files"></a>
#### The Block Files

Running the artisan command for making blocks, will generate all necessary files for a block inside the `app/Blocks/{YourType}` directory. The generated files for each block type are:

<style>
    span.file-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 13px;
        color: #239cdd;
    }
</style>

<span class="file-class">app/Blocks/{YourType}/Composer.php</span>

This is the view composer for the block. In this view composer you can also add any additional logic your block type might have. It already exposes to the view the following variables:
- `$block` representing the block model instance
- `$data` representing the block's information in an array structured format

The view composer is automatically bound to the `front.blade.php` block view file, so you don't have to worry about that, it just works.

<span class="file-class">app/Blocks/{YourType}/Views/front.blade.php</span>

This is the actual blade view that will be rendered where you assign your block. In here, you should add the code for rendering your actual piece of content.

<span class="file-class">app/Blocks/{YourType}/Views/admin.blade.php</span>

This blade will be used inside the admin panel when you add / edit a block of this type. In here, you should add your fields that are specific for this block type.

<a name="generate-new-block"></a>
### Generate New Block

In order to be able to create blocks from the admin panel and assign them to your entities, you first need to define the block types your application has.

<a name="define-the-block--type"></a>
#### Define The Block Type

Inside the `config/varbox/blocks.php` config file, specify your `block type`

```php
'types' => [

    'YourType' => [

        // The pretty formatted block type name. 
        // This is mainly used inside the admin panel, in places that reference blocks.
        'label' => 'Your Block Type',

        // The full namespace to the block's view composer. 
        // Each block you create will have a view composer. 
        // You can define any extra logic in the block's view composer class.
        'composer_class' => 'App\Blocks\YourType\Composer',

        // The full path to the block's views directory. 
        // When creating a new block type, you will also have two views (front & admin).
        'views_path' => 'app/Blocks/YourType/Views',

        // The name of the image used as block type preview in admin. 
        // This should contain the full path to an image inside the "public/" directory. 
        // The path is relative two the "public/" directory.
        'preview_image' => 'images/blocks/your-type.jpg',
    ],

],
```

<a name="run-the-block-command"></a>
#### Run The Command

After you've defined your block type, you can generate the block files by running the below command. Please note that the command accepts a single argument, which is your block type.

```
php artisan varbox:make-block YourType
```

Running the command, will generate all necessary files for a block inside the `app/Blocks` directory.   
> Check out [the block files](#the-block-files) section to see what those files are and how they're being used.

<a name="block-uploads"></a>
#### Block Uploads

If some of your blocks require file uploads (eg. images), please know that you can overwrite their specific configuration from inside the `config/varbox/blocks` config file, specifically from the `upload` section.

> To learn more about model specific upload configuration, please read this [documentation section](/docs/{{version}}/upload-files#specific-model-configurations).

<a name="enable-blocks-for-custom-entities"></a>
### Enable Blocks For Custom Entities

As you've probably noticed by now, after generating a block and creating a record from the admin panel, you can instantly assign it to a page.
 
But what about adding the block assignment functionality to other custom entities of yours, such as a blog post?
Let's learn how you can achieve this.

<a name="apply-the-bock-trait"></a>
#### Apply The Trait

Your models should use the `Varbox\Traits\HasBlocks` trait and the `Varbox\Options\BlockOptions` class. 
The trait contains an abstract method `getBlockOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\BlockOptions;
use Varbox\Traits\HasBlocks;

class YourModel extends Model
{
    use HasBlocks;

    /**
     * Set the options for the HasBlocks trait.
     *
     * @return BlockOptions
     */
    public function getBlockOptions(): BlockOptions
    {
        return BlockOptions::instance()
            ->withLocations(['content', 'sidebar']);
    }
}
```

<a name="add-block-blade-code"></a>
#### Add Blade Code

Inside your `edit` blade file, add the following code responsible for generating the block assignment html component.

```php
<div class="col-md-12">
    ...
</div>

@if($item->exists)
    @include('varbox::helpers.block.container', [
        'model' => $item, 
        'revision' => $revision ?? null
    ])
@endif
```

<a name="add-block-controller-code"></a>
#### Add Controller Code

Finally, you'll want to actually save the assigned blocks for your entity, so inside your `admin` controller, specifically in the `update` method, use the `saveBlocks()` method to save your assigned blocks.

```php
public function update(Request $request, YourModel $item)
{
    return $this->_update(function () use ($request, $item) {
        ...

        $item->saveBlocks($request->input('blocks') ?: []);

        ...
    }, $request);
}
```

That's it! Now you can assign blocks for your custom entities and display them in your frontend views.

<a name="display-assigned-blocks"></a>
### Display Assigned Blocks

After you've generated a block, created an entry for it in the admin and assign it to your entities, it's time to actually display the block's contents in your frontend.

You can display all assigned blocks from a location by using the `renderBlocks()` method present on the `HasBlocks` trait, inside your frontend blade view.

```php
{!! $item->renderBlocks('content') !!}
```

This line of code will output the concatenated parsed html for all blocks assigned to the "content" location. 
That method uses the `app/Blocks/{YourType}/front.blade.php` of each block type as the html source, so make sure you update those files accordingly.

<a name="useful-block-methods"></a>
### Useful Block Methods

If you want to manually leverage the `Varbox\Traits\HasBlocks` trait's capabilities, read on.

<a name="fetch-blocks"></a>
#### Fetch Blocks

You can fetch a model record's blocks by using the `revisions()` morph many relation present on the `Varbox\Traits\HasBlocks` trait.

```php
$model = YourModel::find($id);
$blocks = $model->blocks;
```

<a name="fetch-locations"></a>
#### Fetch Locations

You can fetch a model's available block locations by using the `getBlockLocations()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$locations = app(YourModel::class)->getBlockLocations();
```

<a name="get-blocks-in-location"></a>
#### Get Blocks In Location

You can get all assigned blocks in a certain location for a model record by using the `getBlocksInLocation()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$model = YourModel::find($id);
$blocks = $model->getBlocksInLocation('content');
```

<a name="get-blocks-of-location"></a>
#### Get Blocks Of Location

You can get all potential blocks that could be assigned to a location by using the `getBlocksOfLocation()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$blocks = app(YourModel::class)->getBlocksOfLocation();
```

<a name="sync-blocks"></a>
#### Sync Blocks

You can sync all assigned blocks for a model record by using the `saveBlocks()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$blocks = [
    [
        $blockIdOne => [$blockLocationOne, $blockOrderOne]
    ],
    [
        $blockIdTwo => [$blockLocationTwo, $blockOrderTwo]
    ],
    ...
];

$item = YourModel::find($id);
$item->saveBlocks($blocks);
```

<a name="assign-blocks"></a>
#### Assign Block

You can assign a block for a model record by using the `assignBlock()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$item = YourModel::find($id);
$item->assignBlock($blockModel, $blockLocation, $blockOrder ?? null);
```

<a name="unassign-blocks"></a>
#### Unassign Block

You can unassign a block for a model record by using the `unassignBlock()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$item = YourModel::find($id);
$item->unassignBlock($blockModel, $blockLocation, $blockPivotId);
```

<a name="custom-emails"></a>
## Custom Emails

The emails component is a very powerful content management functionality that allows you to quickly customize a mailable with custom details and variable oriented values.

In order to provide you with a complex crud functionality inside the admin, the emails crud implements the following out of the box:

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

[Upload Files](/docs/{{version}}/upload-files)
[Model Revisions](/docs/{{version}}/model-revisions)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-emails-work"></a>
### How Emails Work

First of all, let's understand the architecture behind content blocks and their workflow.

<a name="the-workflow"></a>
#### The Workflow

- Create the email type inside `config/varbox/emails.php` config file
- Generate the mailable files by running the `varbox:make-mail` artisan command
- Create an email from "Admin -> Manage Content -> Emails"
- Send the email

<a name="configuration-file"></a>
#### Configuration File

The emails component comes bundled with a `config/varbox/emails.php` config file.

From here you can specify what email types your application contains and what custom variables your emails can make use of.

> You should specify a new email type for each mailable your application will use. For each email type, you can create only one email record inside the admin.
> <br /><br />
> You should keep the variables section up to date with all the variables you're using inside all your mailable classes.

<a name="mailable-files"></a>
#### Mailable Files

Running the artisan command for generating custom emails, will generate all necessary files for a mailable.The generated files for each email type are:

<style>
    span.file-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 13px;
        color: #239cdd;
    }
</style>

<span class="file-class">app/Mail/{YourMailable}.php</span>

This is the custom mailable class. This class extends the  `Varbox\Mail\Mailable` abstract class, which in turn extends the `Illuminate\Mail\Mailable` class, so you have available any functionality a basic Laravel mailable offers.

With that said, the actual generated mailable class only exposes 3 methods, in order to make your job of creating a mailable easier.

<span class="file-class">resources/views/emails/{your_mailable}.blade.php</span>  

This is the mailable view that will be used to actual render the sent email. The custom emails use the markdown template. 

In the majority of the cases, modifying this file won't be necessary, since you can dynamically change your email's content from the admin panel.

<a name="custom-variables"></a>
#### Custom Variables

Beside the fact that you can have emails with a customizable subject, content or even an attachment, you can even leverage custom variables to display, for example, a user's name in your email's content.

You should define your custom variables inside the `config/varbox/emails.php` config file and then, inside the `mapVariables()` method from your mailable class you should actually provide a value for your variables.

> To specify custom variables for your emails' content inside the admin, wrap your variable name in square brackets: **[username]**

<a name="create-new-mailable"></a>
### Create New Mailable

First, you need to define the `type` of the email inside the `config/varbox/emails.php` config file.

```php
'types' => [

    'your-mail' => [

        // The mailable class used for sending the email.
        // This gets generated automatically when running 
        // "php artisan varbox:make-mail"
        'class' => 'App\Mail\YourMail',

        // The blade file used for rendering the email.
        // The value will be relative to the "resources/views/" directory.
        'view' => 'emails.your_mail',

        // Array of variables that the respective mail type is allowed to use.
        // You can customize these variables's values inside your mailable class.
        'variables' => [
            'your_variable_one', 'your_variable_two'
        ],
    ],

],
```

After you've defined your email type, you can generate the mailable files by running the below command. Please note that the command accepts a single argument, which is your email type.

```
php artisan varbox:make-mail your-mail
```

That's it! Now you have two files for your newly created email:
- `app/Mail/YourMail.php`: which is the mailable class
- `resources/views/emails/your_mail.blade.php`: which is the mailable view

<a name="use-custom-variables"></a>
### Use Custom Variables

Inside your generated mailable class you'll see a method called `mapVariables()` that by default returns an array of your custom variables that you've setup in your `config/varbox/emails.php` for that type.

To specify a dynamic value for your custom variables, simply modify that array with the actual values for each of the variables.

```php
/**
 * Return an array containing:
 * - keys: each variable name as defined inside the "config/varbox/emails.php" -> "variables" config option
 * - values: the actual value that the respective variable (key) should be transformed to
 *
 * @return array
 */
protected function mapVariables()
{
    return [
        'username' => $this->user->name,
        'company' => config('app.name'),

   ];
}
```

After you've defined the dynamic values for your custom variables, you can reference them from inside the admin, when creating or updating an email like so:

```
Hi [username]

Welcome to our site!

Regards, [company] team
```

<a name="send-a-custom-email"></a>
### Send A Custom Email

Given that these custom emails the [VarBox](/) platform provides finally extend the Laravel's `Illuminate\Mail\Mailable` class, you can send these emails like you always did inside a Laravel application.

```php
use App\Mail\YourMail;

Mail::to($request->user())->send(new YourMail);
```

<a name="customize-email-design"></a>
### Customize Email Design

In order to change the design of your emails, please follow this Laravel [documentation section](https://laravel.com/docs/7.x/mail#customizing-the-components)

<a name="configuration"></a>
## Configuration

For more information on how you can customize the content management components, please read the comments from their configuration files.

<a name="pages-configuration"></a>
#### Pages Configuration

The pages configuration file is located at `config/varbox/pages.php`.

<a name="menus-configuration"></a>
#### Menus Configuration

The menus configuration file is located at `config/varbox/menus.php`.

<a name="blocks-configuration"></a>
#### Blocks Configuration

The blocks configuration file is located at `config/varbox/blocks.php`.

<a name="emails-configuration"></a>
#### Emails Configuration

The emails configuration file is located at `config/varbox/emails.php`.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
[VarBox](/) makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

<style>
    span.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
    }
</style>

<a name="page-bindings"></a>
### Page Bindings

The `page` classes available for binding overwrites are:

<span class="overwrite-class">Varbox\Models\Page</span>

Found in `config/varbox/bindings.php` at `models.page_model` key.   
This class represents the page model.

<span class="overwrite-class">Varbox\Controllers\PagesController</span>

Found in `config/varbox/bindings.php` at `controllers.pages_controller` key.   
This class is used for interactions with the Admin -> Manage Content -> Pages section.

<span class="overwrite-class">Varbox\Controllers\PagesTreeController</span>

Found in `config/varbox/bindings.php` at `controllers.pages_tree_controller` key.   
Used for interactions with the pages tree from the Admin -> Manage Content -> Pages section.

<span class="overwrite-class">Varbox\Requests\PageRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.page_form_request` key.   
This class is used for validating any page when creating or updating.

<a name="menu-bindings"></a>
### Menu Bindings

The `menu` classes available for binding overwrites are:

<span class="overwrite-class">Varbox\Models\Menu</span>

Found in `config/varbox/bindings.php` at `models.menu_model` key.   
This class represents the menu model.

<span class="overwrite-class">Varbox\Controllers\MenusController</span>

Found in `config/varbox/bindings.php` at `controllers.menus_controller` key.   
This class is used for interactions with the Admin -> Manage Content -> Menus section.

<span class="overwrite-class">Varbox\Controllers\MenusTreeController</span>

Found in `config/varbox/bindings.php` at `controllers.menus_tree_controller` key.   
Used for interactions with the menus tree from the Admin -> Manage Content -> Menus section.

<span class="overwrite-class">Varbox\Requests\MenuRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.menu_form_request` key.   
This class is used for validating any menu when creating or updating.


<a name="block-bindings"></a>
### Block Bindings

The `block` classes available for binding overwrites are:

<span class="overwrite-class">Varbox\Models\Block</span>

Found in `config/varbox/bindings.php` at `models.block_model` key.   
This class represents the block model.

<span class="overwrite-class">Varbox\Controllers\BlocksController</span>

Found in `config/varbox/bindings.php` at `controllers.blocks_controller` key.   
This class is used for interactions with the Admin -> Manage Content -> Pages section.

<span class="overwrite-class">Varbox\Requests\BlockRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.block_form_request` key.   
This class is used for validating any block when creating or updating.


<a name="email-bindings"></a>
### Email Bindings

The `email` classes available for binding overwrites are:

<span class="overwrite-class">Varbox\Models\Email</span>

Found in `config/varbox/bindings.php` at `models.email_model` key.   
This class represents the email model.

<span class="overwrite-class">Varbox\Controllers\EmailsController</span>

Found in `config/varbox/bindings.php` at `controllers.emails_controller` key.   
This class is used for interactions with the Admin -> Manage Content -> Emails section.

<span class="overwrite-class">Varbox\Requests\EmailRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.email_form_request` key.   
This class is used for validating any email when creating or updating.

<a name="full-example"></a>
## Full Example

For an example on how to setup a frontent page supporting menus and blocks, please follow this [Full Example](/docs/{{version}}/example-page-blocks).
