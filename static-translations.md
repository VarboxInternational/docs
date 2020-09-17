<h1>Static Translations</h1>

- [Admin Interface](#admin-interface)
- [Initial Setup](#initial-setup)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Modify Static Translations](#modify-static-translations)
- [The Translation Model](#the-translation-model)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

<p id="first-p">
The translations component is a very powerful multi language functionality that allows you to quickly make your models translatable, but also manipulate your static translations.
</p>

In order to provide you with a complex crud functionality inside the admin, the translations crud implements the following out of the box:

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

</div>

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your translations and languages.

You can find the translations section inside **Admin -> Multi Language -> Translations**.   
Feel free to explore all available options this section offers.

![Translations List](/docs/{{version}}/translations-list.png)

<a name="initial-setup"></a>
## Initial Setup

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

<a name="how-it-works"></a>
## How It Works

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
#### Modify Static Translations

From inside the "Admin -> Multi Language -> Translations" you have the possibility to dynamically modify your static translations' values. 
This is achievable, by first `importing` your static translations, modifying them from the admin and then `exporting` them. 

At the end of this 3-step procedure, your static translations inside the `resources/lang` directory, will have the updated values, so you can use them as you'd normally would in a Laravel app.

```php
{{ trans('messages.welcome') }}
```

<a name="the-translation-model"></a>
## The Translation Model

The `Varbox\Models\Translation` model is used to manage your static translations.   

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

<a name="configuration"></a>
## Configuration

The translations configuration file is located at `config/varbox/translation.php`.

For more information on how you can customize the multi language components, please read the comments from their configuration files.

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
