<h1>Query Cache <small class="free">FREE</small></h1>

- [Usage](#usage)
    - [Cache All Queries](#cache-all-queries)
    - [Cache Duplicate Queries](#cache-duplicate-queries)
    - [Disable Temporarily](#disable-temporarily)
- [Configuration](#cofiguration)

<p id="first-p">
This functionality allows you to cache all read queries or only just the duplicated ones for a model.
</p>    
   
<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsCacheable` trait.   

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsCacheable;

class YourModel extends Model
{
    use IsCacheable;
}
```

Next, if you've installed Varbox correctly, you should already see the following in your `.env` file. 
If not, please add them.

```
CACHE_ALL_QUERIES=false
CACHE_DUPLICATE_QUERIES=false
```

<a name="cache-all-queries"></a>
#### Cache All Queries

It's possible to cache all read queries for a model. The queries will be cached using <a href="https://laravel.com/docs/7.x/cache#cache-tags" target="_blank">cache tagging</a>.

> When creating, updating or deleting a record for a model, all queries for that entire model will be flushed from cache.

In your `.env` file change `CACHE_DRIVER` to a value that's persistent and also supports tagging.   

```
CACHE_DRIVER=redis
``` 

In your `.env` file change the `CACHE_ALL_QUERIES` environment variable to `true`.

```
CACHE_ALL_QUERIES=true
```

<a name="cache-duplicate-queries"></a>
#### Cache Duplicate Queries

If you don't want to cache all queries for your models forever, you can opt in for caching only duplicate queries. The queries will be cached only for the current request using <a href="https://laravel.com/docs/7.x/cache#cache-tags" target="_blank">cache tagging</a>.

> If you already cache all your queries, it's redundant to enable the duplicate query cache.

In your `.env` file change `CACHE_DRIVER` to a value that supports tagging.   

```
CACHE_DRIVER=array
```

In your `.env` file change the `CACHE_DUPLICATE_QUERIES` environment variable to `true`.

```
CACHE_DUPLICATE_QUERIES=true
```

<a name="disable-temporarily"></a>
#### Disable Temporarily

In some edge cases you may want to temporarily disable the query caching functionality for good.   
You can do this using the `disableQueryCache()` method.

```php
app(YourModel::class)->disableQueryCache();

// or for an existing model instance

$model = YourModel::find($id);
$model->disableQueryCache();
```

You can also enable it back further down the road in the same request by using the `enableQueryCache()` method.

```php
app(YourModel::class)->enableQueryCache();

// or for an existing model instance

$model = YourModel::find($id);
$model->enableQueryCache();
```

<a name="configuration"></a>
## Configuration

The query cache configuration file is located at `config/varbox/query-cache.php`.

For more information on how you can customize the query caching functionality, please read the comments from that file.
