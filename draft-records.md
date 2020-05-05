# Draft Records

- [Usage](#usage)
    - [Save Record As Draft](#save-record-as-draft)
    - [Publish Drafted Record](#publish-drafted-record)
    - [Check If It's Draft](#check-if-draft)
    - [Modify Draft Column](#modify-draft-column)
- [Query Scopes](#query-scope)
    - [Include Drafted Models](#include-drafted-models)
    - [Exclude Drafted Models](#exclude-drafted-models)
    - [Retrieve Only Drafted Models](#retrieve-only-drafted-models)
- [Eloquent Events](#eloquent-events)
- [Crud Implementation](#crud-implementation)
    - [Create The Routes](#create-the-routes)
    - [Apply Controller Trait](#apply-controller-trait)
    - [Add Blade Code](#add-blade-code)
    - [Create Permissions](#create-permissions)
    - [Dig Deeper](#dig-deeper)

This functionality allows you to save eloquent model records in a drafted state.   
A record in a drafted state is much like an `inactive` or a `soft deleted` record.   

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsDraftable` trait.   

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsDraftable;

class YourModel extends Model
{
    use IsDraftable;

    ...
}
```

You should also add the `drafted_at` column to your database table.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->timestamp('drafted_at')->nullable();
});
```

Additionally, it's recommended you cast the `drafted_at` column to a `date` type on your model.

```php
/**
 * The attributes that should be mutated to dates.
 *
 * @var array
 */
protected $dates = [
    'drafted_at',
];
```

<a name="save-record-as-draft"></a>
#### Save Record As Draft

Once you've used the `IsDraftable` trait in your eloquent models and created the `draftd_at` column, you can save model records as drafts by using the `saveAsDraft()` method present on the `IsDraftable` trait.

This means that besides the fact that the attributes you pass will be saved on that model record, the `drafted_at` column will also be populated with the current `datetime`.

```php
// create new record as draft
$draft = app(YourModel::class)->saveAsDraft([
    'name' => 'Your Name',
    'content' => 'Your Content'
]);

// update existing record to a draft
$draft = YourModel::find($id)->saveAsDraft([
   'name' => 'Your Name',
   'content' => 'Your Content'
]);
```

<a name="publish-drafted-record"></a>
#### Publish Drafted Record

You can publish a model record which was saved as a draft by using the `publishDraft()` method present on the `IsDraftable` trait.

When you publish a draft, all it means is that the `drafted_at` column for that model record, will become `null`.

```php
$model = YourModel::find($id);

$model->publishDraft();
```

<a name="check-if-draft"></a>
#### Check If It's Draft

You can check if a model record is drafted or not, by using the `isDrafted()` method present on the `IsDraftable` trait.

```php
$model = YourModel::find($id);

dump($model->isDrafted()); // true or false
```

<a name="modify-draft-column"></a>
#### Modify Draft Column

If you'd like, you can modify the name of `drafted_at` column the `IsDraftable` ttrait is using behind the scenes. 
To do so, you'll have to overwrite the `getDraftedAtColumn()` method in your models.

```php
/**
 * Get the name of the draft column.
 *
 * @return string
 */
public function getDraftedAtColumn()
{
    return 'drafted_at';
}
```

<a name="query-scope"></a>
## Query Scopes

When querying a model that is draftable, the drafted model records will be `automatically excluded` from all query results. This is because the `IsDraftable` trait applies a global query scope on your models.

<a name="include-drafted-models"></a>
#### Include Drafted Models

As noted above, drafted model records will automatically be excluded from query results. 
However, you may force drafted model records to appear in a result set using the `withDrafts()` method on the query.

```php
$allRecords = YourModel::withDrafts()->get();
```

<a name="exclude-drafted-models"></a>
#### Exclude Drafted Models

Even though drafted model records are automatically excluded, you may stumble upon a scenario where you're discarding global scopes on a model by using the `withoutGlobalScopes()` method, but you still don't want to fetch your drafted model records.

You can exclude your drafted model records from your query results by using the `withoutDrafts()` method.

```php
$onlyPublishedRecords = YourModel::withoutDrafts()->get();
```

<a name="retrieve-only-drafted-models"></a>
#### Retrieve Only Drafted Models

If you want to only retrieve the drafted model records, you can do so by applying the `onlyDrafts()` query scope.

```php
$onlyDraftedRecords = YourModel::onlyDrafts()->get();
```

<a name="eloquent-events"></a>
## Eloquent Events

The draft functionality comes packed with two eloquent events: `drafting` and `drafted`   
   
You can implement these events in your eloquent models as you would implement any other eloquent events that come with the Laravel framework.

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsDraftable;

class YourModel extends Model
{
    use IsDraftable;

    /**
     * Boot the model.
     *
     * @return void
     */
    public static function boot()
    {
        parent::boot();

        static::drafting(function ($model) {
            // your logic here
        });

        static::drafted(function ($model) {
            // your logic here
        });
    }
}
```

<a name="crud-implementation"></a>
## Crud Implementation

As you've probably noticed browsing the admin panel, you can implement drafts directly into your CRUDs.
You can find a good example of this kind of implementation inside:    
- `Admin -> Manage Content -> Pages (edit)`
- `Admin -> Manage Content -> Blocks (edit)`
- `Admin -> Manage Content -> Emails (edit)`

> To learn how to create a CRUD step by step (**with drafts included**), please follow the [Admin Crud Example](/docs/{{version}}/full-example) guide.

To apply the draft functionality in your own CRUDs there's a few steps you'll need to follow.   
For this example we'll assume you have a `App\Http\Controllers\Admin\PostsController` on which you want to apply drafts.

<a name="create-the-routes"></a>
#### Create The Routes

The first thing we need to do is create the `draft routes` responsible for saving and publishing a draft.

Inside your `routes/web.php` file, append the following routes to your crud routes. In the below example we're assuming you're using a `route group` to group your post routes.

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
], function () {
    // your other post crud routes

    Route::post('draft/{post?}', [
        'as' => 'admin.posts.draft', 
        'uses' => 'PostsController@saveDraft', 
        'permissions' => 'pages-draft' // optional to restrict access
    ]);
    
    Route::put('publish/{post}', [
        'as' => 'admin.posts.publish', 
        'uses' => 'PostsControllr@publishDraft', 
        'permissions' => 'pages-publish' // optional to restrict access
    ]);
});
```

<a name="apply-controller-trait"></a>
#### Apply Controller Trait

In your `controller` use the `Varbox\Traits\CanDraft` trait.   
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use App\Http\Requests\PostRequest;
use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanDraft;

class PostsController extends Controller
{
    use CanDraft;
    ...

    /**
     * Get the model to be drafted.
     *
     * @return string
     */
    protected function draftModel(): string
    {
        return Post::class;
    }

    /**
     * Get the form request to validate the draft upon.
     *
     * @return string|null
     */
    protected function draftRequest(): ?string
    {
        return PostRequest::class;
    }

    /**
     * Get the url to redirect after drafting/publishing.
     *
     * @param Model $model
     * @return string
     */
    protected function draftRedirectTo(Model $model): string
    {
        return route('admin.posts.edit', $model->id);
    }
}
```

<a name="add-blade-code"></a>
#### Add Blade Code

In your `edit` blade view file add the code for displaying the `draft` state of a model record.

```
@if($item->exists)
    // use this check only if you're also implementing revisions in your crud
    @if(!isset($revision))
        @include('varbox::helpers.draft.container', [
            'model' => $item, 
            'route' => 'admin.posts.publish', 
            'permission' => 'pages-publish' // optional
        ])
    @endif
@endif
```

Additionally you'll also want to add the actual button that makes saving records as drafts possible.

```
@if($item->exists)
    @if(!$item->isDrafted())
        @permission('pages-draft')
            @include('varbox::buttons.save_draft', [
                'url' => route('admin.posts.draft', $item->id)
            ])
        @endpermission
    @endif
@else
    @permission('pages-draft')
        @include('varbox::buttons.save_draft', [
            'url' => route('admin.pages.draft')
        ])
    @endpermission
@endif
```

<a name="create-permissions"></a>
#### Create Permissions

You have the possibility to restrict the drafting actions by setting specific permissions.   
The [VarBox](/) platform is using a `role based permission` system out of the box.

> This step is optional and you should follow it only if you've added permissions on your draft routes, or in your blade file.

<p style="margin-bottom: 0;">You should add two permissions:</p>
- one for allowing an admin to save a record as a draft
- one for allowing an admin to publish a drafted record

> As a good practice, it's recommended you create your own `PermissionsSeeder` class to handle this.
> For an in-depth example of such a seeder, please have a look at `Varbox\Seed\PermissionsSeeder`

```php
app(PermissionModelContract::class)->create([
    'group' => 'Posts',
    'label' => 'Draft',
    'guard' => 'admin',
    'name' => 'posts-draft',
]);

app(PermissionModelContract::class)->create([
    'group' => 'Posts',
    'label' => 'Publish',
    'guard' => 'admin',
    'name' => 'posts-publish',
]);
```

<a name="dig-deeper"></a>
#### Dig Deeper

If you still have difficulties in implementing drafts on your own, you can look at how `VarBox Pages` are done. The files you need to inspect are:
- `Varbox\Controllers\PagesController`
- `resources/views/vendor/varbox/admin/pages/_form.blade.php`
