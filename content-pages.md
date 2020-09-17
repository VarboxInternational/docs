<h1>Content Pages</h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Configuration File](#configuration-file)
    - [Controller Dispatcher](#controller-dispatcher)
- [Usage](#usage)
    - [Display Frontend Page](#display-frontend-page)
    - [Create New Page Type](#create-new-page-type)
    - [Add Extra Page Fields](#add-extra-page-fields)
    - [Manage Page Uploads](#manage-page-uploads)
    - [Render Location Blocks](#render-page-location-blocks)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The pages component is a very powerful content management functionality that allows you to quickly convert a static Laravel page into a dynamic page that can be modified from the admin panel.
</p>

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

[Upload Files](/docs/{{version}}/file-uploads)
[Model Revisions](/docs/{{version}}/model-revisions)
[Model Urls](/docs/{{version}}/model-urls)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)
[Csv Exports](/docs/{{version}}/csv-exports)
[Content Blocks](/docs/{{version}}/content-blocks)
[Meta Tags](/docs/{{version}}/meta-tags)

</div>

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your pages.

You can find the pages section inside **Admin -> Manage Content -> Pages**.   
Feel free to explore all available options this section offers.

![Pages List](/docs/{{version}}/pages-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the architecture behind content pages and their workflow.

<a name="the-workflow"></a>
#### The Workflow

- Create a page from "Admin -> Manage Content -> Pages" and access its url
- The route will be dispatched to `App\Http\Controllers\PagesController@show`
- Initially you'll get an error saying `pages.show` view doesn't exist
- From here it's all up to you to create the view and display your details

<a name="configuration-file"></a>
#### Configuration File

The pages component comes bundled with a `config/varbox/pages.php` config file.

From here you can specify what page types your application contains and a custom upload configuration for pages only, if you happen to use file uploads.

> You should specify a new page type only if you want to dispatch the request to another controller or action, or if you have different block locations in that page.
> <br /><br />
> You should specify a custom upload config only if your pages use file uploads and you want to overwrite the default configuration.

<a name="controller-dispatcher"></a>
#### Controller Dispatcher

By default the `config/varbox/pages.php` config file comes with a single page type called `default` so any pages created from the admin will point to `App\Http\Controllers\PagesController@show`

You can customize to what controller and method to dispatch the route of a page, either by modifying the `default` type configuration or by adding a new `page type`

> Specifying custom route dispatches for some of your pages might be a good idea if you have different content pages that also hold different additional logic, thus keeping your controllers and methods clean.

<a name="usage"></a>
## Usage

Let's see the most common things you can do using content pages.

<a name="display-frontend-page"></a>
#### Display Frontend Page

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
#### Create New Page Type

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
#### Add Extra Page Fields

More often than not, in your applications you'll want additional fields for your pages.

By default, the pages component exposes the "title", "subtitle" and "content" fields.   
Those fields are all part of the `data` column from the `pages` database table.

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

You can then reference those fields in your `resources/views/pages/show.blade.php` like this:

```php
<span>{{ $page->data['author'] }}</span>
<p>{{ $page->data['intro'] }}</p>
```

The above method for adding extra fields was the easy one without no hassle on your part, but if for some reason you actually need additional table columns for pages, you can create a `migration` to add those fields and then [extend](#overwrite-bindings) the `Varbox\Models\Page` model with your own model to make those fields `fillable`

<a name="manage-page-uploads"></a>
#### Manage Page Uploads

Besides some basic extra fields, you might need file uploads for your pages component.

> If the below code doesn't seem familiar, you might want to take a look at the [Upload Files](/docs/{{version}}/file-uploads) documentation section.

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

You can then reference those fields in your `resources/views/pages/show.blade.php` by using the `uploaded()` helper method.

```php
<img src="{{ uploaded($page->data['image'])->url() }}" />

<video controls>
    <source src="{{ uploaded($page->data['video'])->url() }}" type="video/mp4">
</video>
```

You can also cusomize the upload configuration from inside the `config/varbox/pages` config file, specifically from the `upload` section.

<a name="render-page-location-blocks"></a>
#### Render Location Blocks

As you've probably noticed, the admin crud for pages also supports assigning blocks in different locations for a page.

> To learn more about blocks, please see the [Content Blocks](/docs/{{version}}/content-blocks) documentation section.

Assuming you've already created some blocks and assigned them in different locations on your page from the admin, you can render all the blocks assigned to a location by leveraging the `render()` method from the `Varbox\Helpers\BlockHelper` class.

```php
{!! block()->render($page, 'your_location') !!}
``` 

By applying this line of code you'll be iterating through your assigned blocks from a given location and display their html.

<a name="configuration"></a>
## Configuration

The pages configuration file is located at `config/varbox/pages.php`.

For more information on how you can customize the content management components, please read the comments from their configuration files.

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

The page classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Page</p>

Found in `config/varbox/bindings.php` at `models.page_model` key.   
This class represents the page model.

<p class="overwrite-class">Varbox\Controllers\PagesController</p>

Found in `config/varbox/bindings.php` at `controllers.pages_controller` key.   
This class is used for interactions with the "Admin -> Manage Content -> Pages" section.

<p class="overwrite-class">Varbox\Controllers\PagesTreeController</p>

Found in `config/varbox/bindings.php` at `controllers.pages_tree_controller` key.   
Used for interactions with the pages tree from the "Admin -> Manage Content -> Pages" section.

<p class="overwrite-class">Varbox\Requests\PageRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.page_form_request` key.   
This class is used for validating any page when creating or updating.

<p class="overwrite-class">Varbox\Filters\PageFilter</p>

Found in `config/varbox/bindings.php` at `filters.page_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\PageSort</p>

Found in `config/varbox/bindings.php` at `sorts.page_sort` key.   
This class is used for applying the sorting logic.

