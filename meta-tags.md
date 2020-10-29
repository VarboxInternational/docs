<h1>Meta Tags</h1>

- [General Usage](#general-usage)
    - [Set Meta Tag Value](#set-meta-tag-value)
    - [Get Meta Tag Value](#get-meta-tag-value)
    - [Display Meta Tags](#display-meta-tags)
- [Model Usage](#model-usage)
    - [Apply The Trait](#apply-the-trait)
    - [Create Database Column](#create-database-column)
    - [Add Admin Form](#add-admin-form)
    - [Display Model Meta Tags](#display-model-meta-tags)
- [Configuration](#configuration)
- [Overwrite Bindngs](#overwrite-bindings)
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to quickly set values for any meta tags you might have and properly display them in the <code class="class="language-php">head</code> section of your pages.
</p>

<a name="general-usage"></a>
## General Usage

To quickly set, get or display your meta tags, you should use the `Varbox\Helpers\MetaHelper` class.   
Please note that a `meta()` helper method is also available for working with the `MetaHelper` class.

<a name="set-meta-tag-value"></a>
#### Set Meta Tag Value

To set the values for your meta tags use the `set()` method present on the `MetaHelper` class.

```php
meta()->set('title', 'Your meta title');
meta()->set('description', 'Your meta description');
```

<a name="get-meta-tag-value"></a>
#### Get Meta Tag Value

To get the values of your meta tags, after you've set them, use the `get()` method present on the `MetaHelper` class.

```php
$metaTitle = meta()->get('title');
$metaDescription = meta()->get('description');
```

Please note that if you're trying to get the value for an empty meta tag, the default value will be returned, if any. 
You can set default meta tag values from inside the `config/varbox/meta.php`, specifically the `default_values` key.

<a name="display-meta-tags"></a>
#### Display Meta Tags

To actually display the html for your meta tags, use the `tag()` or `tags()` method present on the `MetaHelper` class, in your `<head>` section.

```php
<head>
    {!! meta()->tags('title', 'description') !}}
</head>
```

Please note that only meta tags marked as available will be displayed. 
You can customize the available tags from inside the `varbox/config/meta.php`, specifically the `available_tags` key.

<a name="model-usage"></a>
## Model Usage

You can also assign meta tags to your model records. Varbox provides you with an easy way of saving any number of meta tags directly on your model records, store those values in the database and display their proper html in your blade views.

<a name="apply-the-trait"></a>
#### Apply The Trait

Your models should use the `Varbox\Traits\HasMetaTags` trait and the `Varbox\Options\MetaTagOptions` class. The trait contains an abstract method `getMetaTagOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\MetaTagOptions;
use Varbox\Traits\HasMetaTags;

class YourModel extends Model
{
    use HasMetaTags;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        ...
        'meta',
    ];

    /**
     * The attributes that should be casted to native types.
     *
     * @var array
     */
    protected $casts = [
        ...
        'meta' => 'array',
    ];

    /**
     * Set the options for the HasMetaTags trait.
     *
     * @return MetaTagOptions
     */
    public function getMetaTagOptions(): MetaTagOptions
    {
        return MetaTagOptions::instance();
    }
}
```

<a name="create-database-column"></a>
#### Create Database Column

By default the trait uses the `meta` column (json) to store any meta tags, so you should create a `migration` to add this column.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->json('meta')->nullable();
});
```

After you've setup your migration, run the `php artisan migrate` command.

<a name="add-admin-form"></a>
#### Add Admin Form

Next you should add the fields for saving any meta tags that you might want. 
Fortunately, the Varbox platform offers a nice partial to help you with that.

> You you're not happy with the default meta tags, you can customize those fields by modifying the `resources/views/vendor/varbox/helpers/meta/container.blade.php` file.

All you have to do is just include the partial in your `form` blade file and your admin crud will instantly support saving values for meta tags.

```php
@include('varbox::helpers.meta.container', ['model' => $item ?? null])
```

<a name="display-model-meta-tags"></a>
#### Display Model Meta Tags

Up until this point, you've learned how you can assign save meta tag values for your custom entities inside the admin panel.

To actually display the html of the meta tags saved for a model record, use the `displayMetaTags()` method present in the `HasMetaTags` trait.

```php
<head>
    {!! $model->displayMetaTags() !!}
</head>
```

Please note that only the meta tags with an actual value will be rendered.

<a name="configuration"></a>
## Configuration

The meta tags configuration file is located at `config/varbox/meta.php`.

For more information on how you can customize the meta tags functionality, please read the comments from that file.

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

<p class="overwrite-class">Varbox\Helpers\MetaHelper</p>

Found in `config/varbox/bindings.php` at `helpers.meta_helper` key.   
This class is used for setting, getting and displaying meta tags.

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#meta-tags) page.
