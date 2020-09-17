# Full Example

<ul>
    <li>
        <a href="#initial-setup">Initial Setup</a>
        <ul>
            <li><a href="#create-migration">Create Migration</a></li>
            <li><a href="#create-model">Create Model</a></li>
        </ul>
    </li>
    <li><a href="#basic-crud">Basic Crud</a>
        <ul>
            <li><a href="#add-crud-routes">Add Routes</a></li>
            <li><a href="#add-crud-permissions">Add Permissions</a></li>
            <li><a href="#create-crud-form-request">Create Form Request</a></li>
            <li><a href="#create-crud-controller">Create Controller</a></li>
            <li><a href="#create-crud-views">Create Views</a></li>
            <li><a href="#add-menu-button">Add Menu Button</a></li>
        </ul>
    </li>
    <li>
        <a href="#filter-records">Filter Records</a>
        <ul>
            <li><a href="#create-filter-class">Create Filter Class</a></li>
            <li><a href="#apply-filter-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-filter-controller-code">Add Controller Code</a></li>
            <li><a href="#add-filter-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#sort-records">Sort Records</a>
        <ul>
            <li><a href="#create-sort-class">Create Sort Class</a></li>
            <li><a href="#apply-sort-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-sort-controller-code">Add Controller Code</a></li>
            <li><a href="#add-sort-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#order-records">Order Records</a>
        <ul>
            <li><a href="#add-order-table-column">Add Table Column</a></li>
            <li><a href="#add-order-route">Add Route</a></li>
            <li><a href="#apply-order-model-trait">Apply Model Trait</a></li>
            <li><a href="#apply-order-controller-trait">Apply Controller Trait</a></li>
            <li><a href="#add-order-controller-code">Add Controller Code</a></li>
            <li><a href="#add-order-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#csv-exports">Csv Exports</a>
        <ul>
            <li><a href="#add-export-route">Add Route</a></li>
            <li><a href="#add-export-permission">Add Permission</a></li>
            <li><a href="#apply-export-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-export-controller-code">Add Controller Code</a></li>
            <li><a href="#add-export-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#upload-files">Upload Files</a>
        <ul>
            <li><a href="#add-upload-table-columns">Add Table Columns</a></li>
            <li><a href="#add-upload-fillable-fields">Add Fillable Fields</a></li>
            <li><a href="#apply-upload-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-upload-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#draft-records">Draft Records</a>
        <ul>
            <li><a href="#add-draft-table-column">Add Table Column</a></li>
            <li><a href="#add-draft-routes">Add Routes</a></li>
            <li><a href="#add-draft-permissions">Add Permissions</a></li>
            <li><a href="#add-draft-cast-field">Add Cast Field</a></li>
            <li><a href="#define-draft-explicit-route-binding">Define Explicit Rotue Binding</a></li>
            <li><a href="#apply-draft-model-trait">Apply Model Trait</a></li>
            <li><a href="#apply-draft-controller-trait">Apply Controller Trait</a></li>
            <li><a href="#add-draft-controller-code">Add Controller Code</a></li>
            <li><a href="#add-draft-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#duplicate-records">Duplicate Records</a>
        <ul>
            <li><a href="#add-duplicate-route">Add Route</a></li>
            <li><a href="#add-duplicate-permission">Add Permission</a></li>
            <li><a href="#apply-duplicate-model-trait">Apply Model Trait</a></li>
            <li><a href="#apply-duplicate-controller-trait">Apply Controller Trait</a></li>
            <li><a href="#add-duplicate-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#preview-records">Preview Records</a>
        <ul>
            <li><a href="#add-preview-route">Add Route</a></li>
            <li><a href="#add-preview-permission">Add Permission</a></li>
            <li><a href="#apply-preview-controller-trait">Apply Controller Trait</a></li>
            <li><a href="#add-preview-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#meta-tags">Meta Tags</a>
        <ul>
            <li><a href="#add-meta-table-column">Add Table Column</a></li>
            <li><a href="#add-meta-fillable-field">Add Fillable Field</a></li>
            <li><a href="#add-meta-cast-field">Add Cast Field</a></li>
            <li><a href="#apply-meta-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-meta-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#activity-log">Activity Log</a>
        <ul>
            <li><a href="#modify-activity-env-variable">Modify Env Variable</a></li>
            <li><a href="#apply-activity-model-trait">Apply Model Trait</a></li>
        </ul>
    </li>
    <li>
        <a href="#model-revisions">Model Revisions</a>
        <ul>
            <li><a href="#add-revision-route">Add Route</a></li>
            <li><a href="#apply-revision-model-trait">Apply Model Trait</a></li>
            <li><a href="#apply-revision-controller-trait">Apply Controller Trait</a></li>
            <li><a href="#add-revision-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#model-url">Model Url</a>
        <ul>
            <li><a href="#add-url-table-column">Add Table Column</a></li>
            <li><a href="#add-url-fillable-field">Add Fillable Field</a></li>
            <li><a href="#apply-url-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-url-blade-code">Add Blade Code</a></li>
            <li><a href="#create-url-controller-class">Create Controller Class</a></li>
            <li><a href="#create-url-blade-file">Create Blade File</a></li>
        </ul>
    </li>
    <li>
        <a href="#translatable-models">Translatable Models</a>
        <ul>
            <li><a href="#modify-translate-table-columns">Modify Table Columns</a></li>
            <li><a href="#apply-translate-model-trait">Apply Model Trait</a></li>
            <li><a href="#attach-translate-middleware">Attach Middleware</a></li>
            <li><a href="#modify-translate-blade-code">Modify Blade Code</a></li>
        </ul>
    </li>
    <li>
        <a href="#content-blocks">Content Blocks</a>
        <ul>
            <li><a href="#apply-block-model-trait">Apply Model Trait</a></li>
            <li><a href="#add-block-controller-code">Apply Controller Code</a></li>
            <li><a href="#add-block-blade-code">Add Blade Code</a></li>
        </ul>
    </li>
</ul>

<blockquote>
    <p id="first-p">
        This example will take you though the journey of building an admin crud for your posts that will contain most of the functionalities.
    </p>
</blockquote>

#### In this example you'll learn how to   
- develop a basic crud system *(see [Admin Crud](/docs/{{version}}/admin-crud))*
- enable activity logging for your records *(see [Activity Log](/docs/{{version}}/activity-log))*
- apply filtering for your records *(see [Filter Records](/docs/{{version}}/filter-records))*
- apply sorting for your records *(see [Sort Records](/docs/{{version}}/sort-records))*
- apply ordering for your records *(see [Order Records](/docs/{{version}}/order-records))*
- support downloading csv exports *(see [Csv Exports](/docs/{{version}}/csv-exports))*
- enable file uploads for your records *(see [Upload Files](/docs/{{version}}/file-uploads))*
- support drafting your records *(see [Draft Records](/docs/{{version}}/draft-records))*
- support revisioning your records *(see [Model Revisions](/docs/{{version}}/model-revisions))*
- support duplicating your records *(see [Duplicate Records](/docs/{{version}}/duplicate-records))*
- attach custom urls to your records *(see [Model Urls](/docs/{{version}}/model-urls))*
- add ability to preview your records *(see [Preview Records](/docs/{{version}}/preview-records))*
- add multi language support for your records *(see [Translatable Models](/docs/{{version}}/multi-language#translatable-models))*
- add seo meta tags to your records *(see [Meta Tags](/docs/{{version}}/meta-tags))*
- add block assignment support for your records *(see [Content Blocks](/docs/{{version}}/content-management#content-blocks))*

<hr>

We've split the above functionalities into their own documentation section, so in the future, when building a crud for your entities, you can come back and read only the parts that interest you.

<a name="initial-setup"></a>
## Initial Setup

> To be able to implement the following example, you'll need an entity to work with.
> For this example we'll assume your entity will be `App\Post` which is a model for your blog posts.
   
<a name="create-migration"></a>
#### Create Migration

Create a new migration file responsible for creating your `posts` table.

```
php artisan make:migration create_posts_table
```

Add a title, subtitle and a description for your posts. Your posts will also belong to users.

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreatePostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();

            $table->bigInteger('user_id')->unsigned()->index();
            $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
                    
            $table->string('title')->unique();
            $table->string('subtitle')->nullable();
            $table->text('description')->nullable();

            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="create-model"></a>
#### Create Model

Create the `app/Post.php` file containing the following:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * The database table.
     *
     * @var string
     */
    protected $table = 'posts';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'user_id',
        'title',
        'subtitle',
        'description',
    ];

    /**
     * A post belongs to a user.
     *
     * @return \Illuminate\Database\Eloquent\Relations\BelongsTo
     */
    public function user()
    {
        return $this->belongsTo(User::class, 'user_id');
    }
}
```

<a name="basic-crud"></a>
## Basic Crud

Let's start creating a basic admin crud system for your posts. Estimated time: **10 minutes**

> For questions you might have following this section, please refer to the [Admin Crud](/docs/{{version}}/admin-crud) and [Roles & Permissions](/docs/{{version}}/roles-and-permissions) documentation pages.

At the end of this example section you'll be able to:
- create, read, update and destroy post records
- restrict access via individual permissions
- validate posts when creating and updating them
- a menu button in the header directing you to the posts section

<a name="add-crud-routes"></a>
#### Add Routes

Inside your `routes/web.php` file, add the following. 

```php
// the admin route group
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin'),
    'middleware' => [
        'varbox.auth.session:admin',
        'varbox.authenticated:admin',
        'varbox.check.roles',
        'varbox.check.permissions',
    ],
], function () {
    // the posts route group
    Route::group([
        'prefix' => 'posts',
    ], function () {
        // the crud routes
        Route::get('/', [
            'as' => 'admin.posts.index',
            'uses' => 'PostsController@index',
            'permissions' => 'posts-list'
        ]);
    
        Route::get('create', [
            'as' => 'admin.posts.create',
            'uses' => 'PostsController@create',
            'permissions' => 'posts-add'
        ]);
    
        Route::post('store', [
            'as' => 'admin.posts.store',
            'uses' => 'PostsController@store',
            'permissions' => 'posts-add'
        ]);
    
        Route::get('edit/{post}', [
            'as' => 'admin.posts.edit',
            'uses' => 'PostsController@edit',
            'permissions' => 'posts-edit'
        ]);
    
        Route::put('update/{post}', [
            'as' => 'admin.posts.update',
            'uses' => 'PostsController@update',
            'permissions' => 'posts-edit'
        ]);
    
        Route::delete('destroy/{post}', [
            'as' => 'admin.posts.destroy',
            'uses' => 'PostsController@destroy',
            'permissions' => 'posts-delete'
        ]);    
    });
});
```

<a name="add-crud-permissions"></a>
#### Add Permissions

Inside your `database/seeds/PermissionsSeeder.php` file add these to the `$permissions` property.

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        'List' => [
            'group' => 'Posts',
            'label' => 'List',
            'guard' => 'admin',
            'name' => 'posts-list',
        ],
        'Add' => [
            'group' => 'Posts',
            'label' => 'Add',
            'guard' => 'admin',
            'name' => 'posts-add',
        ],
        'Edit' => [
            'group' => 'Posts',
            'label' => 'Edit',
            'guard' => 'admin',
            'name' => 'posts-edit',
        ],
        'Delete' => [
            'group' => 'Posts',
            'label' => 'Delete',
            'guard' => 'admin',
            'name' => 'posts-delete',
        ],
    ],
];
```

Seed the permissions by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeder"
```

<a name="create-crud-form-request"></a>
#### Create Form Request

Create the `app/Http/Requests/Admin/PostRequest.php` file containing the following:

```php
<?php

namespace App\Http\Requests\Admin;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rule;

class PostRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        $model = $this->model();

        return [
            'user_id' => [
                'required',
                Rule::exists('users', 'id'),
            ],
            'title' => [
                'required',
                Rule::unique('posts', 'title')->ignore(
                    $model && $model->exists ? $model->id : null
                ),
            ],
        ];
    }

    /**
     * Get the pretty name of attributes.
     *
     * @return array
     */
    public function attributes()
    {
        return [
            'user_id' => 'user',
        ];
    }

    /**
     * Get the model by extracting it from route binding.
     *
     * @return \Illuminate\Routing\Route|object|string|null
     */
    protected function model()
    {
        return $this->route('post') ?: null;
    }
}
```

<a name="create-crud-controller"></a>
#### Create Controller

Create the `app/Http/Controllers/Admin/PostsController.php` file containing the following:

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use App\Http\Requests\Admin\PostRequest;
use App\Post;
use App\Models\User;
use Illuminate\Http\Request;
use Varbox\Traits\CanCrud;

class PostsController extends Controller
{
    use CanCrud;

    /**
     * @var Post
     */
    protected $model;

    /**
     * @param Post $model
     * @return void
     */
    public function __construct(Post $model)
    {
        $this->model = $model;

        view()->share('_model', $this->model);
    }

    /**
     * @param Request $request
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function index(Request $request)
    {
        return $this->_index(function () use ($request) {
            $this->items = $this->model->query()
                ->paginate(config('varbox.crud.per_page', 30));

            $this->title = 'Posts';
            $this->view = view('admin.posts.index');
            $this->vars = [
                'users' => $this->getUsers(),
            ];
        });
    }

    /**
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function create()
    {
        return $this->_create(function () {
            $this->title = 'Add Post';
            $this->view = view('admin.posts.add');
            $this->vars = [
                'users' => $this->getUsers(),
            ];
        });
    }

    /**
     * @param PostRequest $request
     * @return \Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function store(PostRequest $request)
    {
        return $this->_store(function () use ($request) {
            $this->item = $this->model->create($request->all());
            $this->redirect = redirect()->route('admin.posts.index');
        }, $request);
    }

    /**
     * @param Post $post
     * @return \Illuminate\View\View
     * @throws \Exception
     */
    public function edit(Post $post)
    {
        return $this->_edit(function () use ($post) {
            $this->item = $post;
            $this->title = 'Edit Post';
            $this->view = view('admin.posts.edit');
            $this->vars = [
                'users' => $this->getUsers(),
            ];
        });
    }

    /**
     * @param PostRequest $request
     * @param Post $post
     * @return \Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function update(PostRequest $request, Post $post)
    {
        return $this->_update(function () use ($post, $request) {
            $this->item = $post;
            $this->redirect = redirect()->route('admin.posts.index');

            $this->item->update($request->all());
        }, $request);
    }

    /**
     * @param Post $post
     * @return \Illuminate\Http\RedirectResponse
     * @throws \Exception
     */
    public function destroy(Post $post)
    {
        return $this->_destroy(function () use ($post) {
            $this->item = $post;
            $this->redirect = redirect()->route('admin.posts.index');

            $this->item->delete();
        });
    }

    /**
     * @return \Illuminate\Database\Eloquent\Collection
     */
    protected function getUsers()
    {
        return User::excludingAdmins()->alphabetically()->get();
    }
}
```

<a name="create-crud-views"></a>
#### Create Views

Create the `resources/views/admin/posts/index.blade.php` file containing the following:

```php
@extends('varbox::layouts.default')

@section('title', $title)

@section('content')
    <div class="row row-cards">
        <div class="col-lg-3">
            <div class="card">
                <div class="card-body">
                    @permission('posts-add')
                        @include('varbox::buttons.add', ['url' => route('admin.posts.create')])
                    @endpermission
                </div>
            </div>
        </div>
        <div class="col-lg-9">
            @include('admin.posts._table')

            {!! $items->links('varbox::pagination', request()->query()) !!}
        </div>
    </div>
@endsection
```

Create the `resources/views/admin/posts/add.blade.php` file containing the following:

```php
@extends('varbox::layouts.default')

@section('title', $title)

@section('content')
    @include('admin.posts._form', ['url' => route('admin.posts.store')])
@endsection
```

Create the `resources/views/admin/posts/edit.blade.php` file containing the following:

```php
@extends('varbox::layouts.default')

@section('title', $title)

@section('content')
    @include('admin.posts._form', ['url' => route('admin.posts.update', ['post' => $item->id])])
@endsection
```

Create the `resources/views/admin/posts/_table.blade.php` file containing the following:

```php
<div class="card">
    <table class="table card-table table-vcenter">
        <tr>
            <th>Title</th>
            <th class="text-right d-table-cell"></th>
        </tr>
        @forelse($items as $index => $item)
            <tr>
                <td>
                    {{ $item->title ?: 'N/A' }}
                </td>
                <td class="text-right d-table-cell">
                    @permission('posts-edit')
                        @include('varbox::buttons.edit', ['url' => route('admin.posts.edit', ['post' => $item->id])])
                    @endpermission
                    @permission('posts-delete')
                        @include('varbox::buttons.delete', ['url' => route('admin.posts.destroy', ['post' => $item->id])])
                    @endpermission
                </td>
            </tr>
        @empty
            <tr>
                <td colspan="10">No records found</td>
            </tr>
        @endforelse
    </table>
</div>
```

Create the `resources/views/admin/posts/_form.blade.php` file containing the following:

```php
@include('varbox::validation')

@if($item->exists)
    {!! form_admin()->model($item, ['url' => $url, 'method' => 'put', 'class' => 'frm row row-cards', 'files' => true]) !!}
@else
    {!! form_admin()->open(['url' => $url, 'method' => 'post', 'class' => 'frm row row-cards', 'files' => true]) !!}
@endif
<div class="col-md-12">
    <div class="card">
        <div class="card-status bg-blue"></div>
        <div class="card-header">
            <h3 class="card-title">Basic Info</h3>
        </div>
        <div class="card-body">
            <div class="row">
                <div class="col-md-12">
                    {!! form_admin()->text('title', 'Title', null, ['required']) !!}
                </div>
                <div class="col-md-12">
                    {!! form_admin()->select('user_id', 'User', ['' => 'Please select'] + $users->pluck('email', 'id')->toArray(), null, ['required']) !!}
                </div>
                <div class="col-md-12">
                    {!! form_admin()->text('subtitle', 'Subtitle') !!}
                </div>
                <div class="col-md-12">
                    {!! form_admin()->editor('description', 'Description') !!}
                </div>
            </div>
        </div>
    </div>
</div>
<div class="col-12">
    <div class="card">
        <div class="card-body">
            <div class="d-flex text-left">
                @include('varbox::buttons.cancel', ['url' => route('admin.posts.index')])

                @if($item->exists)
                    @include('varbox::buttons.save_stay')
                @else
                    @include('varbox::buttons.save_new')
                    @include('varbox::buttons.save_continue', ['route' => 'admin.posts.edit'])    
                @endif
                
                @include('varbox::buttons.save')
            </div>
        </div>
    </div>
</div>
{!! form_admin()->close() !!}
```

<a name="add-menu-button"></a>
#### Add Menu Button

Inside the `app/Http/Composers/AdminMenuComposer.php` file add a new entry for your posts.

```php
/**
 * Construct the admin menu.
 *
 * @param View $view
 */
public function compose(View $view)
{
    $menu = menu()->make(function (MenuHelper $menu) {
        ...

        $menu->add(function ($item) use ($menu) {
            $blog = $item->name('Blog Area')->data('icon', 'fa-blog')
                ->permissions('posts-list')
                ->active('admin/posts/*');

            $menu->child($blog, function (MenuItem $item) {
                $item->name('Posts')
                    ->url(route('admin.posts.index'))
                    ->permissions('posts-list')
                    ->active('admin/posts/*');
            });
        });

    })->...
}
```

**That's it!** Now you have a working basic admin crud accessible at `/admin/posts`

<a name="activity-log"></a>
## Activity Log

Let's add the possibility of logging post record changes. Estimated time: **5 minutes**   
At the end of this example section you'll be able to view any operation done on a post record inside "Activity Log" admin section.

> For questions you might have following this section, please refer to the [Activity Log](/docs/{{version}}/activity-log) documentation page.

<a name="modify-activity-env-variable"></a>
#### Modify Env Variable

To enable activity logging in your application, update the following in your `.env` file:

```
LOG_ACTIVITY=true
```

<a name="apply-activity-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasActivity` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\ActivityOptions;
use Varbox\Traits\HasActivity;
...

class Post extends Model
{
    use HasActivity;

    /**
     * Set the options for the HasActivity trait.
     *
     * @return ActivityOptions
     */
    public function getActivityOptions()
    {
        return ActivityOptions::instance()
            ->withEntityType('post')
            ->withEntityName($this->title)
            ->withEntityUrl(route('admin.posts.edit', $this->id));
    }

    ...
}
```

**That's it!** Now any operation is logged and can be viewed from the "Activity Log" admin section.

<a name="filter-records"></a>
## Filter Records

Let's add the filtering functionality for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to filter your post records by title / subtitle / description / user / start and end date.

> For questions you might have following this section, please refer to the [Filter Records](/docs/{{version}}/filter-records) documentation page.

<a name="create-filter-class"></a>
#### Create Filter Class

Create the `app/Filters/Admin/PostFilter.php` file containing the following:

```php
<?php

namespace App\Filters\Admin;

use Varbox\Filters\Filter;

class PostFilter extends Filter
{
    /**
     * Get the main where condition between entire request fields.
     *
     * @return string
     */
    public function morph()
    {
        return 'and';
    }

    /**
     * Get the filters that apply to the request.
     *
     * @return array
     */
    public function filters()
    {
        return [
            'search' => [
                'operator' => Filter::OPERATOR_LIKE,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'title,subtitle,description',
            ],
            'user' => [
                'operator' => Filter::OPERATOR_EQUAL,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'user_id',
            ],
            'start_date' => [
                'operator' => Filter::OPERATOR_DATE_GREATER_OR_EQUAL,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'created_at',
            ],
            'end_date' => [
                'operator' => Filter::OPERATOR_DATE_SMALLER_OR_EQUAL,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'created_at',
            ],
        ];
    }

    /**
     * Get the modified value of a request filter field.
     *
     * @return array
     */
    public function modifiers()
    {
        return [];
    }
}
```

<a name="apply-filter-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsFilterable` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Traits\IsFilterable;

class Post extends Model
{
    use IsFilterable;

    ...
}
```

<a name="add-filter-controller-code"></a>
#### Add Controller Code

Now that you've applied the trait, you can use the `filtered` query scope to easily filter your records. 
Inside your `app/Http/Controllers/Admin/PostsController.php` file modify your `index` method:

```php
use App\Filters\Admin\PostFilter;

/**
 * @param Request $request
 * @param PostFilter $filter
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function index(Request $request, PostFilter $filter, ...)
{
    return $this->_index(function () use ($request, $filter, ...) {
        $this->items = $this->model
            ->filtered($request->all(), $filter)
            ->...

        ...
    });
}
```

<a name="add-filter-blade-code"></a>
#### Add Blade Code

Create the `resources/views/admin/posts/_filter.blade.php` file containing the following:

```php
{!! form()->open(['url' => request()->url(), 'method' => 'get', 'class' => 'card ' . (empty(request()->except(['page'])) ? 'card-collapsed' : '')]) !!}

<div class="filter-records-container card-header" data-toggle="card-collapse" style="cursor: pointer;">
    <h3 class="card-title">Filter Records</h3>
    <div class="card-options">
        <a href="#" class="card-options-collapse"><i class="fe fe-chevron-up"></i></a>
    </div>
</div>
<div class="card-body">
    {!! form_admin()->text('search', 'Keyword', request()->query('search') ?: null) !!}
    {!! form_admin()->select('user', 'User', ['' => 'All Users'] + $users->pluck('email', 'id')->toArray(), request()->query('user') ?: null) !!}
    
    <div class="row">
        <div class="col">
            {!! form_admin()->date('start_date', 'From', request()->query('start_date') ?: null) !!}
        </div>
        <div class="col">
            {!! form_admin()->date('end_date', 'To', request()->query('end_date') ?: null) !!}
        </div>
    </div>
</div>
<div class="card-footer text-right">
    @include('varbox::buttons.clear')
    @include('varbox::buttons.filter')
</div>

{!! form()->close() !!}
```

Inside your `resources/views/admin/posts/index.blade.php` file include the `_filter` partial:

```php
<div class="col-lg-3">
    ...

    @include('admin.posts._filter')
</div>
<div class="col-lg-9">
    ...
</div>
```

**That's it!** Now you can filter your records using the filter section on the left.

<a name="sort-records"></a>
## Sort Records

Let's add the filtering sorting for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to sort your post records by their title.

> For questions you might have following this section, please refer to the [Sort Records](/docs/{{version}}/sort-records) documentation page.

<a name="create-sort-class"></a>
#### Create Sort Class

Create the `app/Sorts/Admin/PostSort.php` file containing the following:

```php
<?php

namespace App\Sorts\Admin;

use Varbox\Sorts\Sort;

class PostSort extends Sort
{
    /**
     * Get the request field name to sort by.
     *
     * @return string
     */
    public function field()
    {
        return 'sort';
    }

    /**
     * Get the direction to sort by.
     *
     * @return string
     */
    public function direction()
    {
        return 'direction';
    }
}
```

<a name="apply-sort-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsSortable` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Traits\IsSortable;

class Post extends Model
{
    use IsSortable;

    ...
}
```

<a name="add-sort-controller-code"></a>
#### Add Controller Code

Now that you've applied the trait, you can use the `sorted` query scope to easily sort your records. 
Inside your `app/Http/Controllers/Admin/PostsController.php` file modify your `index` method:

```php
use App\Sorts\Admin\PostSort;

/**
 * @param Request $request
 * @param PostSort $sort
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function index(Request $request, PostSort $sort, ...)
{
    return $this->_index(function () use ($request, $sort, ...) {
        $this->items = $this->model
            ->sorted($request->all(), $sort)
            ->...

        ...
    });
}
```

<a name="add-sort-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_table.blade.php` file modify the table heading:

```php
<table ...>
    <tr>
        <th class="sortable" data-sort="title">
            <i class="fa fa-sort mr-2"></i>Title
        </th>

        ...
    </tr>
    
    ...
</table>
```

**That's it!** Now you can sort your records by their title by clicking on the table heading.

<a name="order-records"></a>
## Order Records

Let's add the ordering functionality to your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to order your post records by drag & dropping them.

> For questions you might have following this section, please refer to the [Order Records](/docs/{{version}}/order-records) documentation page.

<a name="add-order-table-column"></a>
#### Add Table Column

Create a new migration file for adding the `ord` column to your table:

```
php artisan make:migration add_ord_column_to_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddOrdColumnToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->unsignedInteger('ord')->default(0);
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('ord');
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="add-order-route"></a>
#### Add Route

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the order route
    Route::patch('order', [
        'as' => 'admin.posts.order', 
        'uses' => 'PostsController@order', 
        'permissions' => 'posts-list'
    ]); 
});
```

<a name="apply-order-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsOrderable` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\OrderOptions;
use Varbox\Traits\IsOrderable;
...

class Post extends Model
{
    use IsOrderable;

    /**
     * Set the options for the IsOrderable trait.
     *
     * @return OrderOptions
     */
    public static function getOrderOptions()
    {
        return OrderOptions::instance();
    }

    ...
}
```

<a name="apply-order-controller-trait"></a>
#### Apply Controller Trait

Use the `Varbox\Traits\CanOrder` trait inside your `App\Http\Controllers\Admin\PostsController` controller.

```php
<?php

namespace App\Http\Controllers\Admin;

use Varbox\Traits\CanOrder;
...

class PostsController extends Controller
{
    use CanOrder;

    ...
}
```

<a name="add-order-controller-code"></a>
#### Add Controller Code
   
With the model trait applied, you can use the `ordered` query scope to fetch your records in their established order. 
Inside your `app/Http/Controllers/Admin/PostsController.php` file modify your `index()` method:

```php
/**
 * @param Request $request
 * @param PostFilter $filter
 * @param PostSort $sort
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function index(Request $request, PostFilter $filter, PostSort $sort)
{
    return $this->_index(function () use ($request, $filter, $sort) {
        if (count($request->all()) == 0) {
            // fetch records in order when no filtering / sorting is applied
            $this->items = $this->model->ordered()->get();
        } else {
            // fetch records normally when filtering / sorting is applied
            $this->items = $this->model
                ->filtered($request->all(), $filter)
                ->sorted($request->all(), $sort)
                ->paginate(config('varbox.crud.per_page', 30));
        }

        ...
    });
}
```
 
Please note that we apply the `ordered` scope only when no request data exists (eg. no filtering, sorting or pagination parameters are passed in the query string)

<a name="add-order-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/index.blade.php` file, encapsulate the pagination in an `if` statement. 
For ordering records to work properly you shouldn't have paginated records, so only display paginated records when viewing filtered or sorted records.

```php
...

<div class="col-lg-9">
    ...

    @if(count(request()->all()))
        {!! $items->links('varbox::pagination', request()->query()) !!}
    @endif
</div>

...
```

Modify the `resources/views/admin/posts/_table.blade.php` with the following:

```php
<table ...
    data-orderable="{{ empty(request()->all()) ? 'true' : 'false' }}"
    data-order-url="{{ route('admin.posts.order') }}"
    data-order-model="{{ \App\Post::class }}"
    data-order-token="{{ csrf_token() }}">
    
    <tr class="nodrag nodrop">
        ...
    </tr>
    
    @forelse($items as $index => $item)
        <tr id="{{ $item->id }}">
            ...
        </tr>
    @empty
        ...
    @endforelse
</table>
```

**That's it!** Now you can order your records by dragging & dropping the table rows.

<a name="csv-exports"></a>
## Csv Exports

Let's add the possibility to download a csv file for your records. Estimated time: **5 minutes**   
At the end of this example section you'll be able download a csv export for your records. It will also take into consideration your filtering and sorting parameters.

> For questions you might have following this section, please refer to the [Csv Exports](/docs/{{version}}/csv-exports) documentation page.

<a name="add-export-route"></a>
#### Add Route

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the export route
    Route::post('csv', [
        'as' => 'admin.posts.csv', 
        'uses' => 'PostsController@csv', 
        'permissions' => 'posts-export'
    ]);
});
```

<a name="add-export-permission"></a>
#### Add Permission

Inside your `database/seeds/PermissionsSeeder.php` file add this to the `$permissions` property.

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        ...

        'Export' => [
            'group' => 'Posts',
            'label' => 'Export',
            'guard' => 'admin',
            'name' => 'posts-export',
        ],
    ],
];
```

Seed the permission by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeder"
```

<a name="apply-export-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsCsvExportable` trait inside your `App\Post` model. 
The trait contains two abstract methods that you must implement yourself.

```php
<?php

namespace App\Models;

use Varbox\Traits\IsCsvExportable;

class Post extends Model
{
    use IsCsvExportable;

    /**
     * Get the heading columns for the csv.
     *
     * @return array
     */
    public function getCsvColumns()
    {
        return [
            'Title', 'User', 'Created At', 'Last Modified At'
        ];
    }

    /**
     * Get the values for a row in the csv.
     *
     * @return array
     */
    public function toCsvArray()
    {
        return [
            $this->title,
            $this->user && $this->user->exists ? $this->user->email : 'None',
            $this->created_at->format('Y-m-d H:i:s'),
            $this->updated_at->format('Y-m-d H:i:s'),
        ];
    }

    ...
}
```

<a name="add-export-controller-code"></a>
#### Add Controller Code

Inside your `app/Http/Controllers/Admin/PostsController.php` file, create a new method called `csv()`: 

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Filters\Admin\PostFilter;
use App\Sorts\Admin\PostSort;
use Illuminate\Http\Request;
...

class PostsController extends Controller
{
    /**
     * @param Request $request
     * @param PostFilter $filter
     * @param PostSort $sort
     * @return mixed
     */
    public function csv(Request $request, PostFilter $filter, PostSort $sort)
    {
        $items = $this->model/*->withDrafts()*/
            ->filtered($request->all(), $filter)
            ->sorted($request->all(), $sort)
            ->get();

        return $this->model->exportToCsv($items);
    }

    ...
}
```

<a name="add-export-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/index.blade.php` blade file, add the code for displaying the export button.

```php
<div class="col-lg-3">
    <div class="card">
        <div class="card-body">
            ... 

            @permission('posts-export')
                @include('varbox::buttons.csv', [
                    'url' => route('admin.posts.csv', request()->query())
                ])
            @endpermission
        </div>
    </div>

    ...
</div>
<div class="col-lg-9">
    ...
</div>
```

**That's it!** Now you download a csv file for your model records.

<a name="upload-files"></a>
## Upload Files

Let's add upload support for an images and pdfs. Estimated time: **5 minutes**   
At the end of this example section you'll be able to upload an image and a pdf file for post records.

> For questions you might have following this section, please refer to the [File Uploads](/docs/{{version}}/file-uploads) documentation page.

<a name="add-upload-table-columns"></a>
#### Add Table Columns

Create a new migration file for adding the `image` and `pdf` columns to your table:

```
php artisan make:migration add_image_and_pdf_columns_to_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
use Varbox\Contracts\UploadModelContract;

class AddImageAndPdfColumnsToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            app(UploadModelContract::class)->column('image', $table);
            app(UploadModelContract::class)->column('pdf', $table);
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('pdf');
            $table->dropColumn('image');
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="add-upload-fillable-fields"></a>
#### Add Fillable Fields

Inside your `app/Post.php` file add the two upload fields to the `$fillable` property:

```php
/**
 * The attributes that are mass assignable.
 *
 * @var array
 */
protected $fillable = [
    ...

    'image',
    'pdf',
];
```

<a name="apply-upload-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasUploads` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Traits\HasUploads;
...

class Post extends Model
{
    use HasUploads;

    /**
     * Get the specific upload config parts for this model.
     *
     * @return array
     */
    public function getUploadConfig()
    {
        return [
            'images' => [
                'styles' => [
                    'image' => [
                        'square' => [
                            'width' => '200',
                            'height' => '200',
                            'ratio' => true,
                        ],
                        'landscape' => [
                            'width' => '200',
                            'height' => '100',
                            'ratio' => true,
                        ],
                    ],
                ],
            ],
        ];
    }

    ...
}
```

Please note that we've also specified two custom styles for each uploaded image.

<a name="add-upload-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` file, add the fields for image and pdf:

```php
...

<div class="col-md-12">
    <div class="card">
        <div class="card-status bg-blue"></div>
        <div class="card-header">
            <h3 class="card-title">Basic Info</h3>
        </div>
        <div class="card-body">
            <div class="row">
                ...

                <div class="col-md-12">
                    {!! uploader()->field('image')->label('Image')->model($item)->types('image')->accept('jpg', 'png')->manager() !!}
                </div>
                <div class="col-md-12">
                    {!! uploader()->field('pdf')->label('Pdf')->model($item)->types('file')->accept('pdf')->manager() !!}
                </div>
            </div>
        </div>
    </div>
</div>

...
```

**That's it!** Now you can upload images and pdfs for your post records.

<a name="draft-records"></a>
## Draft Records

Let's add the drafting functionality to your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to save a post record as a draft and publish it.

> For questions you might have following this section, please refer to the [Draft Records](/docs/{{version}}/draft-records) documentation page.

<a name="add-draft-table-column"></a>
#### Add Table Column

Create a new migration file for adding the `drafted_at` column to your table:

```
php artisan make:migration add_drafted_at_column_to_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddDraftedAtColumnToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->timestamp('drafted_at')->nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('drafted_at');
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="add-draft-routes"></a>
#### Add Routes

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the draft routes
    Route::post('draft/{post?}', [
        'as' => 'admin.posts.draft', 
        'uses' => 'PostsController@saveDraft', 
        'permissions' => 'posts-draft'
    ]);
    
    Route::put('publish/{post}', [
        'as' => 'admin.posts.publish', 
        'uses' => 'PostsController@publishDraft', 
        'permissions' => 'posts-publish'
    ]);
});
```

<a name="add-draft-permissions"></a>
#### Add Permissions

Inside your `database/seeds/PermissionsSeeder.php` file add these to the `$permissions` property.

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        ...

        'Draft' => [
            'group' => 'Posts',
            'label' => 'Draft',
            'guard' => 'admin',
            'name' => 'posts-draft',
        ],
        'Publish' => [
            'group' => 'Posts',
            'label' => 'Publish',
            'guard' => 'admin',
            'name' => 'posts-publish',
        ],
    ],
];
```

Seed the permissions by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeder"
```

<a name="add-draft-cast-field"></a>
#### Add Cast Field

Cast the `drafted_at` column to a `date` type inside your `App\Post` model.

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

<a name="define-draft-explicit-route-binding"></a>
#### Define Explicit Route Binding

Inside your `app/Providers/AppServiceProvider.php` file, add an explicit route binding for your posts, so you can access them inside the admin panel.

```php
<?php

namespace App\Providers;

use App\Post;
use Illuminate\Support\Facades\Route;
use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Str;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        Route::bind('post', function ($id) {
            $query = Post::whereId($id);

            if ($this->isOnAdminRoute()) {
                $query->withDrafts();
            }

            return $query->first() ?? abort(404);
        });

        ...
    }

    /**
     * @return bool
     */
    protected function isOnAdminRoute()
    {
        return Str::startsWith(
            Route::current()->uri(), 
            config('varbox.admin.prefix') . '/'
        );
    }
}
```

<a name="apply-draft-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsDraftable` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Traits\IsDraftable;

class Post extends Model
{
    use IsDraftable;

    ...
}
```

<a name="apply-draft-controller-trait"></a>
#### Apply Controller Trait

Use the `Varbox\Traits\CanDraft` trait inside your `App\Http\Controllers\Admin\PostsController` controller. 
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Requests\Admin\PostRequest;
use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanDraft;
...

class PostsController extends Controller
{
    use CanDraft;

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

    ...
}
```

<a name="add-draft-controller-code"></a>
#### Add Controller Code

Inside your `app/Http/Controllers/Admin/PostsController.php` file, modify your `index()` method and add the `withDrafts()` query scope to keep fetching all records:

```php
public function index(Request $request, ...)
{
    return $this->_index(function () use ($request, ...) {
        $this->items = $this->model->withDrafts()->...
            // your other methods such as "filtered", "sorted", "ordered"

        ...
    });
}
```

<a name="add-draft-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_table.blade.php` blade file, add the code for displaying the draft status.

```php
<table ...>
    <tr>
        <th class="sortable d-none d-sm-table-cell" data-sort="drafted_at">
            <i class="fa fa-sort mr-2"></i>Published
        </th>
        
        ...
    </tr>
    
    @forelse($items as $index => $item)
        <tr>
            <td class="d-none d-sm-table-cell">
                <span class="badge @if($item->isDrafted()) badge-danger @else badge-success @endif">
                    {{ $item->isDrafted() ? 'No' : 'Yes' }}
                </span>
            </td>

            ...
        </tr>
    @endforelse
</table>
```

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the code for displaying the draft state of a model record.

```php
...

// the code for the form fields
<div class="col-md-12">...</div>

@if($item->exists && !isset($revision))
    @include('varbox::helpers.draft.container', [
        'model' => $item, 
        'route' => 'admin.posts.publish', 
        'permission' => 'posts-publish'
    ])
@endif

// the code for the form buttons
<div class="col-12">...</div>

...
```

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the actual button that makes saving records as drafts possible.

```php
@if($item->exists)
    @if(!$item->isDrafted())
        @permission('posts-draft')
            @include('varbox::buttons.save_draft', [
                'url' => route('admin.posts.draft', $item->id)
            ]) 
        @endpermission
    @endif
@else
    @permission('posts-draft')
        @include('varbox::buttons.save_draft', [
             'url' => route('admin.posts.draft')
        ])
    @endpermission
@endif
```

**That's it!** Now you can save a post as a draft and publish it from the `edit` view of a post record.

<a name="model-revisions"></a>
## Model Revisions

Let's add the revisioning functionality for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to save post revisions inside the `revisions` database table, when updating a post.

> For questions you might have following this section, please refer to the [Model Revisions](/docs/{{version}}/model-revisions) documentation page.

<a name="add-revision-route"></a>
#### Add Route

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the revision route
    Route::get('revision/{revision}', [
        'as' => 'admin.posts.revision', 
        'uses' => 'PostsController@showRevision', 
        'permissions' => 'posts-edit'
    ]);
});
```

<a name="apply-revision-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasRevisions` trait inside your `App\Post` model. 
The trait contains an abstract method `getRevisionOptions()` that you must implement yourself.

```php
<?php

namespace App\Models;

use Varbox\Options\RevisionOptions;
use Varbox\Traits\HasRevisions;

class Post extends Model
{
    use HasRevisions;

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

    ...
}
```

<a name="apply-revision-controller-trait"></a>
#### Apply Controller Trait

Use the `Varbox\Traits\CanRevision` trait inside `App\Http\Controllers\Admin\PostsController` controller. 
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanRevision;
...

class PostsController extends Controller
{
    use CanRevision;

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
        return 'admin.posts.edit';
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
            'users' => $this->getUsers(),
        ];
    }

    ...
}
```

<a name="add-revision-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the code for displaying the revisions.

```php
...

// the code for the form fields
<div class="col-md-12">...</div>

@if($item->exists)
    @include('varbox::helpers.revision.container', [
        'model' => $item, 
        'route' => 'admin.posts.revision', 
        'revision' => $revision ?? null, 
        'parameters' => []
    ])
@endif

// the code for the form buttons
<div class="col-12">...</div>

...
```

Additionally you shouldn't make any of your form buttons available when viewing a revision.

```
@if(!isset($revision))
    <div class="col-12">
        // your buttons here
    </div>
@endif
```

**That's it!** Now a revision is automatically saved everytime you update a post and you can manage your revisions from inside the `edit` view of a post record.

<a name="duplicate-records"></a>
## Duplicate Records

Let's add the possibility to duplicate your post records. Estimated time: **5 minutes**   
At the end of this example section you'll be able to duplicate your post records with just one click.

> For questions you might have following this section, please refer to the [Duplicate Records](/docs/{{version}}/duplicate-records) documentation page.

<a name="add-duplicate-route"></a>
#### Add Route

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the duplicate route
    Route::post('duplicate/{post}', [
        'as' => 'admin.posts.duplicate', 
        'uses' => 'PostsController@duplicate', 
        'permissions' => 'posts-duplicate'
    ]);
});
```

<a name="add-duplicate-permission"></a>
#### Add Permission

Inside your `database/seeds/PermissionsSeeder.php` file add this to the `$permissions` property.

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        ...

        'Duplicate' => [
            'group' => 'Posts',
            'label' => 'Duplicate',
            'guard' => 'admin',
            'name' => 'posts-duplicate',
        ],
    ],
];
```

Seed the permission by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeder"
```

<a name="apply-duplicate-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasDuplicates` trait inside your `App\Post` model. 
The trait contains an abstract method `getDuplicateOptions()` that you must implement yourself.

```php
<?php

namespace App\Models;

use Varbox\Options\DuplicateOptions;
use Varbox\Traits\HasDuplicates;

class Post extends Model
{
    use HasDuplicates;

    /**
     * Set the options for the HasDuplicates trait.
     *
     * @return DuplicateOptions
     */
    public function getDuplicateOptions(): DuplicateOptions
    {
        return DuplicateOptions::instance()
            ->uniqueColumns('title');
    }

    ...
}
```

<a name="apply-duplicate-controller-trait"></a>
#### Apply Controller Trait

Use the `Varbox\Traits\CanDuplicate` trait inside `App\Http\Controllers\Admin\PostsController` controller. 
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanDuplicate;
...

class PostsController extends Controller
{
    use CanDuplicate;

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
        return route('admin.posts.edit', $duplicate->id);
    }

    ...
}
```

<a name="add-duplicate-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the code for displaying the duplicate button.

```php
@if($item->exists)
    @permission('posts-duplicate')
        @include('varbox::buttons.duplicate', [
            'url' => route('admin.posts.duplicate', $item->id)
        ])
    @endpermission
@endif
```

**That's it!** Now you can duplicate your post records while keeping their title unique.

<a name="model-url"></a>
## Model Url

Let's add the the possibility of generating custom urls for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to generate custom urls for your posts and access them in your browser.

> For questions you might have following this section, please refer to the [Model Urls](/docs/{{version}}/model-urls) documentation page.

<a name="add-ul-table-column"></a>
#### Add Table Column

Create a new migration file for adding the `slug` column to your table:

```
php artisan make:migration add_slug_column_to_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddSlugColumnToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->string('slug')->unique()->nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('slug');
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="add-url-fillable-fields"></a>
#### Add Fillable Field

Inside your `app/Post.php` file add the `slug` field to the `$fillable` property:

```php
/**
 * The attributes that are mass assignable.
 *
 * @var array
 */
protected $fillable = [
    ...

    'slug',
];
```

<a name="apply-url-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasUrl` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\UrlOptions;
use Varbox\Traits\HasUrl;
...

class Post extends Model
{
    use HasUrl;

    /**
     * Set the options for the HasUrl trait.
     *
     * @return UrlOptions
     */
    public function getUrlOptions()
    {
        return UrlOptions::instance()
            ->routeUrlTo('App\Http\Controllers\PostsController', 'show')
            ->generateUrlSlugFrom('slug')
            ->saveUrlSlugTo('slug')
            ->prefixUrlWith('posts');
    }

    ...
}
```

<a name="add-url-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` file include the `slug` field:

```php
...

<div class="card-header">
    <h3 class="card-title">Basic Info</h3>
</div>
<div class="card-body">
    <div class="row">
        <div class="col-md-6">
            {!! form_admin()->text('title', 'Title', null, ['required', 'class' => 'js-SlugFrom']) !!}
        </div>
        <div class="col-md-6">
            {!! form_admin()->text('slug', 'Slug', null, ['required', 'class' => 'js-SlugTo']) !!}
        </div>

        ...
    </div>
</div>

...
```

**That's it!** Now everytime you save a create / update a post, a url is automatically attached using the `slug` field as a reference.

<a name="create-url-controller-class"></a>
#### Create Controller Class

Create the `app/Http/Controllers/PostsController` file containing the following:

```php
<?php

namespace App\Http\Controllers;

use App\Post;
use Illuminate\Http\Request;

class PostsController extends Controller
{
    /**
     * @var Post
     */
    protected $post;

    /**
     * @param Request $request
     * @set Page $page
     */
    public function __construct(Request $request)
    {
        $this->post = $request->route()->action['model'] ?? null;

        if (!($this->post && $this->post->exists) || $this->post->isDrafted()) {
            abort(404);
        }
    }

    /**
     * @return \Illuminate\View\View
     */
    public function show()
    {
        return view('posts.show')->with([
            'post' => $this->post
        ]);
    }
}
```

The method inside this controller will be called when you access a post's url.

<a name="create-url-blade-file"></a>
#### Create Blade File

Create the `resources/views/posts/show.blade.php` file containing the following:

```php
<h1>{{ $post->title}}</h1>
<h2>{{ $post->subtitle}}</h2>

{!! $post->description !!}
```

**That's it!** Now everytime you save a create / update a post, a url is automatically attached using the `slug` field as a reference and when you access a post's url in your browser, you'll see its title, subtitle and description

<a name="preview-records"></a>
## Preview Records

Let's add the the possibility to preview post changes without saving them. Estimated time: **5 minutes**   
At the end of this example section you'll be able to preview your changes made to your posts without actually saving them.

> For questions you might have following this section, please refer to the [Preview Records](/docs/{{version}}/preview-records) documentation page.

<a name="add-order-route"></a>
#### Add Route

Inside your `routes/web.php` file, add the following for your posts route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',
], function () {
    // the crud routes
    ...

    // the preview route
    Route::match(['post', 'put'], 'preview/{post?}', [
        'as' => 'admin.posts.preview', 
        'uses' => 'PostsController@preview', 
        'permissions' => 'posts-preview'
    ]);
});
```

<a name="add-duplicate-permission"></a>
#### Add Permission

Inside your `database/seeds/PermissionsSeeder.php` file add this to the `$permissions` property.

```php
/**
 * Mapping structure of admin permissions.
 *
 * @var array
 */
protected $permissions = [
    ...

    'Posts' => [
        ...

        'Preview' => [
            'group' => 'Posts',
            'label' => 'Preview',
            'guard' => 'admin',
            'name' => 'posts-preview',
        ],
    ],
];
```

Seed the permissions by running the following artisan command:

```
php artisan db:seed --class="PermissionsSeeder"
```

<a name="apply-revision-controller-trait"></a>
#### Apply Controller Trait

Use the `Varbox\Traits\CanPreview` trait inside `App\Http\Controllers\Admin\PostsController` controller. 
The trait contains a few abstract methods that you must implement yourself.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\PostsController as FrontPostsController;
use App\Http\Requests\Admin\PostRequest;
use App\Post;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanPreview;
...

class PostsController extends Controller
{
    use CanPreview;

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
        return FrontPostsController::class;
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

    ...
}
```

<a name="add-preview-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the actual button that makes previewing records possible.

```php
@permission('posts-preview')
    @include('varbox::buttons.preview', [
        'url' => route('admin.posts.preview', $item->id)
    ])
@endpermission
```

**That's it!** Now you can preview your records before creating or updating them.

<a name="translatable-models"></a>
## Translatable Models

Let's make your posts support multi language values. Estimated time: **10 minutes**   
At the end of this example section you'll be able to save the subtitle and description for your posts in different languages.

> For questions you might have following this section, please refer to the [Translatable Models](/docs/{{version}}/multi-language#translatable-models) documentation page.

<a name="modify-translate-table-columns"></a>
#### Modify Table Columns

> You might need to install `doctrine/dbal` for this to work. Also, we suggest that you empty the `subtitle` and `description` for all your posts before modifying their type.

Create a new migration file for updating the `subtitle` and `description` columns to `json` type:

```
php artisan make:migration make_subtitle_and_description_columns_as_json_for_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class MakeSubtitleAndDescriptionColumnsAsJsonForPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->json('subtitle')->nullable()->change();
            $table->json('description')->nullable()->change();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->string('subtitle')->nullable()->change();
            $table->text('description')->nullable()->change();
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="apply-translate-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasTranslations` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\TranslationsOptions;
use Varbox\Traits\HasTranslations;
...

class Post extends Model
{
    use HasTranslations;

    /**
     * Set the options for the HasTranslations trait.
     *
     * @return TranslationOptions
     */
    public function getTranslationOptions(): TranslationOptions
    {
        return TranslationOptions::instance()
            ->fieldsToTranslate('subtitle', 'description');
    }

    ...
}
```

<a name="attach-translate-middleware"></a>
#### Attach Middleware

Inside your `routes/web.php` file, attach the `Varbox\Middleware\IsTranslatable` middleware to your entire post route group:

```php
// the posts route group
Route::group([
    'prefix' => 'posts',    
    'middleware' => [
        'varbox.is.translatable',
        ...
    ], 
], function () {
    ...
});
```

Inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\PersistLocale` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\PersistLocale::class
    ],

    ...
];
```

<a name="modify-translate-blade-code"></a>
#### Modify Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` file, modify the subtitle and description fields to support multi language values. 
Please note the use of the `form_admin_lang()` helper for these two fields.

```php
@if($item->exists)
    {!! form_admin_lang()->model(...) !!}
@else
    {!! form_admin_lang()->open(...) !!}
@endif

...

<div class="row">
    ...

    <div class="col-md-12">
        {!! form_admin_lang()->text('subtitle', 'Subtitle') !!}
    </div>
    <div class="col-md-12">
        {!! form_admin_lang()->editor('description', 'Description') !!}
    </div>
    
    ...
</div>

...

{!! form_admin_lang()->close() !!}
```

**That's it!** Now you can save the subtitle and description for your posts in different languages.

<a name="meta-tags"></a>
## Meta Tags

Let's add meta tags support for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able to fill in the meta tags specific for your posts.

> For questions you might have following this section, please refer to the [Meta Tags](/docs/{{version}}/meta-tags) documentation page.

<a name="add-meta-table-column"></a>
#### Add Table Column

Create a new migration file for adding the `meta` column to your table:

```
php artisan make:migration add_meta_column_to_posts_table
```

Inside your generated migration file add the following:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class AddMetaColumnToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->json('meta')->nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->dropColumn('meta');
        });
    }
}
```

Run the migration with the following artisan command:

```
php artisan migrate
```

<a name="add-meta-fillable-field"></a>
#### Add Fillable Field

Inside your `app/Post.php` file add the 'meta' field to the `$fillable` property:

```php
/**
 * The attributes that are mass assignable.
 *
 * @var array
 */
protected $fillable = [
    ...

    'meta',
];
```

<a name="add-meta-cast-field"></a>
#### Add Cast Field

Cast the `meta` column to a `json` type inside your `App\Post` model.

```php
/**
 * The attributes that should be casted to native types.
 *
 * @var array
 */
protected $casts = [
    'meta' => 'array',
    
    ...
];
```

<a name="apply-meta-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasMetaTags` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\MetaTagOptions;
use Varbox\Traits\HasMetaTags;
...

class Post extends Model
{
    use HasMetaTags;

    /**
     * Set the options for the HasMetaTags trait.
     *
     * @return MetaTagOptions
     */
    public function getMetaTagOptions(): MetaTagOptions
    {
        return MetaTagOptions::instance();
    }

    ...
}
```

<a name="add-meta-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the code for displaying the fields for each meta tag.

```php
...

// the code for the form fields
<div class="col-md-12">...</div>

@include('varbox::helpers.meta.container', ['model' => $item ?? null])

// the code for the form buttons
<div class="col-12">...</div>

...
```

Additionally you should paste the following inside your frontend view for your posts, in order to actually display the filled meta tags from the admin:

```
<head>
    {!! $model->displayMetaTags() !}}
</head>
```

**That's it!** Now you can fill in values for your posts' meta tags and display them in your frontend.

<a name="content-blocks"></a>
## Content Blocks

Let's add blocks assignment support for your posts. Estimated time: **5 minutes**   
At the end of this example section you'll be able assign blocks to your posts.

> For questions you might have following this section, please refer to the [Content Blocks](/docs/{{version}}/content-management#content-blocks) documentation page.

<a name="apply-block-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\HasBlocks` trait inside your `App\Post` model.

```php
<?php

namespace App\Models;

use Varbox\Options\BlockOptions;
use Varbox\Traits\HasBlocks;
...

class Post extends Model
{
    use HasBlocks;

    /**
     * Set the options for the HasBlocks trait.
     *
     * @return BlockOptions
     */
    public function getBlockOptions()
    {
        return BlockOptions::instance()
            ->withLocations(['content', 'sidebar']);
    }

    ...
}
```

<a name="add-block-controller-code"></a>
#### Add Controller Code

Inside your `app/Http/Controllers/Admin/PostsController.php` file, modify your `update()` method and add the `saveBlocks()` method to actually persist your assigned blocks:

```php
public function update(PostRequest $request, Post $post)
{
    return $this->_update(function () use ($post, $request) {
        ...

        $this->item->update($request->all());
        $this->item->saveBlocks($request->input('blocks') ?: []);

        ...
    }, $request);
}
```

<a name="add-block-blade-code"></a>
#### Add Blade Code

Inside your `resources/views/admin/posts/_form.blade.php` blade file, add the code for displaying the block assignment section.

```php
...

// the code for the form fields
<div class="col-md-12">...</div>

@if($item->exists)
    @include('varbox::helpers.block.container', [
        'model' => $item, 
        'revision' => $revision ?? null
    ])
@endif

// the code for the form buttons
<div class="col-12">...</div>

...
```

To display your assigned blocks for a given block location, use the `renderBlocks()` method present on the trait.

```php
{!! $model->renderBlocks('content') !!}
```

**That's it!** Now you can assign and display content blocks inside your posts.
