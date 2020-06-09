# Multi Language

- [Admin Interface](#admin-interface)
- [General Setup](#general-setup)
- [Translatable Models](#translatable-models)
    - [Make Model Translatable](#make-model-translatable)
    - [Save Translated Values](#save-translated-values)
    - [Display Translated Values](#display-translated-values)
    - [Admin Implementation](#admin-implementation)
    - [Useful Translation Methods](#useful-translation-methods)
- [Static Translations](#static-translations)
    - [How Static Translations Work](#how-static-translations-work)
    - [Modify Static Translations](#modify-static-translations)
    - [The Translation Model](#the-translation-model)
- [Languages](#languages)
    - [The Active Languages](#the-active-languages)
    - [The Default Language](#the-default-language)
    - [The Language Model](#the-language-model)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The multi language module is a powerful tool that offers you the following features out of the box:
- manage your static translations by importing & exporting them
- manage your active & default languages for your application
- manage multi language support in your admin for your entities
- display language based content for your entities

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your translations and languages.

<a name="translations-interface"></a>
#### Translations Interface

You can find the translations section inside **Admin -> Multi Language -> Translations**.   
Feel free to explore all available options this section offers.

![Translations List](/docs/{{version}}/translations-list.png)

<a name="languages-interface"></a>
#### Languages Interface

You can find the languages section inside **Admin -> Multi Language -> Languages**.   
Feel free to explore all available options this section offers.

![Languages List](/docs/{{version}}/languages-list.png)

<a name="general-setup"></a>
## General Setup

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

<a name="translatable-models"></a>
## Translatable Models

You can make your eloquent models translatable.  
Translations are stored in a json column, so there's no extra table needed.

> No model that comes with the Varbox platform is translatable by default.    
> If you want to make a Varbox model translatable, you should extend the original model and create a new migration in order to change you translatable columns type to json.

<a name="make-model-translatable"></a>
### Make Model Translatable

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

<a name="translatable-database-columns"></a>
#### Translatable Database Columns

> If you plan on saving translated values for fields already stored in a "json" column, like how [Content Pages](/docs/{{version}}/content-management#content-pages) use the "data" field for storing the title, subtitle and content, then skip this step.

Setup a `migration` to support translatable columns by making them of `json` type in your table.

```php
Schema::table('your_table', function (Blueprint $table) {
    $table->json('title')->nullable();
    $table->json('content')->nullable();
});
```

<a name="save-translated-values"></a>
### Save Translated Values

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
### Display Translated Values

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

<a name="admin-implementation"></a>
### Admin Implementation

Now that you've enabled the translatable functionality on your models, it's time to learn how we can save your translatable fields in multiple languages.

> We're going to assume you already have an [admin crud](/docs/{{version}}/admin-crud) in place for your entity and you've already [applied the trait](#make-model-translatable) on your model.

<a name="the-is-translatable-middleware"></a>
#### The Is Translatable Middleware

> Applying this middleware, will instruct the platform that this section is translatable and therefore a [language dropdown](#active-languages) will appear in the top right header section, as well as constraining you to only create records in the [default language](#default-language)

Apply the `varbox.is.translatable` middleware on your admin crud routes for your translatable model.

```php
Route::group([
    'middleware' => [
        // your other middleware
        'varbox.is.translatable',
    ]
], function () {
    // your routes here
});
```

<a name="the-form-admin-lang-helper"></a>
#### The Form Admin Lang Helper

> Very imporant here is to also use `form_admin_lang()` for your form opening, even though not all fields inside the form are translatable. This is to be able to show existing field values on edit.

To instruct your translatable form fields to support multi language format, use the `form_admin_lang()` helper method instead of the `form_admin()` one.

```php
// open the form to support translatable fields
@if($item->exists)
    {!! form_admin_lang()->model($item, ['url' => $url, 'method' => 'PUT']) !!}
@else
    {!! form_admin_lang()->open(['url' => $url, 'method' => 'POST']) !!}
@endif

// normal form fields
{!! form_admin()->text('name', 'Name') !!}
{!! form_admin()->text('slug', 'Slug') !!}

// translatable form fields
{!! form_admin_lang()->text('title', 'Title') !!}
{!! form_admin_lang()->textarea('content[summary]', 'Summary') !!}
{!! form_admin_lang()->editor('content[description]', 'Content') !!}

// close the form
{!! form_admin()->close() !!}
```

**That's it!** Now you can manage your dynamic translations for your records directly from the admin.

<a name="useful-translation-methods"></a>
### Useful Translation Methods

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

<a name="static-translations"></a>
## Static Translations

The translations component is a very powerful multi language functionality that allows you to quickly make your models translatable, but also manipulate your static translations.

In order to provide you with a complex crud functionality inside the admin, the translations crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 4; -moz-column-count: 4; -webkit-column-count: 4;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="how-static-translations-work"></a>
### How Static Translations Work

First of all, let's understand the architecture behind static & dynamic translations and their workflow.

<a name="the-translations-workflow"></a>
#### The Workflow

- Assuming you have static translations inside the `resources/lang` directory
- Import all translations from "Admin -> Multi Language -> Translations"
- Edit the translations for which wish to update their values and then save
- Export the translations from "Admin -> Multi Language -> Translations" 
- Now your translations inside the `resources/lang` directory contain the updated values
- Keep using your static translations as you'd normally would in a Laravel application

<a name="modify-static-translations"></a>
### Modify Static Translations

From inside the "Admin -> Multi Language -> Translations" you have the possibility to dynamically modify your static translations' values. 
This is achievable, by first `importing` your static translations, modifying them from the admin and then `exporting` them. 

At the end of this 3-step procedure, your static translations inside the `resources/lang` directory, will have the updated values, so you can use them as you'd normally would in a Laravel app.

```php
{{ trans('messages.welcome') }}
```

<a name="the-translation-model"></a>
### The Translation Model

The `Varbox\Models\Translation` model is used behind the scenes to manage your static translations.   

You can get all your imported translations that have a value assigned by using the `withValue` query scope present on the `Translation` model.

```php
use Varbox\Models\Translation;

$translations = Translation::withValue()->get();
```   

You can get all your imported translations that don't have a value assigned by using the `withoutValue` query scope present on the `Translation` model.

```php
use Varbox\Models\Translation;

$translations = Translation::withoutValue()->get();
``` 

You can get all your imported translations that have a group allocated by using the `withGroup` query scope present on the `Translation` model.

```php
use Varbox\Models\Translation;

$translations = Translation::withGroup()->get();
```   

You can get all your imported translations that don't have a group allocated by using the `withoutGroup` query scope present on the `Translation` model.

```php
use Varbox\Models\Translation;

$translations = Translation::withoutGroup()->get();
```

<a name="languages"></a>
## Languages

The Varbox platform also puts at your disposal the "Admin -> Multi Language -> Languages" section from where you can manage the languages for your application.

> As a bonus, 135 languages are already seeded for you when installing the platform.

In order to provide you with a complex crud functionality inside the admin, the languages crud implements the following out of the box:

<style>
    #available-filter-operators-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    #available-filter-operators-list a {
        display: block;
        color: #4AAEE3;
    }
</style>
<div id="available-filter-operators-list" markdown="1">

[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)
[Csv Exports](/docs/{{version}}/csv-exports)

</div>

<a name="the-active-languages"></a>
### The Active Languages

Even though you have 135 languages, the chances you're going to be using all of them are very slim.   
Because of that, only active languages will show in the language switcher from the header.

You can apply the same logic in your frontend to use the multi language functionality only for the languages your application supports.

<a name="the-default-language"></a>
### The Default Language

Beside active languages support, we also offer the ability to set a default language from the admin, which by default is set to English.
You can use this default language to fallback to it in your edge cases.

It can only be a single default language, so saving another language as default, will cause the old one to loose its default status. 

<a name="the-language-model"></a>
### The Language Model

The `Varbox\Models\Language` model is used behind the scenes to manage your languages.   

You can get all languages sorted in alphabetical order by using the `alphabetically` query scope present on the `Translation` model.

```php
use Varbox\Models\Language;

$languages = Language::alphabetically()->get();
```   

You can get only the default language by using the `onlyDefault` query scope present on the `Translation` model.

```php
use Varbox\Models\Language;

$language = Language::onlydefault()->first();
```    

You can get all languages except the default on by using the `excludingDefault` query scope present on the `Translation` model.

```php
use Varbox\Models\Language;

$languages = Language::excludingDefault()->get();
```   

You can get all active languages by using the `onlyActive` query scope present on the `Translation` model.

```php
use Varbox\Models\Language;

$languages = Language::onlyActive()->get();
```

You can get all inactive languages by using the `onlyInactive` query scope present on the `Translation` model.

```php
use Varbox\Models\Language;

$languages = Language::onlyInactive()->get();
```

<a name="configuration"></a>
## Configuration

For more information on how you can customize the multi language components, please read the comments from their configuration files.

<a name="pages-configuration"></a>
#### Translations Configuration

The translations configuration file is located at `config/varbox/translation.php`.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
Varbox makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

<style>
    p.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 15px;
        margin: 0;
    }
</style>

<a name="translation-bindings"></a>
### Translation Bindings

The translation classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Services\TranslationService</p>

Found in `config/varbox/bindings.php` at `services.translation_service` key.   
This class is responsible for importing and exporting static translations.

<p class="overwrite-class">Varbox\Models\Translation</p>

Found in `config/varbox/bindings.php` at `models.translation_model` key.   
This class represents the translation model.

<p class="overwrite-class">Varbox\Controllers\TranslationsController</p>

Found in `config/varbox/bindings.php` at `controllers.translations_controller` key.   
This class is used for interactions with the "Admin -> Multi Language -> Translations".

<p class="overwrite-class">Varbox\Requests\TranslationRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.translation_form_request` key.   
This class is used for validating any translation when updating.

<p class="overwrite-class">Varbox\Filters\TranslationFilter</p>

Found in `config/varbox/bindings.php` at `filters.translation_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\TranslationSort</p>

Found in `config/varbox/bindings.php` at `sorts.translation_sort` key.   
This class is used for applying the sorting logic.

<a name="language-bindings"></a>
### Language Bindings

The language classes available for binding overwrites are:

<p class="overwrite-class">Varbox\Models\Language</p>

Found in `config/varbox/bindings.php` at `models.language_model` key.   
This class represents the language model.

<p class="overwrite-class">Varbox\Controllers\LanguagesController</p>

Found in `config/varbox/bindings.php` at `controllers.languages_controller` key.   
This class is used for interactions with the "Admin -> Multi Language -> Languages".

<p class="overwrite-class">Varbox\Requests\LanguageRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.language_form_request` key.   
This class is used for validating any translation when creating or updating.

<p class="overwrite-class">Varbox\Composers\LanguagesComposer</p>

Found in `config/varbox/bindings.php` at `view_composers.languages_view_composer` key.   
The view composer for the language switcher in the header.

<p class="overwrite-class">Varbox\Filters\LanguageFilter</p>

Found in `config/varbox/bindings.php` at `filters.language_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\LanguageSort</p>

Found in `config/varbox/bindings.php` at `sorts.language_sort` key.   
This class is used for applying the sorting logic.
