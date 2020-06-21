<h1>Sort Records <small class="free">FREE</small></h1>

- [Usage](#usage)
    - [Sort By Relation](#sort-by-relation)
    - [Change Sorting Parameters](#change-sorting-parameters)
- [Implementation Example](#implementation-example)

This functionality allows you to sort eloquent model records.      

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsSortable` trait.   

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsSortable;

class YourModel extends Model
{
    use IsSortable;

    ...
}
```

You would access by whatever means the desired url that specifies how the sort should behave:

```
/some-list?sort=name&direction=asc
```

> Please note the `sort` and `direction` parameters!   
> <br />
> These are very important if you're using the `IsSortable` trait, as these two parameters are the default ones for defining the field to sort by and the direction to sort in.   

Once you've used the `IsSortable` trait on your models and you've supplied the correct sorting parameters, you can sort the model records by using the `sorted()` query scope from that trait.

```php
YourModel::sorted($request->all())->get();
```

The `sorted` query scope receives a mandatory first argument, that should be an associative array containing the field to sort by and the direction to sort in:

```php
['sort' => 'name', 'direction' => 'asc']
```

<a name="sort-by-relation"></a>
#### Sort By Relation

To sort model records by a relationship, use the following format when specifying the `sort` parameter: `{relationName}.{columnName}`

Please note that you can only sort by relationships of type `belongsTo` or `hasOne`.

```
// sort posts by their author's name in ascending order

/search-posts?sort=author.name&direction=asc
```

<a name="change-sorting-parameters"></a>
#### Change Sorting Parameters

Above we've talked about the importance of specifying the parameters that tell the trait the field to sort by and the direction to sort it. If you wish, those fields can be changed to other fields.   
   
In order to do that, you'll have to create a `Sort` class that will extend the `Varbox\Sorts\Sort` class.

```php
<?php

namespace App\Sorts;

use Varbox\Sorts\Sort;

class YourSort extends Sort
{
    /**
     * Get the request field name to sort by.
     *
     * @return string
     */
    public function field()
    {
        return 'field-to-sort-by';
    }

    /**
     * Get the direction to sort by.
     *
     * @return string
     */
    public function direction()
    {
        return 'direction-to-sort-in';
    }
}
```

After you've created the sort class, pass it as the second argument to the `sorted` query scope when sorting your model records.

```php
YourModel::sorted($request->all(), new YourSort)->get();
```

For the example above, your URI should look like this:

```
/some-list?field-to-sort-by=name&direction-to-sort-in=asc
```

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#sort-records) page.
