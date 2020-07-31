<h1>Translatable Models <small class="paid">PAID</small></h1>

- [Setup](#setup)
- [Usage](#translatable-models)
    - [Make Model Translatable](#make-model-translatable)
    - [Save Translated Values](#save-translated-values)
    - [Display Translated Values](#display-translated-values)
    - [Admin Implementation](#admin-implementation)
    - [Useful Translation Methods](#useful-translation-methods)
- [Methods](#methods)
    - [Get Translation](#get-translation)
    - [Get All Translations](#get-all-translations)
    - [Set Translation](#set-translation)
    - [Set Multiple Translations](#set-multiple-translations)
    - [Remove Translation](#remove-translation)
    - [Remove All Translations](#remove-all-translations)
- [Implementation Example](#implementation-example)

<p id="first-p">
You can make your eloquent models translatable. Translations are stored in a json column, so there's no extra table needed.
</p>

<a name="setup"></a>
## Setup

To make use of the translatable functionality you'll have to apply the `Varbox\Middleware\PersistLocale` middleware to your entire `web` route group.

> This middleware persists the locale from one request to another, if any was set manually inside the locale session name.
> For working with translations in the admin, the locale is set inside `Varbox\Controllers\LanguagesController@change`

Inside your `app/Http/Kernel.php` file, add the `Varbox\Middleware\PersistLocale` middleware to your entire `web` middleware group.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \Varbox\Middleware\PersistLocale::class
    ],

    ...
];
```

You can also enable auto locale detection based on your browser's preferred language from inside the `varbox/config/translation.php` config file.

<a name="usage"></a>
## Usage

No model that comes with the Varbox platform is translatable by default.    

If you want to make a Varbox model translatable, you should extend the original model and create a new migration in order to change you translatable columns type to json.

<a name="make-model-translatable"></a>
#### Make Model Translatable

Your models should use the `Varbox\Traits\HasTranslations` trait & the `Varbox\Options\TranslationOptions` class. 
The trait contains an abstract method `getTranslationOptions()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Options\TranslationOptions;
use Varbox\Traits\HasTranslations;

class YourModel extends Model
{
    use HasTranslations;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        ...
        'title',
        'content',
    ];

    /**
     * The attributes that should be casted to native types.
     *
     * @var array
     */
    protected $casts = [
        ...
        'title' => 'array',
        'content' => 'array',
    ];

    /**
     * Set the options for the HasTranslations trait.
     *
     * @return TranslationOptions
     */
    public function getTranslationOptions(): TranslationOptions
    {
        return TranslationOptions::instance()
            ->fieldsToTranslate('title', 'content');
    }
}
```

Setup a `migration` to support translatable columns by making them of `json` type in your table.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->json('title')->nullable();
    $table->json('content')->nullable();
});
```

<a name="save-translated-values"></a>
#### Save Translated Values

Now that you've enabled the translatable functionality on your models, it's time to learn how we can save your translatable fields in multiple languages.

Given that your columns supporting multiple languages are of type `json` and that the `HasTranslations` trait already casts the translatable columns to an `array`, you can save your translated values just by passing an array like this to the `create` or `update` methods.

```php
$item = YourModel::create([
    // not a translatable field
    'slug' => 'your-slug',

    // a translatable field
    'name' => [
        'en' => 'Your name',
        'ro' => 'Numele tau',
    ],

    // a translatable field containing multiple fields
    'data' => [
        'en' => [
            'title' => 'Your title',
            'content' => 'Your content',
        ],
        'ro' => [
            'title' => 'Titlul tau',
            'content' => 'Continutul tau',
        ]
    ]
]);
```

You're not required to specify all of the locales when creating, so the above can also be achieved like this:

```php
$item = YourModel::create([
    'slug' => 'your-slug',
    'name' => [
        // just for "en"
        'en' => 'Your name',
    ],
    'data' => [
        // just for "en"
        'en' => [
            'title' => 'Your title',
            'content' => 'Your content',
        ]
    ]
]);

$item->update([
    'name' => [
        // just for "ro"
        'ro' => 'Numele tau',
    ],
    'data' => [
        // just for "ro"
        'ro' => [
            'title' => 'Titlul tau',
            'content' => 'Continutul tau',
        ]
    ]
]);
```

<a name="display-translated-values"></a>
#### Display Translated Values

Now that you've learned how to actually save translated values for your model records, it's time to display them in your frontend.

> In order to actually persist your set locale, don't forget to [apply the middleware](#general-setup)

Displaying your translated values for a model record couldn't be easier!   
Just access the fields as you'd normally would and the `HasTranslations` trait will take care of the rest.

```php
// if the locale is "en", the value displayed would be "$model->title['en']"
// if the locale is "ro", the value displayed would be "$model->title['ro']"
{{ $model->title }}

// if the locale is "en", the value displayed would be "$model->content['summary']['en']"
// if the locale is "ro", the value displayed would be "$model->content['summary']['ro']"
{!! $model->content['summary'] !!}

// if the locale is "en", the value displayed would be "$model->content['description']['en']"
// if the locale is "ro", the value displayed would be "$model->content['description']['ro']"
{!! $model->content['description'] !!}
```

<a name="useful-methods"></a>
## Useful Methods

If you want to manually leverage the `Varbox\Traits\HasTranslations` trait's capabilities, read on.

<a name="get-translation"></a>
#### Get Translation

You can get a model record's translation by using the `getTranslation()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$title = $model->getTranslation('title', 'en');
```

<a name="get-all-translations"></a>
#### Get All Translations

You can get all translations for a model record by using the `getTranslation()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$titles = $model->getTranslations('title');
```

<a name="set-translation"></a>
#### Set Translation

You can set a translation for a model record by using the `setTranslation()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$model->setTranslation('title', 'Your title', 'en')->save();
```

<a name="set-multiple-translations"></a>
#### Set Multiple Translations

You can set multiple translations for a model record by using the `setTranslations()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$model->setTranslations('title', [
    'en' => 'Your title',
    'ro' => 'Titlul tau',
])->save();
```

<a name="remove-translation"></a>
#### Remove Translation

You can remove a field translation for a model record by using the `forgetTranslation()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$model->forgetTranslation('title', 'en')->save();
```

<a name="remove-all-translation"></a>
#### Remove All Translations

You can remove al fields' translations for specific locale by using the `forgetTranslations()`method present on the `Varbox\Traits\HasTranslations` trait.

```php
$model->forgetTranslations('en')->save();
```

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#translatable-models) page.
