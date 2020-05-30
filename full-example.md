# Full Example

- [Initial Setup](#initial-setup)
    - [Create Migration](#create-migration)
    - [Create Model](#create-model)
- [Basic Crud](#basic-crud)
    - [Add Routes](#add-crud-routes)
    - [Add Permissions](#add-crud-permissions)
    - [Create Form Request](#create-crud-form-request)
    - [Create Controller](#create-crud-controller)
    - [Create Views](#create-crud-views)
    - [Add Menu Button](#add-menu-button)
- [Filter Records](#filter-records)
    - [Create Filter Class](#create-filter-class)
    - [Apply Model Trait](#apply-filter-model-trait)
    - [Add Controller Code](#add-filter-controller-code)
    - [Create Filter Partial](#create-filter-partial)
    - [Include Filter Partial](#include-filter-partial)
- [Sort Records](#sort-records)
    - [Create Sort Class](#create-sort-class)
    - [Apply Model Trait](#apply-sort-model-trait)
    - [Add Controller Code](#add-sort-controller-code)
    - [Modify Table Partial](#modify-sort-table-partial)
- [Order Records](#order-records)
    - [Add Table Column](#add-order-table-column)
    - [Add Route](#add-order-route)
    - [Apply Model Trait](#apply-order-model-trait)
    - [Apply Controller Trait](#apply-order-controller-trait)
    - [Add Controller Code](#add-order-controller-code)
    - [Modify Index View](#modify-order-index-view)
    - [Modify Table Partial](#modify-order-table-partial)
- [Upload Files](#upload-files)

This example will take your though the journey of building an admin crud for your posts that will contain most of the functionalities, as well as referencing a post page in the frontend.

#### In this example you'll learn how to   
- develop a basic crud system *(see [Admin Crud](/docs/{{version}}/admin-crud))*
- apply filtering for your records *(see [Filter Records](/docs/{{version}}/filter-records))*
- apply sorting for your records *(see [Sort Records](/docs/{{version}}/sort-records))*
- apply ordering for your records *(see [Order Records](/docs/{{version}}/order-records))*
- enable file uploads for your records *(see [Upload Files](/docs/{{version}}/file-uploads))*
- enable activity logging for your records *(see [Activity Log](/docs/{{version}}/activity-log))*
- enable query caching for your records *(see [Query Cache](/docs/{{version}}/query-cache))*
- support drafting your records *(see [Draft Records](/docs/{{version}}/draft-records))*
- support revisioning your records *(see [Model Revisions](/docs/{{version}}/model-revisions))*
- support duplicating your records *(see [Duplicate Records](/docs/{{version}}/duplicate-records))*
- attach custom urls to your records *(see [Model Urls](/docs/{{version}}/model-urls))*
- add ability to preview your records *(see [Preview Records](/docs/{{version}}/preview-records))*
- add seo meta tags to your records *(see [Meta Tags](/docs/{{version}}/meta-tags))*
- add multi language support for your records *(see [Translatable Models](/docs/{{version}}/multi-language#translatable-models))*
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

Add a title and a description for your posts. Your posts will also belong to users.

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

Create the table inside your database by running the following artisan command:

```
php artisan migrate
```

<a name="create-model"></a>
#### Create Model

Create the `app/Post.php` file containing the following:

```php
<?php

namespace App;

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

Let's start creating a basic admin crud system for your posts. 
Below you'll find all the necessary steps you'll need to make in order to achieve this. 
Estimated time: **10 minutes**

> At the end of this section you'll have a fully functional crud for your posts accessible at `/admin/posts`. 
> You'll be able to create, read, update and delete post records.

<a name="add-crud-routes"></a>
#### Add Routes

Inside your `routes/web.php`, add the following:

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
    'middleware' => [
        'varbox.auth.session:admin',
        'varbox.authenticated:admin',
        'varbox.check.roles',
        'varbox.check.permissions',
    ],
], function () {
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
```

<a name="add-crud-permissions"></a>
#### Add Permissions

Inside your `database/seeds/PermissionsSeeder.php` file add these to the `$permissions` property:

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

Store your newly added permissions by running the following artisan command:

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
            'description' => [
                'required',
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
use App\User;
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
            @permission('posts-add')
                @include('varbox::buttons.add', ['url' => route('admin.posts.create')])
            @endpermission
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
                <div class="col-md-6">
                    {!! form_admin()->text('title', 'Title', null, ['required']) !!}
                </div>
                <div class="col-md-6">
                    {!! form_admin()->select('user_id', 'User', ['' => 'Please select'] + $users->pluck('email', 'id')->toArray(), null, ['required']) !!}
                </div>
                <div class="col-md-12">
                    {!! form_admin()->editor('description', 'Description', null, ['required']) !!}
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
                @includeWhen($item->exists, 'varbox::buttons.save_stay')
                @includeWhen(!$item->exists, 'varbox::buttons.save_new')
                @includeWhen(!$item->exists, 'varbox::buttons.save_continue', ['route' => 'admin.posts.edit'])
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

That's it! Now you have a working basic admin crud accessible at `/admin/posts`

<a name="filter-records"></a>
## Filter Records

Let's add the filtering functionality for your post records. 
Below you'll find all the necessary steps you'll need to make in order to achieve this. 
Estimated time: **5 minutes**

> At the end of this section you'll be able to filter your post records (by title, description, user, start and end date), by using the filtering section on the left.

<a name="create-filter-class"></a>
#### Create Filter Class

First you'll have to create your class responsible with the filtering logic.   
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
                'columns' => 'title,description',
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

Use the `Varbox\Traits\IsFilterable` trait on your `App\Post` model.

```php
<?php

namespace App;

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
        $this->items = $this->model->filtered($request->all(), $sort)->...

        ...
    });
}
```

<a name="create-filter-partial"></a>
#### Create Filter Partial

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

<a name="include-filter-partial"></a>
#### Include Filter Partial

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

That's it! Now you can filter your records using the filter section on the left.

<a name="sort-records"></a>
## Sort Records

Let's add the sorting functionality for your post records. 
Below you'll find all the necessary steps you'll need to make in order to achieve this. 
Estimated time: **5 minutes**

<a name="create-sort-class"></a>
#### Create Sort Class

First you'll have to create your class responsible with the sorting logic.   
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

Use the `Varbox\Traits\IsSortable` trait on your `App\Post` model.

```php
<?php

namespace App;

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
        $this->items = $this->model->sorted($request->all(), $sort)->...

        ...
    });
}
```

<a name="modify-sort-table-partial"></a>
#### Modify Table Partial

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

That's it! Now you can sort your records by their title by clicking on the table heading.

<a name="order-records"></a>
## Order Records

Let's add the ordering functionality for your post records. 
Below you'll find all the necessary steps you'll need to make in order to achieve this. 
Estimated time: **5 minutes**

<a name="add-order-table-column"></a>
#### Add Table Column

Create a new migration file for adding the order specific column to your table:

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

Add the column to your table by running the following artisan command:

```
php artisan migrate
```

<a name="add-order-route"></a>
#### Add Route

Create the route that will handle ordering your posts between each other.   
Inside your `routes/web.php`, add the following for your posts route group:

```php
Route::group([
    ...
], function () {
    ...

    Route::patch('order', [
        'as' => 'admin.posts.order', 
        'uses' => 'PostsController@order', 
        'permissions' => 'posts-list'
    ]);
});
```

<a name="apply-order-model-trait"></a>
#### Apply Model Trait

Use the `Varbox\Traits\IsOrderable` trait on your `App\Post` model.

```php
<?php

namespace App;

use Varbox\Options\OrderOptions;
use Varbox\Traits\IsOrderable;
...

class Post extends Model
{
    use IsOrderable;

    /**
     * Get the options for the IsOrderable trait.
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

Use the `Varbox\Traits\CanOrder` trait on your `App\Http\Controllers\Admin\PostsController` controller.

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
   
With the model trait applied, you can use the `ordered` query scope to fetch your records in their order.   
Inside your `app/Http/Controllers/Admin/PostsController.php` file modify your `index` method:

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
            $this->items = $this->model
                ->ordered()
                ->get();
        } else {
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

<a name="modify-order-index-view"></a>
#### Modify Index View

Inside your `resources/views/admin/posts/index.blade.php` modify the following:

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

For ordering records to work properly you shouldn't have paginated records, so only display paginated records when viewing filtered or sorted records.

<a name="modify-order-table-partial"></a>
#### Modify Table Partial

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

That's it! Now you can order your records by dragging & dropping the table rows.

<a name="upload-files"></a>
## Upload Files

Let's add upload support for an image and a pdf field. 
Below you'll find all the necessary steps you'll need to make in order to achieve this. 
Estimated time: **5 minutes**

> At the end of this section you'll be able to upload an image and a pdf file for your post records directly from the admin panel.

<a name="add-upload-table-columns"></a>
#### Add Table Columns

Create a new migration file for adding the upload specific columns to your table:

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

Add the columns to your table by running the following artisan command:

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

Use the `Varbox\Traits\HasUploads` trait on your `App\Post` model.

```php
<?php

namespace App;

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

<a name="add-upload-form-fields"></a>
#### Add Form Fields

Modify the `resources/views/admin/posts/_form.blade.php` file with the following:

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

That's it! Now you can upload images and pdfs for your post records.
