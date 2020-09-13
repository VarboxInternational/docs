<h1>Draft Records <small class="free">FREE</small></h1>

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
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to save eloquent model records in a drafted state.<br />
A record in a drafted state is much like an inactive or a soft deleted record.
</p>   

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsDraftable` trait.   

```php
<?php

namespace App\Models;

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

You can save model records as drafts using the `saveAsDraft()` method present on the trait.

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

You can publish a model record which was saved as a draft using the `publishDraft()` method present on the trait.

When you publish a draft, all it means is that the `drafted_at` column for that model record, will become `null`.

```php
$model = YourModel::find($id);

$model->publishDraft();
```

<a name="check-if-draft"></a>
#### Check If It's Draft

You can check if a model record is drafted or not using the `isDrafted()` method present on the trait.

```php
$model = YourModel::find($id);

dump($model->isDrafted()); // true or false
```

<a name="modify-draft-column"></a>
#### Modify Draft Column

If you'd like, you can modify the name of the `drafted_at` column the `IsDraftable` trait is using behind the scenes. 
To do so, you'll have to overwrite the `getDraftedAtColumn()` method inside your models that use the `IsDraftable` trait.

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

When querying a model that is draftable, the drafted model records will be automatically excluded from all query results. This is because the `IsDraftable` trait applies a global query scope on your models.

<a name="include-drafted-models"></a>
#### Include Drafted Models

As noted above, drafted model records will automatically be excluded from query results. 
However, you may force drafted model records to appear in a result set using the `withDrafts()` query scope.

```php
$allRecords = YourModel::withDrafts()->get();
```

<a name="exclude-drafted-models"></a>
#### Exclude Drafted Models

Even though drafted model records are automatically excluded, you may stumble upon a scenario where you're discarding global scopes on a model by using the `withoutGlobalScopes()` method, but you still don't want to fetch your drafted model records.

You can exclude your drafted records from your query by using the `withoutDrafts()` query scope.

```php
$onlyPublishedRecords = YourModel::withoutDrafts()->get();
```

<a name="retrieve-only-drafted-models"></a>
#### Retrieve Only Drafted Models

You can only retrieve the drafted model records by using the `onlyDrafts()` query scope.

```php
$onlyDraftedRecords = YourModel::onlyDrafts()->get();
```

<a name="eloquent-events"></a>
## Eloquent Events

The draft functionality comes packed with two eloquent events: `drafting` and `drafted`   
   
You can implement these events in your eloquent models as you would implement any other eloquent events that come with the Laravel framework.

```php
<?php

namespace App\Models;

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

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#draft-records) page.
