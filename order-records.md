<h1>Order Records <small class="free">FREE</small></h1>

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
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to order model records between them.
</p>

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsOrderable` trait and the `Varbox\Options\OrderOptions` class.   

```php
<?php

namespace App\Models;

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

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#order-records) page.
