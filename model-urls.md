# Model Urls

- [Usage](#usage)
    - [Generate Url](#generate-url)
    - [Fetch The Url](#fetch-the-url)
    - [Disable Url Generation](#disable-url-generation)
- [Customizations](#customizations)
    - [Define Route Dispatch](#define-route-dispatch)
    - [Change Separator](#change-separator)
    - [Set Url Prefix](#set-url-prefix)
    - [Set Url Suffix](#set-url-suffix)
    - [Disable Cascade Update](#disable-cascade-update)
- [The Model](#the-model)
    - [Get Original Model](#get-original-model)
- [Overwrite Bindings](#overwrite-bindings)

This functionality allows you to generate custom urls for your model records.   
The generated url will be stored inside the `urls` database table.   

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\HasUrl` trait and the `Varbox\Options\UrlOptions` class. 
The trait contains an abstract method `getUrlOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\UrlOptions;
use Varbox\Traits\HasUrl;

class YourModel extends Model
{
    use HasUrl;

    /**
     * The` attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name',
        'slug',
    ];

    /**
     * Get the options for the HasUrl trait.
     *
     * @return UrlOptions
     */
    public function getUrlOptions(): UrlOptions
    {
        return UrlOptions::instance()
            ->routeUrlTo(YourController::class, 'yourShowMethod')
            ->generateUrlSlugFrom('name')
            ->saveUrlSlugTo('slug');
    }
}
```

The `routeUrlTo()` method is mandatory and it's used to specify to what controller and method to dispatch the request, when accessing the generated url in your browser.

The `generateUrlSlugFrom()` and `saveUrlSlugTo()` are mandatory and they're used by the underlying `HasSlug` trait which is used by the `HasUrl` trait. 
For more information on how slugs work please see [Model Slugs](/docs/{{version}}/model-slugs).

> When using the `HasUrl` trait on your models you're not required to also use the `HasSlug` trait.
> The `HasUrl` trait directly uses the `HasSlug` trait.

<hr>

Next you should create the table column that you've specified for the `saveUrlSlugTo()` method.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->string('slug')->nullable()->unique();
});
```

<a name="generate-url"></a>
#### Generate Url

After you've done the above steps, generating a url for a model record will be done automatically when you'll be saving that model record.

A new entry for the url will be stored inside the `urls` database table.

```php
$model = YourModel::create([
    'name' => 'Initial name'
]); // url is "initial-name"

$model->update([
    'name' => 'Modified name'
]); // url is "modified-name"
``` 

<a name="fetch-the-url"></a>
#### Fetch The Url

By using the `Varbox\Traits\HasUrl` trait on your models, you automatically relate that model to the `Varbox\Models\Url` model via a `morphOne` relationship.

To get the full url of a model use the `getUrl()` method.

```php
$model = YourModel::find($id);

$model->getUrl();
``` 

To get only the relative url of a model use the `getUri()` method.

```php
$model = YourModel::find($id);
 
$model->getUri();
``` 

<a name="disable-url-generation"></a>
#### Disable Url Generation

By default, a url will be generated for your model record each time you create or update one.   
You may have the need to just save the model without generating a url.

You can do that by using the `doGenerateUrl()` method. 

```php
// url will not be generated
$model = YourModel::doNotGenerateUrl()->create([
    ...
]);

// url will not be generated here as well
$model->doNotGenerateUrl()->update([
    ...
]);
```

If you're doing multiple operations on the same model in the same request and at some point you want to generate a url for it, you can enable it back up by using the `doGenerateUrl()` method.

```php
// url will not be generated
$model = YourModel::doNotGenerateUrl()->create([
    ...
]);

// url will be generated
$model->doGenerateUrl()->update([
    ...
]);
``` 

<a name="customizations"></a>
## Customizations

The url functionality offers a variety of customizations to fit your needs.

<a name="define-route-dispatch"></a>
#### Define Route Dispatch

You can specify to what controller and method to dispatch the request, when accessing the generated url in your browser, by using the `routeUrlTo()` method in your definition of the `getUrlOptions()` method.

```php
/**
 * Get the options for the HasUrl trait.
 *
 * @return UrlOptions
 */
public function getUrlOptions(): UrlOptions
{
    return UrlOptions::instance()
        ->routeUrlTo(YourController::class, 'yourShowMethod')
        ->generateUrlSlugFrom('name')
        ->saveUrlSlugTo('slug');
}
```

<a name="change-separator"></a>
#### Change Separator

By default the url is generated by concatenating the prefix, attribute and suffix using a slash `/`

You can change the url separator by using the `glueUrlWith()` method in your definition of the `getUrlOptions()` method.

```php
/**
 * Get the options for the HasUrl trait.
 *
 * @return UrlOptions
 */
public function getUrlOptions(): UrlOptions
{
    return UrlOptions::instance()
        ->routeUrlTo(YourController::class, 'yourShowMethod')
        ->generateUrlSlugFrom('name')
        ->saveUrlSlugTo('slug')
        ->glueUrlWith('-');
}
```

<a name="set-url-prefix"></a>
#### Set Url Prefix

You can prefix the generated url with a value by using the `prefixUrlWith()` method in your definition of the `getUrlOptions()` method.

```php
/**
 * Get the options for the HasUrl trait.
 *
 * @return UrlOptions
 */
public function getUrlOptions(): UrlOptions
{
    return UrlOptions::instance()
        ->routeUrlTo(YourController::class, 'yourShowMethod')
        ->generateUrlSlugFrom('name')
        ->saveUrlSlugTo('slug')
        ->prefixUrlWith(function ($prefix, $model) {
            foreach ($model->ancestors()->get() as $ancestor) {
                $prefix[] = $ancestor->slug;
            }

            return implode('/' , (array)$prefix);
        });
}
```

<a name="set-url-suffix"></a>
#### Set Url Suffix

You can suffix the generated url with a value by using the `suffixUrlWith()` method in your definition of the `getUrlOptions()` method.

```php
/**
 * Get the options for the HasUrl trait.
 *
 * @return UrlOptions
 */
public function getUrlOptions(): UrlOptions
{
    return UrlOptions::instance()
        ->routeUrlTo(YourController::class, 'yourShowMethod')
        ->generateUrlSlugFrom('name')
        ->saveUrlSlugTo('slug')
        ->suffixUrlWith(function ($suffix, $model) {
            foreach ($model->descendants()->get() as $ancestor) {
                $suffix[] = $ancestor->slug;
            }

            return implode('/' , (array)$suffix);
        });
}
```

<a name="disable-cascade-update"></a>
#### Disable Cascade Update

By default, when updating a model record's url that has children, it's underlying children's urls will also be updated to reflect the change.

You can disable this using the `doNotUpdateCascading()` method in your definition of the `getUrlOptions()` method.

```php
/**
 * Get the options for the HasUrl trait.
 *
 * @return UrlOptions
 */
public function getUrlOptions(): UrlOptions
{
    return UrlOptions::instance()
        ->routeUrlTo(YourController::class, 'yourShowMethod')
        ->generateUrlSlugFrom('name')
        ->saveUrlSlugTo('slug')
        ->doNotUpdateCascading();
}
```

<a name="the-model"></a>
## The Model

Up until this point you've learned how to use the functionality exposed by the `HasUrl` trait.

However, there's also a `Varbox\Models\Url` model that manages the urls.

Below you'll find some basic functionalities of the `Varbox\Models\Url` model, but you can always inspect the model yourself, or even [extend](#overwrite-bindings) it.

<a name="get-original-model"></a>
#### Get Original Model

The `Url` model is related to the models, using a `morphTo` relation called `urlable`.

```php
use Varbox\Models\Url;

$url = Url::find($id);
$model = $url->urlable;
```

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
`VarBox` makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

The `url` classes available for binding overwrites are:

#### Varbox\Models\Url

Found in `config/varbox/bindings.php` at `models.url_model` key.   
This class represents the url model.
