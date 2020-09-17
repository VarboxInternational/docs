<h1>Admin Menu</h1>

<br />

<p id="first-p">
If you've successfully followed the installation guide, you should already have this setup.
</p>

> If not, please make sure you've followed the installation guide properly.

You should have a file called `app/Http/Composers/AdminMenuComposer.php` present in your application. 
That file should have already been bound inside your `config/varbox/bindings.php` file, specifically for the `admin_menu_view_composer` key.

<hr >

#### Add New Items To The Admin Menu

To add your own menu items inside the admin, open `app/Http/Composers/AdminMenuComposer.php` and add the code for your new menu items. 

The already existing code is self-explanatory, so you shouldn't have any trouble adding your own menu items, but just in case, here's an example of adding a "Shop Panel" with "Orders" and "Products" as submenu items.

```php
$menu->add(function ($item) use ($menu) {
    $shop = $item->name('Shop Panel')->data('icon', 'fa-cart')
        ->permissions('orders-list', 'products-list')
        ->active('admin/orders/*', 'admin/products/*');

    $menu->child($shop, function (MenuItem $item) {
        $item->name('Orders')
            ->url(route('admin.orders.index'))
            ->permissions('orders-list')
            ->active('admin/orders/*');
    });

    $menu->child($shop, function (MenuItem $item) {
        $item->name('Products')
            ->url(route('admin.products.index'))
            ->permissions('products-list')
            ->active('admin/products/*');
    });
});
```
