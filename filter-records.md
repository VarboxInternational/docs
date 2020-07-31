<h1>Filter Records <small class="free">FREE</small></h1>

- [Usage](#usage)
    - [Filter By Relation](#filter-by-relation)
- [Filter Class Explained](#filter-class-explained)
    - [The `filters` Method](#the-filters-method)
    - [The `morph` Method](#the-morph-method)
    - [The `modifiers` Method](#the-modifiers-method)
- [Available Filter Operators](#available-filter-operators)
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to filter eloquent model records by defining filter rules, similar to how Laravel's form request rules work.
</p>   

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsFilterable` trait.   

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsFilterable;

class YourModel extends Model
{
    use IsFilterable;

    ...
}
```

Next, you should create a filter class to define the filtering rules. It's recommended you keep all your filter classes inside an `app/Filters` directory.

> To learn more about what a filter class should contain please read [this section](#filter-class-explained)

Your filter class should extend the `Varbox\Filters\Filter` and implement the `filters()`, `morph()` and `modifiers()` methods.

```php
<?php

namespace App\Filters;

use Varbox\Filters\Filter;

class YourFilter extends Filter
{
    /**
     * Get the filters that apply to the request.
     *
     * @return array
     */
    public function filters()
    {
        return [
            'keyword' => [
                'operator' => Filter::OPERATOR_LIKE,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'name,content',
            ],
            'status' => [
                'operator' => Filter::OPERATOR_EQUAL,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'status',
            ],
            'date' => [
                'operator' => Filter::OPERATOR_DATE_GREATER_OR_EQUAL,
                'condition' => Filter::CONDITION_OR,
                'columns' => 'date',
            ],
        ];
    }

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

Once you've completed the above steps you can filter your model records by using the `filtered` query scope. 
This query scope accepts two parameters:   
- an array representing the key / value pairs on which filtering will work
- an instance of your filter class that will define the filtering logic

```php
YourModel::filtered($request->all(), new YourFilter)->get();
```

<a name="filter-by-relation"></a>
#### Filter By Relation

You can also filter model records by a field from a relation.

To specify that you want to filter a certain field by a related field, you should specify `{relationName.columnName}` as the value for the `columns` key present on the `filters()` method.

```php
public function filters()
{
    return [
        'keyword' => [
            'operator' => Filter::OPERATOR_LIKE,
            'condition' => Filter::CONDITION_OR,
            'columns' => 'relation.column',
        ],
   ];
}
```

<a name="filter-class-explained"></a>
## Filter Class Explained

Up until this point you've learned how to implement the filtering functionality on your models, but you might still have some questions about how the filter class actually works and what it should contain.

The filter class is used to define the filtering logic on which the `filtered` query scope will function.

<a name="the-filters-method"></a>
#### The `filters` Method

This method is used to define your actual filtering rules, that will later on be passed to the `filtered` query scope. 
This method should always return an array of this format:

```php
public function filters()
{
    return [
        '{query_field}' => [
            'operator' => Filter::OPERATOR_LIKE,
            'condition' => Filter::CONDITION_OR,
            'columns' => 'name,content',
        ],
       
        // append as many rules you want for as many query fields
   ];
}
```

The `{query_field}` should be replaced with the name of your actual query field from your url.   
For this url: `/search?keyword=test&status=1` you should replace `{query_field}` with either `keyword` or `status`. 

The `operator` defines the way filtering should be done for the respective query field.   
You can choose from a variety of filter operators listed [here](#available-filter-operators).

The `condition` applies only if you specify multiple columns for a query field.   
It defines how filtering between the columns should behave.   
The available values are: `Filter::CONDITION_AND` or `FILTER::CONDITION_OR`   

The `columns` defines on which database table columns the query should filter against.   
You can specify multiple columns by using `comma (,)` between column names.

<a name="the-morph-method"></a>
#### The `morph` Method

This method defines how filtering conditions between multiple query fields should be handled.   
It should return either `and` or `or`.

```php
public function morph()
{
    return 'and';
}
```

If your `morph` method returns `and` the final query will look like this:

```php
->where('query_field_1', 'some value')
->where('query_field_2', 'some value')
->...
```

If your `morph` method returns `or` the final query will look like this:

```php
->orWhere('query_field_1', 'some value')
->orWhere('query_field_2', 'some value')
->...
```

#### The `modifiers` method

In most cases this method will return an `empty array`.

However, you can use this method if you want to filter by a value, but your database table column holds that value in another format.

A good example for this would be to filter files by size in `megabytes`, but you store the size in `bytes` at database level.

```php
public function modifiers()
{
    return [
        // assuming you have "max_size" defined your "filters()" method

        'max_size' => function ($modified) {
            return request()->query('size')* pow(1024, 2);
        },
    ];
}
``` 

<a name="available-filter-operators"></a>
## Available Filter Operators

The `Varbox\Filters\Filter` class exposes a few filter operators that you can use inside your `filters()` method in order to define the filter behavior for certain query string fields.

<style>
    #available-filter-operators-list > p {
        column-count: 2; -moz-column-count: 2; -webkit-column-count: 2;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list span.list-item {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
    }
</style>

<div id="available-filter-operators-list" markdown="1">

<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NOT_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_SMALLER</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_GREATER</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_SMALLER_OR_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_GREATER_OR_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NULL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NOT_NULL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_IN</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NOT_IN</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_LIKE</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NOT_LIKE</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_BETWEEN</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_NOT_BETWEEN</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_NOT_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_SMALLER</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_GREATER</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_SMALLER_OR_EQUAL</span></span>
<span class="list-item">Filter::<span style="color: #4AAEE3">OPERATOR_DATE_GREATER_OR_EQUAL</span></span>

</div>

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#filter-records) page.




