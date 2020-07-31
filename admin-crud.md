<h1>Admin Crud <small class="free">FREE</small></h1>

- [Usage](#usage)
    - [The Index Method](#the-index-method)
    - [The Create Method](#the-create-method)
    - [The Store Method](#the-store-method)
    - [The Edit Method](#the-edit-method)
    - [The Update Method](#the-update-method)
    - [The Destroy Method](#the-destroy-method)
- [Customizations](#customizations)
    - [Create Success Message](#create-success-message)
    - [Update Success Message](#update-success-message)
    - [Delete Success Message](#delete-success-message)
    - [Record Not Found Message](#record-not-found-message)
    - [Use Database Transactions](#use-database-transactions)
- [Configuration](#configuration)
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to easily setup a system for creating / reading / updating / deleting for your entities.   
</p>

<a name="usage"></a>
## Usage

Your crud controller should use the `Varbox\Traits\CanCrud` trait.

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use Varbox\Traits\CanCrud;

class AdsController extends Controller
{
    use CanCrud;

    ...
}
```

The trait exposes the following methods that you should use.    
These methods are `callable`, meaning the code inside them will be executed by the `CanCrud` trait.

> **Why should you use these methods?**   
>   
> Using these methods will take of your hands a lot of boilerplate such as:
> - verifying the appropriate HTTP verb for each request
> - encapsulating your logic in database transactions
> - displaying the proper success or error message
> - setting the page title and meta title
> - remembering your last query string for appropriate redirection
>   
> Without having to worry about all these stuff, your methods will only contain the bare minimum of code needed for achieving one of the crud operations.

<a name="the-index-method"></a>
#### The Index Method

The `_index()` method is meant to be used inside your `index()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function index()
{
    return $this->_index(function () {
        $this->items = YourModel::query()
            ->paginate(config('varbox.crud.per_page', 30));

        $this->title = 'Your List Title';
        $this->view = view('your.index.view');
        $this->vars = [];
    });
}
```

As you can see, inside the `_index()` method we've initialized four properties:   

- **items** (mandatory)
    - should contain your model records. (paginated, filtered or sorted)
- **title** (mandatory)
    - string representing your page title and meta title
- **view** (mandatory)
    - the blade view you want to use for your corresponding route
- **vars** (optional)
    - any additional view variables that you might want available in your blade view

<a name="the-create-method"></a>
#### The Create Method

The `_create()` method is meant to be used inside your `create()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function create()
{
    return $this->_create(function () {
        $this->title = 'Your Add Title';
        $this->view = view('your.create.view');
        $this->vars = [];
    });
}
```

As you can see, inside the `_create()` method we've initialized three properties:   

- **title** (mandatory)
    - string representing your page title and meta title
- **view** (mandatory)
    - the blade view you want to use for your corresponding route
- **vars** (optional)
    - any additional view variables that you might want available in your blade view
    
<a name="the-store-method"></a>
#### The Store Method

The `_store` method is meant to be used inside your `store()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @param YourRequest $request
 * @return \Illuminate\Http\RedirectResponse
 * @throws \Exception
 */
public function store(YourRequest $request)
{
    return $this->_store(function () use ($request) {
        $this->item = YourModel::create($request->all());
        $this->redirect = redirect()->route('your.index.route');

        // $this->item->someRelation()->save(...)
        // $this->item->someRelation()->create(...)
        // $this->item->someRelation()->attach(...)
    }, $request);
}
```

As you can see, inside the `_store()` method we've initialized two properties:   

- **item** (optional)
    - your loaded model instance
- **redirect** (mandatory)
    - the redirect you want to use for your corresponding route
    
<a name="the-edit-method"></a>
#### The Edit Method

The `_edit` method is meant to be used inside your `edit()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @param YourModel $model
 * @return \Illuminate\View\View
 * @throws \Exception
 */
public function edit(YourModel $model)
{
    return $this->_edit(function () use ($model) {
        $this->item = $model;
        $this->title = 'Your Edit Title';
        $this->view = view('your.edit.view');
        $this->vars = [];
    });
}
```

As you can see, inside the `_edit()` method we've initialized four properties:     

- **item** (mandatory)
    - your loaded model instance also available in your view via the `$item` variable
- **title** (mandatory)
    - string representing your page title and meta title
- **view** (mandatory)
    - the blade view you want to use for your corresponding route
- **vars** (optional)
    - any additional view variables that you might want available in your blade view
    
<a name="the-update-method"></a>
#### The Update Method

The `_update` method is meant to be used inside your `update()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @param YourRequest $request
 * @param YourModel $model
 * @return \Illuminate\Http\RedirectResponse
 * @throws \Exception
 */
public function update(YourRequest $request, YourModel $model)
{
    return $this->_update(function () use ($request, $model) {
        $this->item = $model;
        $this->redirect = redirect()->route('your.index.route');

        $this->item->update($request->all());

        // $this->item->someRelation()->save(...)
        // $this->item->someRelation()->update(...)
        // $this->item->someRelation()->sync(...)
    }, $request);
}
```

As you can see, inside the `_update()` method we've initialized two properties:   

- **item** (optional)
    - your loaded model instance
- **redirect** (mandatory)
    - the redirect you want to use for your corresponding route
    
<a name="the-destroy-method"></a>
#### The Destroy Method

The `_destroy` method is meant to be used inside your `destroy()` method on your controller.   
Apart from what's displayed below, you can always add your own additional logic.

```php
/**
 * @param YourModel $model
 * @return \Illuminate\Http\RedirectResponse
 * @throws \Exception
 */
public function destroy(YourModel $model)
{
    return $this->_destroy(function () use ($model) {
        $this->item = $model;
        $this->redirect = redirect()->route('your.index.route');

        $this->item->delete();
    });
}
```

As you can see, inside the `_destroy()` method we've initialized two properties:   

- **item** (optional)
    - your loaded model instance
- **redirect** (mandatory)
    - the redirect you want to use for your corresponding route

<a name="customizations"></a>
## Customizations

The crud functionality offers a variety of customizations to fit your needs.

<a name="create-success-message"></a>
#### Create Success Message

You can modify the success message displayed to the end-user when a create operation has executed successfully by overriding the `createSuccessMessage()` method in your controller.

```php
/**
 * Get the message to display when a "create" action has been completed successfully.
 *
 * @return string
 */
protected function createSuccessMessage()
{
    return 'The record was successfully created!';
}
```

<a name="update-success-message"></a>
#### Update Success Message

You can modify the success message displayed to the end-user when an update operation has executed successfully by overriding the `updateSuccessMessage()` method in your controller.

```php
/**
 * Get the message to display when an "update" action has been completed successfully.
 *
 * @return string
 */
protected function updateSuccessMessage()
{
    return 'The record was successfully updated!';
}
```

<a name="delete-success-message"></a>
#### Delete Success Message

You can modify the success message displayed to the end-user when a delete operation has executed successfully by overriding the `deleteSuccessMessage()` method in your controller.

```php
/**
 * Get the message to display when a "delete" action has been completed successfully.
 *
 * @return string
 */
protected function deleteSuccessMessage()
{
    return 'The record was successfully deleted!';
}
```

<a name="record-not-found-message"></a>
#### Record Not Found Message

You can modify the error message displayed to the end-user when he tries to access a record that doesn't exist by overriding the `recordNotFoundMessage()` method in your controller.

```php
/**
 * Get the message to display when accessing a non-existent database record.
 *
 * @return string
 */
protected function recordNotFoundMessage()
{
    return 'You are trying to access a record that does not exist!';
}
```

<a name="use-database-transactions"></a>
#### Use Database Transactions

You can specify if you want to use database transactions by using the `shouldUseTransactions()` method in your controller.

```php
/**
 * Determine if crud operations should be wrapped inside database transactions.
 *
 * @return bool
 */
protected function shouldUseTransactions()
{
    return config('varbox.crud.use_transactions') === true;
}
```

<a name="configuration"></a>
## Configuration

The configuration file is located at `config/varbox/crud.php`.

> For more information on how you can customize the crud functionality, please read the comments from that file.

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#basic-crud) page.
