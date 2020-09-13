<h1>Csv Exports <small class="free">FREE</small></h1>

- [Usage](#usage)
    - [Set Heading Columns](#set-heading-columns)
    - [Define Row Values](#define-row-values)
    - [Export Csv File](#export-csv-file)
- [Implementation Example](#implementation-example)

<p id="first-p">
This functionality allows you to download csv files for a given collection of model records.
</p>

<a name="usage"></a>
## Usage

Your models should use the `Varbox\Traits\IsCsvExportable` trait. 
The trait contains two abstract methods which you'll have to implement yourself:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\IsCsvExportable;

class YourModel extends Model
{
    use IsCsvExportable;

    /**
     * Get the heading columns for the csv.
     *
     * @return array
     */
    public function getCsvColumns()
    {
        return [
            'Name', 'Url', 'Active', 'Created At', 'Last Modified At'
        ];
    }

    /**
     * Get the values for a row in the csv.
     *
     * @return array
     */
    public function toCsvArray()
    {
        return [
            $this->name,
            $this->getUrl(),
            $this->isActive() ? 'Yes' : 'No',
            $this->created_at->format('Y-m-d H:i:s'),
            $this->updated_at->format('Y-m-d H:i:s'),
        ];
    }

    ...
}
```

<a name="set-heading-columns"></a>
#### Set Heading Columns

You can set the heading columns for the csv by implementing the `getCsvColumns()` method.

```php
/**
 * Get the heading columns for the csv.
 *
 * @return array
 */
public function getCsvColumns()
{
    return [
        'Name', 'Url', 'Active', 'Created At', 'Last Modified At'
    ];
}
```

<a name="define-row-values"></a>
#### Define Row Values

You can define what values each exported row will have by using the `toCsvArray()` method.

Please note that inside this method, `$this` actually represents the loaded model instance, so you can call methods, relations, etc. and parse them as you'd  like to show in the exported csv file.

```php
/**
 * Get the values for a row in the csv.
 *
 * @return array
 */
public function toCsvArray()
{
    return [
        $this->name,
        $this->getUrl(),
        $this->isActive() ? 'Yes' : 'No',
        $this->created_at->format('Y-m-d H:i:s'),
        $this->updated_at->format('Y-m-d H:i:s'),
    ];
}
```

<a name="export-csv-file"></a>
#### Export Csv File

After you've implemented the trait on your models, you can download a csv file using the `exportToCsv()` method, inside your controller for example.

```php
$items = YourModel::where('active', true)->orderBy('name')->get();

return app(YourModel::class)->exportToCsv($items);
```

<a name="implementation-example"></a>
## Implementation Example

For an implementation example of this functionality please refer to the [Full Example](/docs/{{version}}/full-example#csv-exports) page.
