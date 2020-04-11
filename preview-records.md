# Preview Records

- [Crud Implementation](#crud-implementation)
    - [Create The Route](#create-the-route)
    - [Apply Controller Trait](#apply-controller-trait)
    - [Add Blade Code](#add-blade-code)
    - [Create Permission](#create-permission)
    - [Dig Deeper](#dig-deeper)
- [Check If Preview](#check-if-preview)

This functionality allows you to preview a model record before saving any changes to it.   
Preview can be available on both `add` and `edit`.  

> It only makes sense to implement the preview functionality for model records that have a visual representation in the frontend (eg. a details page).

<a name="crud-implementation"></a>
## Crud Implementation

> Record previewing is an admin specific functionality, so below you'll learn how to attach the preview functionality to your CRUDs.

For the example below we'll assume the following:
- you have a `App\Post` model that holds all your posts
- you have a `App\Http\Controllers\PostsController` controller and a `show` method responsible for displaying details for a single post
- you have a `App\Http\Controllers\Admin\PostsController` responsible for the admin crud of your posts

<a name="create-the-route"></a>
#### Create The Route

Inside your `routes/web.php` file, append the following route to your crud routes. In the below example we're assuming you're using a `route group` to group your post routes.

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
], function () {
    // your other post crud routes

    Route::match(['post', 'put'], 'preview/{post?}', [
        'as' => 'admin.posts.preview', 
        'uses' => 'PostsController@preview', 
        'permissions' => 'pages-preview' // optional to restrict access
    ]);
});
```

<a name="apply-controller-trait"></a>
#### Apply Controller Trait

In your `App\Http\Controllers\Admin\PostsController` use the `Varbox\Traits\CanPreview` trait.   
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use App\Http\Requests\PostRequest;
use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanPreview;

class PostsController extends Controller
{
    use CanPreview;
    ...

    /**
     * Get the model to be previewed.
     *
     * @return string
     */
    protected function previewModel(): string
    {
        return Post::class;
    }

    /**
     * Get the controller where to dispatch the preview.
     *
     * @param Model $model
     * @return Model
     */
    protected function previewController(Model $model): string
    {
        return \App\Http\Controllers\PostsController::class;
    }

    /**
     * Get the action where to dispatch the preview.
     *
     * @param Model $model
     * @return Model
     */
    protected function previewAction(Model $model): string
    {
        return 'show';
    }

    /**
     * Get the form request to validate the preview upon.
     *
     * @return string|null
     */
    protected function previewRequest(): ?string
    {
        return PostRequest::class;
    }
}
```

<a name="add-blade-code"></a>
#### Add Blade Code

In your `edit` blade view file add the preview button.

```
@permission('posts-preview')
    @include('varbox::buttons.preview', [
        'url' => route('admin.posts.preview', $item->getKey())
    ])
@endpermission
```

<a name="create-permission"></a>
#### Create Permission

You have the possibility to restrict the preview action by setting a specific permission.   
The `VarBox` platform is using a `role based permission` system out of the box.

> This step is optional and you should follow it only if you've added permissions on your preview route, or in your blade file.

You should add a permission for allowing the admin to preview a model record.

> As a good practice, it's recommended you create your own `PermissionsSeeder` class to handle this.
> For an in-depth example of such a seeder, please have a look at `Varbox\Seed\PermissionsSeeder`

```php
app(PermissionModelContract::class)->create([
    'group' => 'Posts',
    'label' => 'Preview',
    'guard' => 'admin',
    'name' => 'posts-preview',
]);
```

<a name="dig-deeper"></a>
#### Dig Deeper

If you still have difficulties in implementing preview on your own, you can look at how `VarBox Pages` are done. The files you need to inspect are:
- `Varbox\Controllers\PagesController`
- `resources/views/vendor/varbox/admin/pages/_form.blade.php`

<a name="check-if-preview"></a>
## Check If Preview

When executing a preview request, a `is_previewing = true` is flashed to the session, so you can check if the request is a preview one.

```php
if(session('is_previewing') == true) {
    // logic only for preview
}
```
