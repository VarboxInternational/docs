# Admin Menu

If you've successfully followed the [Installation Guide](/docs/{{version}}/installation), you should already have this setup.

> If not, please make sure you've followed the installation guide properly.

You should have a file called `app/Http/Composers/AdminMenuComposer.php` present in your application.   
That file should have already been bound inside your `config/varbox/bindings.php` file, specifically for the `admin_menu_view_composer` key.

<hr >

#### Add New Items To The Admin Menu

To add your own menu items inside the admin, open the `app/Http/Composers/AdminMenuComposer.php` file and add the code for your new menu items. The already existing code is self-explanator, so you shouldn't have any trouble adding your own menu items.
