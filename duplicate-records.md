# Duplicate Records

- [Usage](#usage)
- [Customizations](#customizations)
    - [Exclude Columns](#exclude-columns)
    - [Unique Columns](#unique-columns)
    - [Exclude Relations](#exclude-relations)
    - [Exclude Relation Columns](#exclude-relation-columns)
    - [Unique Relation Columns](#unique-relation-columns)
    - [Disable Deep Duplication](#disable-deep-duplication)
- [Eloquent Events](#eloquent-events)
- [Crud Implementation](#crud-implementation)
    - [Create The Routes](#create-the-routes)
    - [Apply Controller Trait](#apply-controller-trait)
    - [Add Blade Code](#add-blade-code)
    - [Create Permissions](#create-permissions)
    - [Dig Deeper](#dig-deeper)

This functionality allows you to duplicate any eloquent model record along with its underlying relationships.    
   
The relationship types that can and will make sense to be duplicated are:   
`hasOne`, `morphOne`, `hasMany`, `morphMany`, `belongsToMany`, `morphToMany` 

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\HasDuplicates` trait and the `Varbox\Options\DuplicateOptions` class.   

The trait contains an abstract method `getDuplicateOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\DuplicateOptions;
use Varbox\Traits\HasDuplicates;

class YourModel extends Model
{
    use HasDuplicates;
    
    /**
     * Set the options for the HasDuplicates trait.
     *
     * @return DuplicateOptions
     */
    public function getDuplicateOptions(): DuplicateOptions
    {
        return DuplicateOptions::instance();
    }
}
```

Once you've used the `HasDuplicates` trait in your model, you can duplicate model records by using the `saveAsDuplicate()` method.

```php
$model = YourModel::find($id);

$duplicatedModel = $model->saveAsDuplicate();
```

<a name="customizations"></a>
## Customizations

The duplicate functionality offers a variety of customizations to fit your needs.

<a name="exclude-columns"></a>
#### Exclude Columns

When duplicating a model, you can exclude certain columns from being duplicated by using the `excludeColumns()` method in your definition of the `getDuplicateOptions()` method.

```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->excludeColumns('column_one', 'column_two');
}
```

<a name="unique-columns"></a>
#### Unique Columns

When duplicating a model, you can save certain columns in a unique format by using the `uniqueColumns()` method in your definition of the `getDuplicateOptions()` method.   

```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->uniqueColumns('column_one', 'column_two');
}
```

<a name="exclude-relations"></a>
#### Exclude Relations

By default, when duplicating a model, all of its child relations are also duplicated along with it.   
   
You can exclude certain relations from being duplicated by using the `excludeRelations()` method in your definition of the `getDuplicateOptions()` method.   
   
The relations specified in the `excludeRelations()` method will not be duplicated along with the targeted model, meaning that the newly duplicated model will not have any records associated to it for the specified relations.

```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->excludeRelations('relationOne', 'relationTwo');
}
```

<a name="exclude-relation-columns"></a>
#### Exclude Relation Columns

When duplicating a model, you can exclude certain columns of its child relations from being duplicated by using the `excludeRelationColumns()` method in your definition of the `getDuplicateOptions()` method.   
   
*This method accepts only one parameter which should be an associative array containing:*   
`key`: *a string representing the name of the relation*      
`value`: *an array containing the columns to exclude for that relation*
   
```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->excludeRelationColumns([
            'relationOne' => ['column_one', 'column_two'],
            'relationTwo' => ['column_one'],
        ]);
}
```

<a name="unique-relation-columns"></a>
#### Unique Relation Columns

When duplicating a model, you can save certain columns of its child relations in a unique format by using the `uniqueRelationColumns()` method in your definition of the `getDuplicateOptions()` method.   
   
*This method accepts only one parameter which should be an associative array containing:*   
`key`: *a string representing the name of the relation*      
`value`: *an array containing the columns to exclude for that relation*

```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->uniqueRelationColumns([
            'relationOne' => ['column_one', 'column_two'],
            'relationTwo' => ['column_one'],
        ]);
}
```

<a name="disable-deep-duplication"></a>
#### Disable Deep Duplication

If you only want to duplicate your targeted model without duplicating any relations whatsoever, you can specify this by using the `disableDeepDuplication()` method in your definition of the `getDuplicateOptions()` method.   

```php
/**
 * Set the options for the HasDuplicates trait.
 *
 * @return DuplicateOptions
 */
public function getDuplicateOptions() : DuplicateOptions
{
    return DuplicateOptions::instance()
        ->disableDeepDuplication();
}
```

<a name="eloquent-events"></a>
## Eloquent Events

The duplicate functionality comes packed with two eloquent events: `duplicating` and `duplicated`
   
You can implement these events in your own models as you would implement any other eloquent events that come with the Laravel framework.

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Neurony\Duplicate\Options\DuplicateOptions;
use Neurony\Duplicate\Traits\HasDuplicates;

class YourModel extends Model
{
    use HasDuplicates;

    /**
     * Boot the model.
     *
     * @return DuplicateOptions
     */
    public static function boot()
    {
        parent::boot();

        static::duplicating(function ($model) {
            // your logic here
        });

        static::duplicated(function ($model) {
            // your logic here
        });
    }
    
    /**
     * Set the options for the HasDuplicates trait.
     *
     * @return DuplicateOptions
     */
    public function getDuplicateOptions(): DuplicateOptions
    {
        return DuplicateOptions::instance();
    }
}
```

<a name="crud-implementation"></a>
## Crud Implementation

As you've probably noticed when browsing the admin panel, you can implement duplication directly into your CRUDs.
You can find a good example of this kind of implementation inside:    
- `Admin -> Manage Content -> Pages (edit)`
- `Admin -> Manage Content -> Blocks (edit)`
- `Admin -> Manage Content -> Emails (edit)`

> To learn how to create a CRUD step by step (**with duplication included**), please follow the [Admin Crud Example](/docs/{{version}}/full-example) guide.

To apply the duplicate functionality in your own CRUDs there's a few steps you'll need to follow.   
For this example we'll assume you have a `App\Http\Controllers\Admin\PostsController` on which you want to apply duplication.

<a name="create-the-routes"></a>
#### Create The Routes

The first thing we need to do is create the `duplicate route` responsible for duplicating a model record.

Inside your `routes/web.php` file, append the following routes to your crud routes. In the below example we're assuming you're using a `route group` to group your post routes.

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
], function () {
    // your other post crud routes
    
    Route::post('duplicate/{post}', [
        'as' => 'admin.posts.duplicate', 
        'uses' => 'PostsController@duplicate', 
        'permissions' => 'posts-duplicate' // optional to restrict access
    ]);
});
```

<a name="apply-controller-trait"></a>
#### Apply Controller Trait

In your `controller` use the `Varbox\Traits\CanDuplicate` trait.   
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanDuplicate;

class PostsController extends Controller
{
    use CanDuplicate;
    ...

    /**
     * Get the model to be duplicated.
     *
     * @return Model
     */
    protected function duplicateModel(): string
    {
        return Post::class;
    }

    /**
     * Get the url to redirect to after the duplication.
     *
     * @param Model $duplicate
     * @return string
     */
    protected function duplicateRedirectTo(Model $duplicate): string
    {
        return route('admin.posts.edit', $duplicate->getKey());
    }
}
```

<a name="add-blade-code"></a>
#### Add Blade Code

In your `edit` blade view file add the code for displaying the `duplicate` button.

```
@if($item->exists)
    // use this check only if you're also implementing revisions in your crud
    @if(!isset($revision))
        @permission('posts-duplicate')
            @include('varbox::buttons.duplicate', [
                'url' => route('admin.posts.duplicate', $item->getKey())
            ])
        @endpermission
    @endif
@endif
```

<a name="create-permissions"></a>
#### Create Permissions

You have the possibility to restrict the duplicate action by setting a specific permission.   
The [VarBox](/) platform is using a `role based permission` system out of the box.

> This step is optional and you should follow it only if you've added permissions on your duplicate route, or in your blade file.

You should add a permission for allowing the admin to duplicate a model record.

> As a good practice, it's recommended you create your own `PermissionsSeeder` class to handle this.
> For an in-depth example of such a seeder, please have a look at `Varbox\Seed\PermissionsSeeder`

```php
app(PermissionModelContract::class)->create([
    'group' => 'Posts',
    'label' => 'Duplicate',
    'guard' => 'admin',
    'name' => 'posts-duplicate',
]);
```

<a name="dig-deeper"></a>
#### Dig Deeper

If you still have difficulties in implementing model duplication on your own, you can look at how `VarBox Pages` are done. The files you need to inspect are:
- `Varbox\Controllers\PagesController`
- `resources/views/vendor/varbox/admin/pages/_form.blade.php`
