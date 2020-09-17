<h1>Admin Forms</h1>

<br />

<p id="first-p">
The Varbox platform offers an easy solution for building form elements inside the admin panel.
</p>

> For building admin form elements, the form builder from <a href="https://packagist.org/packages/laravelcollective/html" target="_blank">laravelcollective/html</a> is used as the foundation, so make sure you read their documentation first.

<a name="build-admin-forms"></a>
#### Build Admin Forms

To build admin form elements, you can use the `form_admin()` helper function which simply acts as a wrapper over the `Collective\Html\FormBuilder` class.

> For all of the available form elements you can create, please take a look inside the `Varbox\Helpers\AdminFormHelper` class.

<a name="build-admin-forms"></a>
#### Build Multi Language Admin Forms

To build multi language admin form elements, you can use the `form_admin_lang()` helper function which simply acts as a wrapper over the `Collective\Html\FormBuilder` class.

This helper takes care of injecting your locale, thus offering you the possibility of creating or updating an element in a specific language.

> To see how the locale is automatically leveraged into your forms, please take a look inside the `Varbox\Helpers\AdminFormLangHelper` class.
