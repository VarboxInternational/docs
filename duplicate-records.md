<h1>Duplicate Records</h1>

- [Usage](#usage)
- [Customizations](#customizations)
    - [Exclude Columns](#exclude-columns)
    - [Unique Columns](#unique-columns)
    - [Exclude Relations](#exclude-relations)
    - [Exclude Relation Columns](#exclude-relation-columns)
    - [Unique Relation Columns](#unique-relation-columns)
    - [Disable Deep Duplication](#disable-deep-duplication)
- [Eloquent Events](#eloquent-events)
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to duplicate any model record along with its underlying relationships.
</p> 

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\HasDuplicates` trait and the `Varbox\Options\DuplicateOptions` class. 
The trait contains an abstract method `getDuplicateOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App\Models;

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

Once you've used the `HasDuplicates` trait on your model, you can duplicate model records using the `saveAsDuplicate()` method.

```php
$model = YourModel::find($id);

$duplicatedModel = $model->saveAsDuplicate();
```

<a name="customizations"></a>
## Customizations

The duplicate functionality offers a variety of customizations to fit your needs.

<a name="exclude-columns"></a>
#### Exclude Columns

When duplicating a model you can exclude certain columns from being duplicated using the `excludeColumns()` method in your definition of the `getDuplicateOptions()` method.

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

When duplicating a model you can save certain columns in an unique format using the `uniqueColumns()` method in your definition of the `getDuplicateOptions()` method.   

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

When duplicating a model you can exclude certain columns of its child relations from being duplicated by using the `excludeRelationColumns()` method in your definition of the `getDuplicateOptions()` method.   
   
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

When duplicating a model you can save certain columns of its child relations in a unique format using the `uniqueRelationColumns()` method in your definition of the `getDuplicateOptions()` method.   
   
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

namespace App\Models;

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

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#duplicate-records) page.
