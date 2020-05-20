# Full Example

- [Create Migration](#create-migration)
- [Create Routes](#create-routes)
- [Create Model](#create-model)
- [Create Permissions](#create-permissions)
- [Create Request](#create-request)
- [Create Filter](#create-filter)
- [Create Sort](#create-sort)
- [Create Controller](#create-controller)
- [Create Views](#create-views)

This example illustrates how to create a fully functional `crud` system for your entities.

#### In this example you'll learn how to   
- develop a basic crud system *(see [Admin Crud](#usage))*
- draft / publish your records *(see [Draft Records](/docs/{{version}}/draft-records))*
- attach revisions to your records *(see [Model Revisions](/docs/{{version}}/model-revisions))*
- support your records being duplicated *(see [Duplicate Records](/docs/{{version}}/duplicate-records))*
- support uploading files for your record *(see [Upload Files](/docs/{{version}}/file-uploads))*
- attach custom urls to your records *(see [Model Urls](/docs/{{version}}/model-urls))*
- preview your records *(see [Preview Records](/docs/{{version}}/preview-records))*
- support adding blocks of content *(see [Content Blocks](/docs/{{version}}/content-blocks))*
- integrate activity log for your records *(see [Activity Log](/docs/{{version}}/activity-log))*
- filter your records *(see [Filter Records](/docs/{{version}}/filter-records))*
- sort your records *(see [Sort Records](/docs/{{version}}/sort-records))*
- cache your model queries *(see [Query Cache](/docs/{{version}}/query-cache))*

The following example files will contain support for all the functionalities above. It'll be up to you to remove the things you don't want.

#### For this example we'll assume
- you want to create a full crud system for your `App\Post` model
- your model will have the fields: `user_id`, `name`, `date`, `content`, `image`, `file`

Let's start!

<a name="create-migration"></a>
### Create Migration

Your `App\Post` model will need a table, so let's create one with the fields we specified above.
Create a migration using the `php artisan make:migration` command and inside it, write the following:

```php
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
        
        $table->string('name')->unique();
        $table->timestamp('date')->nullable();
        $table->text('content')->nullable();

        // this field is used to store the slug of the url
        // remove this if you don't want to support "custom urls"
        $table->string('slug')->unique();
        
        // the "column" method is used for creating file fields
        // remove this if you don't want to support "file uploads"
        app(\Varbox\Models\Upload::class)->column('image', $table);
        app(\Varbox\Models\Upload::class)->column('video', $table);
        
        // this field is used to flag a model as drafted
        // remove this if you don't want to support "drafts"
        $table->timestamp('drafted_at')->nullable();

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
```

After you've added the code to your migration, you should execute it.

```
php artisan migrate
```

<a name="create-routes"></a>
### Create Routes

Next you'll need the `admin` routes. Inside your `routes/web.php` file write the following:

```php
// this is the big route group
// should contain all your admin routes
Route::group([
    'namespace' => 'Admin',
    
    // use the configurable "admin" prefix
    'prefix' => config('varbox.admin.prefix', 'admin'),
    
    // add the "admin" middleware
    'middleware' => [
        'varbox.auth.session:admin',
        'varbox.authenticated:admin',
        'varbox.check.roles',
        'varbox.check.permissions',
    ]
], function () {

    // this is the posts route group
    // should contain all your post admin routes
    Route::group([
        'prefix' => 'posts',
    ], function () {
        // crud routes
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

        // draft routes
        // remove this part if you don't want to support "drafts"
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
        
        // revision route
        // remove this part if you don't want to support "revisions"
        Route::get('revision/{revision}', [
            'as' => 'admin.posts.revision', 
            'uses' => 'PostsController@showRevision', 
            'permissions' => 'posts-edit'
        ]);
    
        // duplicate route
        // remove this part if you don't want to support "duplicate"
        Route::post('duplicate/{post}', [
            'as' => 'admin.posts.duplicate', 
            'uses' => 'PostsController@duplicate', 
            'permissions' => 'posts-duplicate'
        ]);

        // preview route
        // remove this part if you don't want to support "preview"
        Route::match(['post', 'put'], 'preview/{post?}', [
            'as' => 'admin.posts.preview', 
            'uses' => 'PostsController@preview', 
            'permissions' => 'posts-preview'
        ]);
    });
});
```

<a name="create-model"></a>
### Create Model

Next up you should create your actual eloquent model class.   
Inside your `app` directory, create a new file called `Post.php`.

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\ActivityOptions;
use Varbox\Options\BlockOptions;
use Varbox\Options\DuplicateOptions;
use Varbox\Options\RevisionOptions;
use Varbox\Options\UrlOptions;
use Varbox\Traits\HasActivity;
use Varbox\Traits\HasBlocks;
use Varbox\Traits\HasDuplicates;
use Varbox\Traits\HasRevisions;
use Varbox\Traits\HasUploads;
use Varbox\Traits\HasUrl;
use Varbox\Traits\IsDraftable;
use Varbox\Traits\IsCacheable;
use Varbox\Traits\IsFilterable;
use Varbox\Traits\IsSortable;

class Post extends Model
{
    use HasActivity;
    use HasBlocks;
    use HasDuplicates;
    use HasRevisions;
    use HasUploads;
    use HasUrl;
    use IsCacheable;
    use IsDraftable;
    use IsFilterable;
    use IsSortable;

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
        'name',
        'slug',
        'date',
        'content',
        'image',
        'video',
    ];

    /**
     * The attributes that should be mutated to dates.
     *
     * @var array
     */
    protected $dates = [
        'date',
        'drafted_at',
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

    /**
     * Get the options for the HasActivity trait.
     *
     * @return ActivityOptions
     */
    public function getActivityOptions()
    {
        return ActivityOptions::instance()
            ->withEntityType('post')
            ->withEntityName($this->name)
            ->withEntityUrl(route('admin.posts.edit', $this->id));
    }

    /**
     * Get the options for the HasBlocks trait.
     *
     * @return BlockOptions
     */
    public function getBlockOptions()
    {
        return BlockOptions::instance()
            ->withLocations('header', 'content', 'footer');
    }

    /**
     * Get the options for the HasDuplicates trait.
     *
     * @return DuplicateOptions
     */
    public function getDuplicateOptions()
    {
        return DuplicateOptions::instance()
            ->uniqueColumns('name', 'slug')
            ->excludeRelations('user', 'url', 'revisions');
    }

    /**
     * Get the options for the HasRevisions trait.
     *
     * @return RevisionOptions
     */
    public function getRevisionOptions()
    {
        return RevisionOptions::instance()
            ->limitRevisionsTo(30);
    }

    /**
     * Get the options for the HasUrl trait.
     *
     * @return UrlOptions
     */
    public function getUrlOptions()
    {
        return UrlOptions::instance()
            ->routeUrlTo('\App\Http\Controllers\PostController', 'show')
            ->generateUrlSlugFrom('name')
            ->saveUrlSlugTo('slug')
            ->prefixUrlWith('posts');
    }

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
                    'data[image]' => [
                        'portrait' => [
                            'width' => '100',
                            'height' => '300',
                            'ratio' => true,
                        ],
                        'landscape' => [
                            'width' => '300',
                            'height' => '100',
                            'ratio' => true,
                        ],
                    ],
                ],
            ],
        ];
    }
}
```
