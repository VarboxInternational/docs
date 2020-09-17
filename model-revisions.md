<h1>Model Revisions</h1>

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
- [Overwrite Bindings](#overwrite-bindings)
- [Implementation Example](#implementation-example)

<p id="first-p">
Create revisions for any Eloquent model record along with its underlying relationships.
</p>    
   
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

namespace App\Models;

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

You can also create a revision manually by using the `saveAsRevision()` method present on the trait.

```php
$model = YourModel::find($id);

// a new entry is stored inside the "revisions" database table
// reflecting the current state of that model record
$model->saveAsRevision();
```

<a name="rollback-to-revision"></a>
#### Rollback To Revision

You can rollback the model record to one of its past revisions by using the `rollbackToRevision()` method present on the trait.

```php
$model = YourModel::find($id);
$revision = $model->revisions()->latest()->first();

$model->rollbackToRevision($revision);
```

<a name="fetch-revisions"></a>
#### Fetch Revisions

You can fetch a model record's revisions by using the `revisions()` morph many relation present on the trait.

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
   
When the limit is reached, after creating the new (latest) revision, the script automatically removes the oldest revision that model record has. 
This prevents ending up with thousands of revisions for a heavily updated record.

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

If you'd like to store the model's timestamps when creating a revision, use the `withTimestamps()` method in your definition of the `getRevisionOptions()` method.

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

You can specify which fields to store when creating a new revision using the `fieldsToRevision()` method in your definition of the `getRevisionOptions()` method.   
   
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

You can exclude certain model attributes when creating a revision using the `fieldsNotToRevision()` method in your definition of the `getRevisionOptions()` method.   
   
> The `fieldsToRevision()` takes precedence over the `fieldsNotToRevision()`   
> Don't use both methods in the same definition of the `getRevisionOptions()` method
  
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
   
> When rolling back the model record to a past revision, the specified relations will also be rolled back to their state when that revision happened. This includes:   
> - re-creating a relation record from ground up if it was force deleted   
> - deleting any future added related records up until the revision checkpoint.
   
You can specify which relations to revision alongside the model record by using the `relationsToRevision()` method in your definition of the `getRevisionOptions()` method.   

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

By default, when creating a new model record, a revision will not be created because the record is fresh and it's in its initial state. 

You can create a revision when creating the model record using the `enableRevisionOnCreate()` method in your definition of the `getRevisionOptions()` method.

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

You can get a revision's original model by using the `revisionable` relation method.

```php
use \Varbox\Models\Revision;

$revision = Revision::find($id);
$model = $revision->revisionable;
```

<a name="get-revision-creator"></a>
#### Get Revision Creator

You can get a revision's user by using the `user` relation method.

```php
use \Varbox\Models\Revision;

$revision = Revision::find($id);
$user = $revision->user;
```

<a name="get-user-revisions"></a>
#### Get User Revisions

You can filter revisions by a user using the `ofUser` query scope present on the model.

```php
use \Varbox\Models\Revision;

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

namespace App\Models;

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

<p class="overwrite-class">Varbox\Models\Revision</p>

Found in `config/varbox/bindings.php` at `models.revision_model` key.   
This class represents the revision model.

<p class="overwrite-class">Varbox\Controllers\RevisionsController</p>

Found in `config/varbox/bindings.php` at `controllers.revision_controller` key.   
This class is used for interactions with revisions inside an admin implementation.

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#model-revisions) page.
