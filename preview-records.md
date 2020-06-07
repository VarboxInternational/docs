# Preview Records

- [Usage](#usage)
    - [Preview Record](#preview-record)
    - [Check If Preview](#check-if-preview)
- [Implementation Example](#implementation-example)

This functionality allows you to preview a model record before saving any changes to it.   
Preview can be available on both `add` and `edit`.  

> It only makes sense to implement the preview functionality for model records that have a visual representation in the frontend (eg. a details page).

<a name="usage"></a>
## Usage

Your controller should use the `Varbox\Traits\CanPreview` trait.   
The trait contains a few abstract methods that you must implement yourself. 

```php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\Controller;
use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\CanPreview;

class YourController extends Controller
{
    use CanPreview;

    /**
     * Get the model to be previewed.
     *
     * @return string
     */
    protected function previewModel(): string
    {
        return YourModel::class;
    }

    /**
     * Get the controller where to dispatch the preview.
     *
     * @param Model $model
     * @return Model
     */
    protected function previewController(Model $model): string
    {
        return YourFrontendController::class;
    }

    /**
     * Get the action where to dispatch the preview.
     *
     * @param Model $model
     * @return Model
     */
    protected function previewAction(Model $model): string
    {
        return 'yourFrontendDetailsMethod';
    }

    /**
     * Get the form request to validate the preview upon.
     *
     * @return string|null
     */
    protected function previewRequest(): ?string
    {
        return YourRequest::class;
    }
}
```

Once you've applied the trait, you'll also need a route to point to the `preview()` method from the `CanPreview` trait.

```php
Route::match(['post', 'put'], 'preview/{yourModel?}', [
    'as' => 'your.preview.route', 
    'uses' => 'YourController@preview', 
]);
````

<a name="preview-record"></a>
#### Preview Record

Now that you've implemented the controller trait and added the method, you can preview any record by accessing that route through a `post` or `put` http verb. 
You could implement a form button to preview your record directly into the entity's `create` or `update` form.

```php
@include('varbox::buttons.preview', ['url' => route('your.preview.route', $model->id)])
```
 
<a name="check-if-preview"></a>
#### Check If Preview

When executing a preview request, a `is_previewing = true` is flashed to the session, so you can check if the request is a preview one.

```php
if(session('is_previewing') == true) {
    // logic only for preview
}
```

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#preview-records) page.
