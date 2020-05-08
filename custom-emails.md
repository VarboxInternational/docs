# Custom Emails

- [Admin Interface](#admin-interface)
- [How It Works](#how-it-works)
    - [The Workflow](#the-workflow)
    - [Configuration File](#configuration-file)
    - [Mailable Files](#mailable-files)
    - [Custom Variables](#custom-variables)
- [Usage](#usage)
    - [Create New Mailable](#create-new-mailable)
    - [Use Custom Variables](#use-custom-variables)
    - [Send A Custom Email](#send-a-custom-email)
    - [Customize The Design](#customize-the-design)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)

The `emails` component is a very powerful content management functionality that allows you to quickly customize a mailable with custom details and variable oriented values.

In order to provide you with a complex crud functionality inside the admin, the emails crud implements the following out of the box:

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

[Upload Files](/docs/{{version}}/upload-files)
[Model Revisions](/docs/{{version}}/model-revisions)
[Draft Records](/docs/{{version}}/draft-records)
[Duplicate Records](/docs/{{version}}/duplicate-records)
[Filter Records](/docs/{{version}}/filter-records)
[Sort Records](/docs/{{version}}/sort-records)
[Query Cache](/docs/{{version}}/query-cache)
[Activity Log](/docs/{{version}}/activity-log)

</div>

<a name="admin-interface"></a>
## Admin Interface

Before you learn about using your emails inside a [VarBox](/) application, you should know that there's already a section in the admin from where you can manage all your emails.

You can find the emails section inside [Admin -> Manage Content -> Emails](/docs/{{version}}/emails-interface).   
Feel free to explore all available options this section offers.

![Emails List](/docs/{{version}}/emails-list.png)

<a name="how-it-works"></a>
## How It Works

First of all, let's understand the architecture behind content blocks and their workflow.

<a name="the-workflow"></a>
#### The Workflow

- Create the email type inside `config/varbox/emails.php` config file
- Generate the mailable files by running the `varbox:make-mail` artisan command
- Create an email from "Admin -> Manage Content -> Emails"
- Send the email

<a name="configuration-file"></a>
#### Configuration File

The emails component comes bundled with a `config/varbox/emails.php` config file.

From here you can specify what email types your application contains and what custom variables your emails can make use of.

> You should specify a new email type for each mailable your application will use. For each email type, you can create only one email record inside the admin.
> <br /><br />
> You should keep the variables section up to date with all the variables you're using inside all your mailable classes.

<a name="mailable-files"></a>
#### Mailable Files

Running the artisan command for generating custom emails, will generate all necessary files for a mailable.The generated files for each email type are:

<style>
    span.file-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 13px;
        color: #239cdd;
    }
</style>

<span class="file-class">app/Mail/{YourMailable}.php</span>

This is the custom mailable class. This class extends the  `Varbox\Mail\Mailable` abstract class, which in turn extends the `Illuminate\Mail\Mailable` class, so you have available any functionality a basic Laravel mailable offers.

With that said, the actual generated mailable class only exposes 3 methods, in order to make your job of creating a mailable easier.

<span class="file-class">resources/views/emails/{your_mailable}.blade.php</span>  

This is the mailable view that will be used to actual render the sent email. The custom emails use the markdown template. 

In the majority of the cases, modifying this file won't be necessary, since you can dynamically change your email's content from the admin panel.

<a name="custom-variables"></a>
#### Custom Variables

Beside the fact that you can have emails with a customizable subject, content or even an attachment, you can even leverage custom variables to display, for example, a user's name in your email's content.

You should define your custom variables inside the `config/varbox/emails.php` config file and then, inside the `mapVariables()` method from your mailable class you should actually provide a value for your variables.

> To specify custom variables for your emails' content inside the admin, wrap your variable name in square brackets: **[username]**

<a name="usage"></a>
## Usage

Now that you've grasped the concept behind the emails component, it's time to put it to use. 

<a name="create-new-mailable"></a>
#### Create New Mailable

First, you need to define the `type` of the email inside the `config/varbox/emails.php` config file.

```php
'types' => [

    'your-mail' => [

        // The mailable class used for sending the email.
        // This gets generated automatically when running 
        // "php artisan varbox:make-mail"
        'class' => 'App\Mail\YourMail',

        // The blade file used for rendering the email.
        // The value will be relative to the "resources/views/" directory.
        'view' => 'emails.your_mail',

        // Array of variables that the respective mail type is allowed to use.
        // You can customize these variables's values inside your mailable class.
        'variables' => [
            'your_variable_one', 'your_variable_two'
        ],
    ],

],
```

After you've defined your email type, you can generate the mailable files by running the below command. Please note that the command accepts a single argument, which is your email type.

```
php artisan varbox:make-mail your-mail
```

That's it! Now you have two files for your newly created email:
- `app/Mail/YourMail.php`: which is the mailable class
- `resources/views/emails/your_mail.blade.php`: which is the mailable view

<a name="use-custom-variables"></a>
#### Use Custom Variables

Inside your generated mailable class you'll see a method called `mapVariables()` that by default returns an array of your custom variables that you've setup in your `config/varbox/emails.php` for that type.

To specify a dynamic value for your custom variables, simply modify that array with the actual values for each of the variables.

```php
/**
 * Return an array containing:
 * - keys: each variable name as defined inside the "config/varbox/emails.php" -> "variables" config option
 * - values: the actual value that the respective variable (key) should be transformed to
 *
 * @return array
 */
protected function mapVariables()
{
    return [
        'username' => $this->user->name,
        'company' => config('app.name'),

   ];
}
```

After you've defined the dynamic values for your custom variables, you can reference them from inside the admin, when creating or updating an email like so:

```
Hi [username]

Welcome to our site!

Regards, [company] team
```

<a name="send-a-custom-email"></a>
#### Send A Custom Email

Given that these custom emails the [VarBox](/) platform provides finally extend the Laravel's `Illuminate\Mail\Mailable` class, you can send these emails like you always did inside a Laravel application.

```php
use App\Mail\YourMail;

Mail::to($request->user())->send(new YourMail);
```

<a name="customize-the-design"></a>
#### Customize The Design

In order to change the design of your emails, please follow this Laravel [documentation section](https://laravel.com/docs/7.x/mail#customizing-the-components)

<a name="configuration"></a>
## Configuration

The emails configuration file is located at `config/varbox/emails.php`.

For more information on how you can customize the emails functionality, please read the comments from that file.

<a name="overwrite-bindings"></a>
## Overwrite Bindings

In your projects, you may stumble upon the need to modify the behavior of these classes, in order to fit your needs.
[VarBox](/) makes this possible via the `config/varbox/bindings.php` configuration file. In that file, you'll find every customizable class the platform uses.

> For more information on how the class binding works, please refer to the [Custom Bindings](/docs/{{version}}/custom-bindings) documentation section.

The `email` classes available for binding overwrites are:

<style>
    span.overwrite-class {
        display: block;
        font-family: SFMono-Regular,Menlo,Monaco,Consolas,Liberation Mono,Courier New,monospace;
        font-weight: 600;
        font-size: 14px;
    }
</style>

<span class="overwrite-class">Varbox\Models\Email</span>

Found in `config/varbox/bindings.php` at `models.email_model` key.   
This class represents the email model.

<span class="overwrite-class">Varbox\Controllers\EmailsController</span>

Found in `config/varbox/bindings.php` at `controllers.emails_controller` key.   
This class is used for interactions with the Admin -> Manage Content -> Emails section.

<span class="overwrite-class">Varbox\Requests\EmailRequest</span>

Found in `config/varbox/bindings.php` at `form_requests.email_form_request` key.   
This class is used for validating any email when creating or updating.
