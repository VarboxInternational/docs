<h1>Content Blocks</h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Configuration File](#configuration-file)
    - [Generated Files](#generated-files)
- [Generate New Block](#generate-new-block)
    - [Define The Type](#define-the-type)
    - [Run The Command](#run-the-command)
    - [Manage Uploads](#manage-uploads)
- [Custom Entity Blocks](#custom-entity-blocks)
    - [Apply The Trait](#apply-the-trait)
    - [Add Blade Code](#add-blade-code)
    - [Add Controller Code](#add-controller-code)
- [Display Assigned Blocks](#display-assigned-blocks)
- [Useful Methods](#useful-methods)
    - [Fetch Blocks](#fetch-blocks)
    - [Fetch Locations](#fetch-locations)
    - [Get Blocks In Location](#get-blocks-in-location)
    - [Get Blocks Of Location](#get-blocks-of-location)
    - [Sync Blocks](#sync-blocks)
    - [Assign Block](#assign-block)
    - [Un-assign Block](#unassign-block)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The blocks component is a very powerful content management functionality that allows you to quickly assign pieces of dynamic content to your content pages or other custom entities of yours that support block assignment.
</p>

In order to provide you with a complex crud functionality inside the admin, the blocks crud implements the following out of the box:

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

[Upload Files](/docs/{{version}}/file-uploads)
[Model Revisions](/docs/{{version}}/model-revisions)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)
[Csv Exports](/docs/{{version}}/csv-exports)

</div>

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your content.

You can find the blocks section inside **Admin -> Manage Content -> Blocks**.   
Feel free to explore all available options this section offers.

![Blocks List](/docs/{{version}}/blocks-list.png)

<a name="how-it-works"></a>
## How It Works

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

<a name="generated-files"></a>
#### Generated Files

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
## Generate New Block

In order to be able to create blocks from the admin panel and assign them to your entities, you first need to define the block types your application has.

<a name="define-the-type"></a>
#### Define The Type

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

<a name="run-the-command"></a>
#### Run The Command

After you've defined your block type, you can generate the block files by running the below command. Please note that the command accepts a single argument, which is your block type.

```
php artisan varbox:make-block YourType
```

Running the command, will generate all necessary files for a block inside the `app/Blocks` directory.   
> Check out [the generated files](#generated-files) section to see what those files are and how they're being used.

<a name="manage-uploads"></a>
#### Manage Uploads

If some of your blocks require file uploads (eg. images), please know that you can overwrite their specific configuration from inside the `config/varbox/blocks` config file, specifically from the `upload` section.

> To learn more about model specific upload configuration, please read this [documentation section](/docs/{{version}}/file-uploads#specific-model-configurations).

<a name="custom-entity-blocks"></a>
## Custom Entity Blocks

As you've probably noticed by now, after generating a block and creating a record from the admin panel, you can instantly assign it to a page.
 
But what about adding the block assignment functionality to other custom entities of yours, such as a blog post?
Let's learn how you can achieve this.

<a name="apply-the-trait"></a>
#### Apply The Trait

Your models should use the `Varbox\Traits\HasBlocks` trait and the `Varbox\Options\BlockOptions` class. 
The trait contains an abstract method `getBlockOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App\Models;

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

<a name="add-blade-code"></a>
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

<a name="add-controller-code"></a>
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

Now you can assign blocks for your custom entities and display them in your frontend views.

<a name="display-assigned-blocks"></a>
## Display Assigned Blocks

After you've generated a block, created an entry for it in the admin and assigned it to your entities, it's time to actually display the block's contents in your frontend.

You can display all assigned blocks from a location by using the `renderBlocks()` method present on the `HasBlocks` trait, inside your frontend blade view.

```php
{!! $item->renderBlocks('content') !!}
```

This line of code will output the concatenated parsed html for all blocks assigned to the "content" location. 
That method uses the `app/Blocks/{YourType}/front.blade.php` of each block type as the html source, so make sure you update those files accordingly.

<a name="useful-methods"></a>
## Useful Methods

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

<a name="assign-block"></a>
#### Assign Block

You can assign a block for a model record by using the `assignBlock()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$item = YourModel::find($id);
$item->assignBlock($blockModel, $blockLocation, $blockOrder ?? null);
```

<a name="unassign-block"></a>
#### Un-assign Block

You can unassign a block for a model record by using the `unassignBlock()` method present on the `Varbox\Traits\HasBlocks` trait.

```php
$item = YourModel::find($id);
$item->unassignBlock($blockModel, $blockLocation, $blockPivotId);
```

<a name="configuration"></a>
## Configuration

The blocks configuration file is located at `config/varbox/blocks.php`.

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

The block classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Block</p>

Found in `config/varbox/bindings.php` at `models.block_model` key.   
This class represents the block model.

<p class="overwrite-class">Varbox\Controllers\BlocksController</p>

Found in `config/varbox/bindings.php` at `controllers.blocks_controller` key.   
This class is used for interactions with the "Admin -> Manage Content -> Pages" section.

<p class="overwrite-class">Varbox\Requests\BlockRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.block_form_request` key.   
This class is used for validating any block when creating or updating.

<p class="overwrite-class">Varbox\Filters\BlockFilter</p>

Found in `config/varbox/bindings.php` at `filters.block_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\BlockSort</p>

Found in `config/varbox/bindings.php` at `sorts.block_sort` key.   
This class is used for applying the sorting logic.
