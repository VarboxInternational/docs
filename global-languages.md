<h1>Global Languages <small class="paid">PAID</small></h1>

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [Active Languages](#active-languages)
    - [Default Language](#default-language)
- [The Language Model](#the-language-model)
- [Overwrite Bindings](#overwrite-bindings)

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

<a name="admin-interface"></a>
## Admin Interface

Before going deeper, you should know that there's already a section in the admin from where you can manage all your translations and languages.

You can find the languages section inside **Admin -> Multi Language -> Languages**.   
Feel free to explore all available options this section offers.

![Languages List](/docs/{{version}}/languages-list.png)

<a name="how-it-works"></a>
## How it Works

First of all, let's understand the architecture behind languages and their workflow.

<a name="active-languages"></a>
#### Active Languages

Even though you have 135 languages, the chances you're going to be using all of them are very slim.   
Because of that, only active languages will show in the language switcher from the header.

You can apply the same logic in your frontend to use the multi language functionality only for the languages your application supports.

<a name="default-language"></a>
#### Default Language

Beside active languages support, we also offer the ability to set a default language from the admin, which by default is set to English.
You can use this default language to fallback to it in your edge cases.

It can only be a single default language, so saving another language as default, will cause the old one to loose its default status. 

<a name="the-language-model"></a>
## The Language Model

The `Varbox\Models\Language` model is used to manage your languages.   

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
