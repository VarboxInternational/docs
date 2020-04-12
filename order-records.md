# Order Records

- [Usage](#usage)
    - [Order Records](#order-records)
    - [Fetch Records In Order](#fetch-records-in-order)
    - [Move Up](#move-up)
    - [Move Down](#move-down)
    - [Move To Start](#move-to-start)
    - [Move To End](#move-to-end)
    - [Swap Order](#swap-order)
- [Customizations](#customizations)
    - [Change Order Column](#change-order-column)
    - [Disable Order When Creating](#disable-order-when-creating)
- [Crud Implementation](#crud-implementation)
    - [Create The Route](#create-the-route)
    - [Apply Controller Trait](#apply-controller-trait)
    - [Modify Index Method](#modify-index-method)
    - [Add Blade Code](#add-blade-code)

This functionality allows you to order model records between them.

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsOrderable` trait and the `Varbox\Options\OrderOptions` class.   

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\OrderOptions;
use Varbox\Traits\IsOrderable;

class YourModel extends Model
{
    use IsOrderable;

    /**
     * Get the options for ordering the model.
     *
     * @return OrderOptions
     */
    public function getOrderOptions(): OrderOptions
    {
        return OrderOptions::instance();
    }
}
```

Next you should create an `ord` table column for your model table.   
This column will be responsible for storing the model record's order number.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->unsignedInteger('ord')->default(0);
});
```

#### Order Records

An easy method to order all you records is to use the `setNewOrder()` method.

This method accepts an `array of model ids` and will set the order incrementally starting from the first array value to the last.
In other words, the order will be established based on the array you pass.

```php
$items = YourModel::latest()->get();
$array = $items->pluck('id')->toArray();

YourModel::setNewOrder(array_values($array));
```

<a name="fetch-records-in-order"></a>
#### Fetch Records In Order

You can fetch model records in the order you established by using the `ordered` query scope.

```php
$items = YourModel::ordered()->get();
```

<a name="move-up"></a>
#### Move Up

You can swap the order of your model with model above it by using the `moveOrderUp()` method.

```php
$model = YourModel::find($id);

$model->moveOrderUp();
```

<a name="move-down"></a>
#### Move Down

You can swap the order of your model with model below it by using the `moveOrderDown()` method.

```php
$model = YourModel::find($id);

$model->moveOrderDown();
```

<a name="move-to-start"></a>
#### Move To Start

You can set a certain model to be at the first position by using the `moveToStart()` method.

```php
$model = YourModel::find($id);

$model->moveToStart();
```

<a name="move-to-end"></a>
#### Move To End

You can set a certain model to be at the last position by using the `moveToEnd()` method.

```php
$model = YourModel::find($id);

$model->moveToEnd();
```

<a name="swap-order"></a>
#### Swap Order

You can swap the order of two models by using the `swapOrder()` method.

```php
$model1 = YourModel::find($id);
$model2 = YourModel::find($id);

$model2->swapOrderWithModel($model1);

// or the static call version

YourModel::swapOrder($model2, $model1);
```

<a name="customizations"></a>
## Customizations

The order functionality offers a variety of customizations to fit your needs.

<a name="change-order-column"></a>
#### Change Order Column

By default, the ordering functionality works based on a `ord` table column.   

You can change the name of that column by using the `setOrderColumn()` method in your definition of the `getOrderOptions()` method.

```php
/**
 * Set the options for the IsOrderable trait.
 *
 * @return OrderOptions
 */
public function getOrderOptions(): OrderOptions
{
    return OrderOptions::instance()
        ->setOrderColumn('position');
}
```

<a name="disable-order-when-creating"></a>
#### Disable Order When Creating

By default, when creating a new model record, the `ord` value for it will be the highest, thus making it last in order.

You can disable this behavior by using the `doNotOrderWhenCreating()` method in your definition of the `getOrderOptions()` method.

Please note that by doing so the value of the `ord` column will be the default one.

```php
/**
 * Set the options for the IsOrderable trait.
 *
 * @return OrderOptions
 */
public function getOrderOptions(): OrderOptions
{
    return OrderOptions::instance()
        ->doNotOrderWhenCreating();
}
```

<a name="crud-implementation"></a>
## Crud Implementation

Below you'll learn how you can implement the order functionality in your own CRUDs inside the [VarBox](/) admin. For the example below we'll assume the following:
- you have a `App\Post` model that holds all your posts
- you're already using the `Varbox\Traits\IsOrderable` trait on your model
- you've already created an `ord` table column for your model
- you have a `App\Http\Controllers\Admin\PostsController` on which to apply ordering

> At the end of this guide you'll be able to drag & drop records between them in order to set their order.

<a name="create-the-route"></a>
#### Create The Route

Inside your `routes/web.php` file, append the following route to your crud routes. In the below example we're assuming you're using a `route group` to group your post routes.

```php
Route::group([
    'namespace' => 'Admin',
    'prefix' => config('varbox.admin.prefix', 'admin') . '/posts',
], function () {
    // your other post crud routes

    Route::patch('order', [
        'as' => 'admin.posts.order', 
        'uses' => 'PostsController@order', 
    ]);
});
```

<a name="apply-controller-trait"></a>
#### Apply Controller Trait

In your `App\Http\Controllers\Admin\PostsController` use the `Varbox\Traits\CanOrder` trait.   
This trait contains the actual `order()` method you referenced your route in the above step.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use Varbox\Traits\CanOrder;

class PostsController extends Controller
{
    use CanOrder;

    ...
}
```

<a name="modify-index-method"></a>
#### Modify Index Method

Inside your `App\Http\Controllers\Admin\PostsController` you should modify the `index` method as follows.

```php
public function index(Request $request, AttributeFilter $filter, AttributeSort $sort)
{
    return $this->_index(function () use ($request, $filter, $sort) {
        // only apply ordered sort if no filtering is applied of any kind
        // also, don't paginate the records
        
        if (count($request->all()) == 0) {
            $this->items = $this->model->ordered()->get();
        } else {
            $this->items = $this->model
                ->filtered($request->all(), $filter)
                ->sorted($request->all(), $sort)
                ->paginate(config('crud.per_page'));
        }
    
        ...
    });
}
```

<a name="add-blade-code"></a>
#### Add Blade Code

In your `index` blade view file add the following attributes to the `table` holding your records.

The admin is already equipped with the javascript code necessary of doing an ajax request to order your records. You just need to specify a couple of parameters for that to work.

```
<table class="table card-table table-vcenter"
   data-orderable="{{ empty(request()->all()) ? 'true' : 'false' }}"
   data-order-url="{{ route('admin.posts.order') }}"
   data-order-model="{{ \App\Post::class }}"
   data-order-token="{{ csrf_token() }}">
```

Additionally, still in your `index` blade view file, you'll need to add an `id` to every `table row`

```
@foreach($items as $item)
    <tr id="{{ $item->id }}">
        ...
    </tr>
@endforeach
```
 
And finally you're going to have to disable the `pagination` for when you want the order functionality active. 
As established [above](#modify-index-method) you should only apply ordering when displaying all records in the same page.

In your `index` blade view file, display the pagination conditionally.

```
@if(count(request()->all()))
    {!! $items->links('varbox::pagination.default', request()->query()) !!}
@endif
```
