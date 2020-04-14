# Model Revisions

- [Usage](#usage)
    - [Create Revisions Automatically](#create-revisions-automatically)
    - [Create Revisions Manually](#create-revisions-manually)
    - [Rollback To Revision](#rollback-to-revision)
    - [Fetch Revisions](#fetch-revisions)
- [Customizations](#customizations)
    - [Limit Revisions Number](#limit-revisions-number)
    - [Attach Timestamps To Revisions](#attach-timestamps-to-revisions)
    - [Include Fields To Revision](#include-fields-to-revision)
    - [Exclude Fields From Revision](#exclude-fields-from-revision)
    - [Include Relations To Revision](#include-relations-to-revision)
    - [Save Revision On Create](#save-revision-on-create)
    - [Do Not Save Revision On Rollback](#do-not-save-revision-on-rollback)
- [The Model](#the-model)
    - [Get Original Model](#get-original-model)
    - [Get Revision Creator](#ge-revision-creator)
    - [Get User Revisions](#get-user-revisions)
- [Eloquent Events](#eloquent-events)
- [Crud Implementation](#crud-implementation)
    - [Create The Route](#create-the-route)
    - [Apply Controller Trait](#apply-controller-trait)
    - [Add Blade Code](#add-blade-code)
    - [Dig Deeper](#dig-deeper)
- [Overwrite Bindings](#overwrite-bindings)

Create revisions for any Eloquent model record along with its underlying relationships.    
   
* when a revision is created, it gets stored inside the `revisions` database table.    
* revisions are created automatically on model update, using the `updated` eloquent event
* revisions can also can be created manually by using the `saveAsRevision()`   
* when a record is force deleted, its revisions will be removed, using the `deleted` eloquent event   

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\HasRevisions` trait and the `Varbox\Options\RevisionOptions` class.   

The trait contains an abstract method `getRevisionOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\RevisionOptions;
use Varbox\Traits\HasRevisions;

class YourModel extends Model
{
    use HasRevisions;

    /**
     * Get the options for revisioning the model.
     *
     * @return RevisionOptions
     */
    public function getRevisionOptions(): RevisionOptions
    {
        return RevisionOptions::instance();
    }
}
```

<a name="create-revisions-automatically"></a>
#### Create Revisions Automatically

Once you've used the `HasRevisions` trait in your eloquent models, each time you update a model record, a revision containing its original attribute values will be created automatically using the `updated` eloquent event.
The revision record will be stored inside the `revisions` database table. 

```php
$model = YourModel::find($id);
$model->update(...);

// the model has been modified
// a revision containing the model's original data is created
```

<a name="create-revisions-manually"></a>
#### Create Revisions Manually

You can also create a revision manually by using the `saveAsRevision()` method present on the `HasRevisions` trait.

```php
$model = YourModel::find($id);

// a new entry is stored inside the "revisions" database table
// reflecting the current state of that model record
$model->saveAsRevision();
```

<a name="rollback-to-revision"></a>
#### Rollback To Revision

You can rollback the model record to one of its past revisions by using the `rollbackToRevision()` method present on the `HasRevisions` trait

```php
$model = YourModel::find($id);
$revision = $model->revisions()->latest()->first();

$model->rollbackToRevision($revision);
```

<a name="fetch-revisions"></a>
#### Fetch Revisions

You can fetch a model record's revisions by using the `revisions()` morph many relation present on the `Varbox\Traits\HasRevisions` trait.

```php
$model = YourModel::find($id);
$revisions = $model->revisions;
```

<a name="customizations"></a>
## Customizations

The revision functionality offers a variety of customizations to fit your needs.

<a name="limit-revisions-number"></a>
#### Limit Revisions Number

You can limit the number of revisions each model record can have by using the `limitRevisionsTo()` method in your definition of the `getRevisionOptions()` method.   
   
This prevents ending up with thousands of revisions for a heavily updated record.   
   
When the limit is reached, after creating the new (latest) revision, the script automatically removes the oldest revision that model record has.

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->limitRevisionsTo(30);
}
```

<a name="attach-timestamps-to-revisions"></a>
#### Attach Timestamps To Revisions

By default, when creating a revision, the model's timestamps are excluded from the revision data.   
   
If you'd like to store the model's timestamps when creating a revision, please use the `withTimestamps()` method in your definition of the `getRevisionOptions()` method.

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->withTimestamps();
}
```

<a name="include-fields-to-revision"></a>
#### Include Fields To Revision

You can specify which fields to store when creating a new revision, by using the `fieldsToRevision()` method in your definition of the `getRevisionOptions()` method.   
   
Please note that the excluded attributes won't be stored when creating the revision, but when rolling back, the excluded attributes will become `null / empty` for the actual model record.

```php
/**
 * Get the options for revisioning the model.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->fieldsToRevision('title', 'content');
}
```

<a name="exclude-fields-from-revision"></a>
#### Exclude Fields From Revision

You can exclude certain model attributes when creating a revision by using the `fieldsNotToRevision()` method in your definition of the `getRevisionOptions()` method.   
   
> The `fieldsToRevision()` takes precedence over the `fieldsNotToRevision()`   
> Don't use both these methods in the same definition of the `getRevisionOptions()` method
  
Please note that the excluded attributes won't be stored when creating the revision, but when rolling back, the excluded attributes will become `null / empty` for the actual model record.

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->fieldsNotToRevision('title', 'content');
}
```

<a name="include-relations-to-revision"></a>
#### Include Relations To Revision

More often than not you will want to create a full copy in time of the model record and this includes revisioning its relations too (especially child relations).   
   
You can specify which relations to revision alongside the model record by using the `relationsToRevision()` method in your definition of the `getRevisionOptions()` method.   
   
> When rolling back the model record to a past revision, the specified relations will also be rolled back to their state when that revision happened. This includes:   
> - re-creating a relation record from ground up if it was force deleted   
> - deleting any future added related records up until the revision checkpoint.

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->relationsToRevision('posts', 'payments');
}
```

<a name="save-revision-on-create"></a>
#### Save Revision On Create

By default, when creating a new model record, a revision will not be created, because the record is fresh and is at its first state. 

If you wish to create a revision when creating the model record, you can do so by using the `enableRevisionOnCreate()` method in your definition of the `getRevisionOptions()` method.

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->enableRevisionOnCreate();
}
```

<a name="do-not-save-revision-on-rollback"></a>
#### Do Not Save Revision On Rollback

By default, when rolling back to a past revision, a new revision is automatically created. This new revision contains the model record's state before the rollback happened.   
   
You can disable this behavior by using the `disableRevisioningWhenRollingBack()` method in your definition of the `getRevisionOptions()` method.   

```php
/**
 * Set the options for the HasRevisions trait.
 *
 * @return RevisionOptions
 */
public function getRevisionOptions(): RevisionOptions
{
    return RevisionOptions::instance()
        ->disableRevisioningWhenRollingBack();
}
```

<a name="the-model"></a>
## The Model

Up until this point you've learned how to use the functionality exposed by the `HasRevisions` trait.

However, there's also a `Varbox\Models\Revision` model that manages the revisions.

Below you'll find some basic functionalities of the `Varbox\Models\Revision` model, but you can always inspect the model yourself, or even [extend](#overwrite-bindings) it.

<a name="get-original-model"></a>
#### Get Original Model

The `Revision` model is polymorphically related to the models, using a `belongs to` relation called `revisionable`.

```php
$revision = Revision::find($id);
$model = $revision->revisionable;
```

<a name="get-revision-creator"></a>
#### Get Revision Creator

The `Revision` model defines a `one to many` relation to the `App\User` model called `user`.

```php
$revision = Revision::find($id);
$user = $revision->user;
```

<a name="get-user-revisions"></a>
#### Get User Revisions

The `Revision` model exposes a `query scope` to help you filter revision records by their `user_id`.

```php
// by passing the loaded user model
$revisions = Revision::ofUser($user)->get();

// or by passing the user id
$revisions = Revision::ofUser($user->id)->get();
```

<a name="eloquent-events"></a>
## Eloquent Events

The revision functionality comes packed with two eloquent events: `revisioning` and `revisioned`   
   
You can implement these events in your eloquent models as you would implement any other eloquent events that come with the Laravel framework.

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\RevisionOptions;
use Varbox\Traits\HasRevisions;

class YourModel extends Model
{
    use HasRevisions;

    /**
     * Boot the model.
     *
     * @return RevisionOptions
     */
    public static function boot()
    {
        parent::boot();

        static::revisioning(function ($model) {
            // your logic here
        });

        static::revisioned(function ($model) {
            // your logic here
        });
    }
    
    /**
     * Set the options for the HasRevisions trait.
     *
     * @return RevisionOptions
     */
    public function getRevisionOptions(): RevisionOptions
    {
        return RevisionOptions::instance();
    }
}
```

<a name="crud-implementation"></a>
## Crud Implementation

As you've probably noticed browsing the admin panel, you can implement revisions directly into your CRUDs.
You can find a good example of this kind of implementation inside:    
- `Admin -> Manage Content -> Pages (edit)`
- `Admin -> Manage Content -> Blocks (edit)`
- `Admin -> Manage Content -> Emails (edit)`

> To learn how to create a CRUD step by step (**with revisions included**), please follow the [Admin Crud Example](/docs/{{version}}/full-example) guide.

To apply the revision functionality in your own CRUDs there's a few steps you'll need to follow.   
For this example we'll assume you have a `App\Http\Controllers\Admin\PostsController` on which you want to apply revisions.

<a name="create-the-route"></a>
#### Create The Route

The first thing we need to do is create the `revision route` responsible for showing a revision of a post model record.

Inside your `routes/web.php` file, append the following route to your crud routes. In the below example we're assuming you're using a `route group` to group your post routes.

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
], function () {
    // your other post crud routes

    Route::get('revision/{revision}', [
        'as' => 'admin.posts.revision', 
        'uses' => 'PostsController@showRevision', 
        'permissions' => 'posts-edit' // optional to restrict access
    ]);
});
```

<a name="apply-controller-trait"></a>
#### Apply Controller Trait

In your `controller` use the `Varbox\Traits\CanRevision` trait.   
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanRevision;

class PostsController extends Controller
{
    use CanRevision;
    ...

    /**
     * Get the title to be used on the revision view.
     * The title will be used in: page title / meta title
     *
     * @return string
     */
    protected function revisionPageTitle(): string
    {
        return 'Post Revision';
    }

    /**
     * Get the blade view to be rendered as the revision view.
     *
     * @return string
     */
    protected function revisionView(): string
    {
        return 'views.posts.edit';
    }

    /**
     * Get additional view variables to be assigned to the revision view.
     * If no additional variables are needed, return an empty array.
     *
     * @param Model $revisionable
     * @return array
     */
    protected function revisionViewVariables(Model $revisionable): array
    {
        return [
            'data' => $revisionable->toArray(),
        ];
    }
}
```

<a name="add-blade-code"></a>
#### Add Blade Code

In your `edit` blade view file add the code for displaying the `revisions`

```
@if($item->exists)
    @include('varbox::helpers.revision.container', [
        'model' => $item, 
        'route' => 'admin.posts.revision', 
        'revision' => $revision, 
        'parameters' => []
    ])
@endif
```

Additionally you shouldn't make your buttons available when viewing a revision.
`save / save & stay / save as draft / duplicate / preview / cancel`

```
@if(!isset($revision))
    // your buttons here
@endif
```

<a name="dig-deeper"></a>
#### Dig Deeper

If you still have difficulties in implementing revisions on your own, you can look at how `VarBox Pages` are done. The files you need to inspect are:
- `Varbox\Controllers\PagesController`
- `resources/views/vendor/varbox/admin/pages/_form.blade.php`

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
`VarBox` makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

The `revision` classes available for binding overwrites are:

#### Varbox\Models\Revision

Found in `config/varbox/bindings.php` at `models.revision_model` key.   
This class represents the revision model.

#### Varbox\Controllers\RevisionsController

Found in `config/varbox/bindings.php` at `controllers.revision_controller` key.   
This class is used for interactions with revisions inside a crud implementation.
- listing revisions
- viewing revisions
- rolling back revisions
- removing revisions
