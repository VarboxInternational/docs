# File Uploads

- [Prerequisites](#prerequisites)
    - [Setup Storge Disk](#setup-storage-disk)
    - [Run Artisan Command](#run-artisan-command)
- [Admin Interface](#admin-interface)
- [Upload Images](#upload-images)
    - [Image Max Size](#image-max-size)
    - [Allowed Image Extensions](#allowed-image-extensions)
    - [Generate Image Thumbnail](#generate-image-thumbnail)
    - [Generate Extra Image Variants](#generate-extra-image-variants)
    - [Save Image To Database](#save-image-to-database)
- [Retrieve Images](#retrieve-images)
    - [Display Original Image](#display-original-image)
    - [Display Image Thumbnail](#display-image-thumbnail)
    - [Display Image Variant](#display-image-variant)
    - [Download Original Image](#download-original-image)
    - [Retrieve Image Path](#retrieve-image-path)
    - [Check If Image Exists](#check-if-image-exists)
- [Upload Videos](#upload-videos)
    - [Video Max Size](#video-max-size)
    - [Allowed Video Extensions](#allowed-video-extensions)
    - [Save Video To Database](#save-video-to-database)
- [Retrieve Videos](#retrieve-videos)
    - [Display Original Video](#display-original-video)
    - [Download Original Video](#download-original-video)
    - [Retrieve Video Path](#retrieve-video-path)
    - [Check If Video Exists](#check-if-video-exists)
- [Upload Audio](#upload-audio)
    - [Audio Max Size](#audio-max-size)
    - [Allowed Audio Extensions](#allowed-audio-extensions)
    - [Save Audio To Database](#save-audio-to-database)
- [Retrieve Audio](#retrieve-audio)
    - [Display Original Audio](#display-original-audio)
    - [Download Original Audio](#download-original-audio)
    - [Retrieve Audio Path](#retrieve-audio-path)
    - [Check If Audio Exists](#check-if-audio-exists)
- [Upload Files](#upload-files)
    - [File Max Size](#file-max-size)
    - [Allowed File Extensions](#allowed-file-extensions)
    - [Save File To Database](#save-file-to-database)
- [Retrieve Files](#retrieve-files)
    - [Display Original File](#display-original-file)
    - [Download Original File](#download-original-file)
    - [Retrieve File Path](#retrieve-file-path)
    - [Check If File Exists](#check-if-file-exists)
- [Model Usage](#model-usage)
    - [Specific Model Configurations](#specific-model-configurations)
    - [Admin File Management](#admin-file-management)
    - [Upload Model Methods](#upload-model-methods)
- [Configuration](#configuration)
- [Overwrite Bindings](#overwrite-bindings)
    
    
This functionality allows you to save files on a disk. 
It supports uploading image, video, audio and normal files.

<a name="prerequisites"></a>
## Prerequisites

If you've followed the [Installation Guide](/docs/{{version}}/installation), all necessary setup for your uploads should already be done. 
If for some reason you're not already setup, here's what you need to do.

<a name="setup-storage-disk"></a>
#### Setup Storage Disk

Setup the `uploads` storage disk inside your `config/filesystems.php` file under `disks` section.

```
'disks' => [
    ...
    
    'uploads' => [
        'driver' => 'local',
        'root' => storage_path('uploads'),
        'url' => env('APP_URL').'/uploads',
        'visibility' => 'public',
    ],
],
```

Next, in your `storage` directory create a new folder called `uploads` and inside it create a new file called `.gitignore` containing the following:

```
!.gitignore
```

<a name="run-artisan-command"></a>
#### Run Artisan Command

Finally, you'll need to create a symbolic link from `public/uploads` to `storage/uploads`

```
php artisan varbox:uploads-link
```

<a name="admin-interface"></a>
## Admin Interface

Before you learn about managing your files inside a Varbox application, you should know that there's already a section in the admin from where you can manage all your files.

You can find the uploads section inside **Admin -> Media Library -> Uploads**.   
Feel free to explore all available options this section offers.

![Uploads List](/docs/{{version}}/uploads-list.png)

<a name="upload-images"></a>
## Upload Images

To upload images you can use the `upload()` method present on the `Varbox\Services\UploadService` class.   

```php
use Varbox\Services\UploadService;

// by using the UploadedFile object
$image = (new UploadService($request->file('image')))->upload();

// by providing a public url
$image = (new UploadService('https://your-image-url.jpg'))->upload();

// by using the $_FILES global variable
$image = (new UploadService($_FILES['image']))->upload();
    
return $image->getPath() . '/' . $image->getName();
```

<a name="image-max-size"></a>
#### Image Max Size

When uploading an image, the `UploadService` will automatically check if the image is smaller than the maximum size allowed. 
If the image is bigger, a `UploadException::maxSizeExceeded()` error will be thrown.

To modify the maximum size allowed for images, change the `images -> max_size` value inside the `config/varbox/upload.php` config file.

```php
'images' => [

    /*
    |
    | The maximum size allowed for uploaded image files in MB.
    |
    | If any integer value is specified, files larger than this defined size won't be uploaded.
    | To allow all image files of all sizes to be uploaded, specify the "null" value for this option.
    |
    */
    'max_size' => 5,

]
```

<a name="allowed-image-extensions"></a>
#### Allowed Image Extensions

When uploading an image, the `UploadService` will automatically check if the image extension is allowed. 
If the extension is not allowed, a `UploadException::extensionNotAllowed()` error will be thrown.

To modify the list of allowed extensions for images, change the `images -> allowed_extensions` value inside the `config/varbox/upload.php` config file.

```php
'images' => [

    /*
    |
    | The allowed extensions for image files.
    | All image extensions can be found in \Varbox\Services\UploadService::getImageExtensions().
    |
    | You can specify allowed extensions by using an array, or a comma "," separated string of extensions.
    | To allow uploading any image files, specify the "null" value for this option.
    |
    */
    'allowed_extensions' => [
        'jpeg', 'jpg', 'png', 'gif', 'bmp',
    ],

]
```

<a name="generate-image-thumbnail"></a>
#### Generate Image Thumbnail

When uploading an image, the `UploadService` will automatically generate a thumbnail for it. 

To enable or disable the thumbnail generation, change the `images -> generate_thumbnail` value inside the `config/varbox/upload.php` config file.

```php
'images' => [

    /*
    |
    | Flag that on image upload to generate one thumbnail as well (true | false).
    |
    | This generated thumbnail will interlace with the image's styles.
    | So be careful not to override it by defining an image style called "thumbnail".
    |
    */
    'generate_thumbnail' => true,

]
```

You can also specify the width and height of the generated thumbnail, by changing the `images -> thumbnail_style` value inside the `config/varbox/upload.php` config file.

```php
'images' => [

    /*
    |
    | The size at which the generated thumbnail will be saved.
    | Please note that the thumbnail will be automatically fitted, keeping the ratio of the original image.
    | Not specifying this option's width and height will force the generated thumbnail to resize itself to 100x100px.
    |
    */
    'thumbnail_style' => [
        'width' => 100,
        'height' => 100,
    ],

]
```

<a name="generate-extra-image-variants"></a>
#### Generate Extra Image Variants

When uploading an image, you can specify that you want extra variants for that image. 

You can do so by adding styles to the `images -> styles` value, inside the `config/varbox/upload.php` config file.

> The styles specified in the config file will apply to all uploaded images. Most likely, you'll want to generate different styles for different eloquent models that support images.   
> <br/>
> To achieve that, please take a look at the [Model Usage](#model-usage) section.

Let's say that besides the original and thumbnail, we also want to generate a "portrait" and a "landscape" version of the uploaded images.

```php
'images' => [

    /*
    |
    | The styles to create from the original uploaded image.
    | You can specify multiple styles, as an array.
    |
    | Specify the "ratio" = true individually on each style, to let the uploader know you want to preserve the original ratio.
    |
    | If ratio preserving is enabled, the image will first be re-sized and then cropped.
    | If ratio preserving is disabled, the image will only be re-sized at the width and height specified.
    |
    | Also, not specifying the ratio for a style, it will consider the ratio as enabled.
    | The only way to disable the ratio, is to set it to false.
    |
    */
    'styles' => [
        'portrait' => [
            'width' => '100',
            'height' => '300',
            'ratio' => true,
        ],
        'landscape' => [
            'width' => '300',
            'height' => '100',
            'ratio' => true,
        ],
    ],

]
```

<a name="save-image-to-database"></a>
#### Save Image To Database

When uploading an image, the `UploadService` will automatically create a database record for it inside the `uploads` database table. 

To enable or disable saving to the database, change the `database -> save` value inside the `config/varbox/upload.php` config file.

> Disabling this option will cause the "Admin -> Media Library -> Uploads" section to stop working, among other things.

```php
'database' => [

    /*
    |
    | Determine if the uploaded files' details will be saved to the database.
    |
    | This is encouraged, every uploaded file would have more details about it.
    | Also, if this is saved, applying the HasUploads trait on models, unlocks certain query scopes.
    |
    | To disable this, set the value to "false".
    |
    */
    'save' => true,

]
```

<a name="retrieve-images"></a>
## Retrieve Images

To display images you can use the `Varbox\Helpers\UploadedHelper` class.   

This class is also aliased through a helper function called `uploaded()` that accepts the full path of the image as its single argument.

<a name="display-original-image"></a>
#### Display Original Image

To display the original image, you can use the `url()` method present on the `UploadedHelper` class.

```php
<img src="{{ uploaded($fullPathToImage)->url() }}" />
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$image = Upload::find($id);

// display the image in your blade files
<img src="{{ $image->helper->url() }}" />
```

<a name="display-image-thumbnail"></a>
#### Display Image Thumbnail

As said [above](#generate-image-thumbnail), when uploading an image, a thumbnail for that image will be automatically created.

To display the image's thumbnails, you can use the `thumbnail()` method present on the `UploadedHelper` class.

```php
<img src="{{ uploaded($fullPathToImage)->thumbnail() }}" />
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$image = Upload::find($id);

// display the image in your blade files
<img src="{{ $image->helper->thumbnail() }}" />
```

<a name="display-image-variant"></a>
#### Display Image Variant

As said [above](#generate-extra-image-variants), when uploading an image, you have the possibility to generate extra variants for it.

To display one of the image's variants, you can use the `url()` method present on the `UploadedHelper` class, but pass the name of the `style` as the only parameter.

```php
<img src="{{ uploaded($fullPathToImage)->url('portrait') }}" />
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$image = Upload::find($id);

// display the image in your blade files
<img src="{{ $image->helper->url('portrait') }}" />
```

<a name="download-original-image"></a>
#### Download Original Image

To download an uploaded image, you can use the `download()` method present on the `UploadService` class.

```php
use Varbox\Services\UploadService;

return (new UploadService($fullPathToImage))->download();
```

<a name="retrieve-image-path"></a>
#### Retrieve Image Path

To get the image path, you can use the `path()` method present on the `UploadedHelper` class.

```php
// get the path to the original image
uploaded($fullPathToImage)->path();

// get the full path to the original image
uploaded($fullPathToImage)->path(null, true);

// get the path to one of the image's variants
uploaded($fullPathToImage)->path('portrait');

// get the full path to one of the image's variants
uploaded($fullPathToImage)->path('portrait', true);
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$image = Upload::find($id);

// get the path to the original image
$image->helper->path();

// get the full path to the original image
$image->helper->path(null, true);

// get the path to one of the image's variants
$image->helper->path('portrait');

// get the full path to one of the image's variants
$image->helper->path('portrait', true);
```

<a name="check-if-image-exists"></a>
#### Check If Image Exists

You can check if an uploaded image actually exists on the disk by using the `exists()` method present on the `UploadedHelper` class.

```php
uploaded($fullPathToImage)->exists();
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$image = Upload::find($id);

// check if the image exists
$image->helper->exists();
```

<a name="upload-videos"></a>
## Upload Videos

To upload videos you can use the `upload()` method present on the `Varbox\Services\UploadService` class.   

```php
use Varbox\Services\UploadService;

// by using the UploadedFile object
$video = (new UploadService($request->file('video')))->upload();

// by providing a public url
$video = (new UploadService('https://your-video-url.mp4'))->upload();

// by using the $_FILES global variable
$video = (new UploadService($_FILES['video']))->upload();
    
return $video->getPath() . '/' . $video->getName();
```

<a name="video-max-size"></a>
#### Video Max Size

When uploading a video, the `UploadService` will automatically check if the video is smaller than the maximum size allowed. 
If the video is bigger, a `UploadException::maxSizeExceeded()` error will be thrown.

To modify the maximum size allowed for videos, change the `videos -> max_size` value inside the `config/varbox/upload.php` config file.

```php
'videos' => [

    /*
    |
    | The maximum size allowed for uploaded video files in MB.
    |
    | If any integer value is specified, files larger than this defined size won't be uploaded.
    | To allow all video files of all sizes to be uploaded, specify the "null" value for this option.
    |
    */
    'max_size' => 10,

]
```

<a name="allowed-video-extensions"></a>
#### Allowed Video Extensions

When uploading a video, the `UploadService` will automatically check if the video extension is allowed. 
If the extension is not allowed, a `UploadException::extensionNotAllowed()` error will be thrown.

To modify the list of allowed extensions for videos, change the `videos -> allowed_extensions` value inside the `config/varbox/upload.php` config file.

```php
'videos' => [

    /*
    |
    | The allowed extensions for video files.
    | All video extensions can be found in \Varbox\Services\UploadService::getVideoExtensions().
    |
    | You can specify allowed extensions by using an array, or a comma "," separated string of extensions.
    | To allow uploading any video files, specify the "null" value for this option.
    |
    */
    'allowed_extensions' => [
        'mp4', 'flv', 'avi'
    ],

]
```

<a name="save-video-to-database"></a>
#### Save Video To Database

When uploading a video, the `UploadService` will automatically create a database record for it inside the `uploads` database table. 

To enable or disable saving to the database, change the `database -> save` value inside the `config/varbox/upload.php` config file.

> Disabling this option will cause the "Admin -> Media Library -> Uploads" section to stop working, among other things.

```php
'database' => [

    /*
    |
    | Determine if the uploaded files' details will be saved to the database.
    |
    | This is encouraged, every uploaded file would have more details about it.
    | Also, if this is saved, applying the HasUploads trait on models, unlocks certain query scopes.
    |
    | To disable this, set the value to "false".
    |
    */
    'save' => true,

]
```

<a name="retrieve-videos"></a>
## Retrieve Videos

To display videos you can use the `Varbox\Helpers\UploadedHelper` class.   

This class is also aliased through a helper function called `uploaded()` that accepts the full path of the video as its single argument.

<a name="display-original-video"></a>
#### Display Original Video

To display the original video, you can use the `url()` method present on the `UploadedHelper` class.

```php
<video controls>
    <source src="{{ uploaded($fullPathToVideo)->url() }}" type="video/mp4">
</video>
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$video = Upload::find($id);

// display the video in your blade files
<video controls>
    <source src="{{ $video->helper->url() }}" type="video/mp4">
</video>
```

<a name="download-original-video"></a>
#### Download Original Video

To download an uploaded video, you can use the `download()` method present on the `UploadService` class.

```php
use Varbox\Services\UploadService;

return (new UploadService($fullPathToVideo))->download();
```

<a name="retrieve-video-path"></a>
#### Retrieve Video Path

To get the video path, you can use the `path()` method present on the `UploadedHelper` class.

```php
// get the path to the original video
uploaded($fullPathToVideo)->path();

// get the full path to the original video
uploaded($fullPathToVideo)->path(null, true);
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$video = Upload::find($id);

// get the path to the original video
$video->helper->path();

// get the full path to the original video
$video->helper->path(null, true);
```

<a name="check-if-video-exists"></a>
#### Check If Video Exists

You can check if an uploaded video actually exists on the disk by using the `exists()` method present on the `UploadedHelper` class.

```php
uploaded($fullPathToVideo)->exists();
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$video = Upload::find($id);

// check if the video exists
$video->helper->exists();
```

<a name="upload-audio"></a>
## Upload Audio

To upload audio you can use the `upload()` method present on the `Varbox\Services\UploadService` class.   

```php
use Varbox\Services\UploadService;

// by using the UploadedFile object
$audio = (new UploadService($request->file('audio')))->upload();

// by providing a public url
$audio = (new UploadService('https://your-audio-url.jpg'))->upload();

// by using the $_FILES global variable
$audio = (new UploadService($_FILES['audio']))->upload();
    
return $audio->getPath() . '/' . $audio->getName();
```

<a name="audio-max-size"></a>
#### Audio Max Size

When uploading an audio file, the `UploadService` will automatically check if the file is smaller than the maximum size allowed. 
If the audio file is bigger, a `UploadException::maxSizeExceeded()` error will be thrown.

To modify the maximum size allowed for audio, change the `audios -> max_size` value inside the `config/varbox/upload.php` config file.

```php
'audios' => [

    /*
    |
    | The maximum size allowed for uploaded audio in MB.
    |
    | If any integer value is specified, files larger than this defined size won't be uploaded.
    | To allow all audio of all sizes to be uploaded, specify the "null" value for this option.
    |
    */
    'max_size' => 5,

]
```

<a name="allowed-audio-extensions"></a>
#### Allowed Audio Extensions

When uploading an audio, the `UploadService` will automatically check if the audio extension is allowed. 
If the extension is not allowed, a `UploadException::extensionNotAllowed()` error will be thrown.

To modify the list of allowed extensions for audio, change the `audios -> allowed_extensions` value inside the `config/varbox/upload.php` config file.

```php
'audios' => [

    /*
    |`
    | The allowed extensions for audio.
    | All audio extensions can be found in \Varbox\Services\UploadService::getAudioExtensions().
    |
    | You can specify allowed extensions by using an array, or a comma "," separated string of extensions.
    | To allow uploading any audio, specify the "null" value for this option.
    |
    */
    'allowed_extensions' => [
        'mp3', 'wav', 'aac', 'ogg',
    ],

]
```

<a name="save-audio-to-database"></a>
#### Save Audio To Database

When uploading an audio, the `UploadService` will automatically create a database record for it inside the `uploads` database table. 

To enable or disable saving to the database, change the `database -> save` value inside the `config/varbox/upload.php` config file.

> Disabling this option will cause the "Admin -> Media Library -> Uploads" section to stop working, among other things.

```php
'database' => [

    /*
    |
    | Determine if the uploaded files' details will be saved to the database.
    |
    | This is encouraged, every uploaded file would have more details about it.
    | Also, if this is saved, applying the HasUploads trait on models, unlocks certain query scopes.
    |
    | To disable this, set the value to "false".
    |
    */
    'save' => true,

]
```

<a name="retrieve-audio"></a>
## Retrieve Audio

To display audio you can use the `Varbox\Helpers\UploadedHelper` class.   

This class is also aliased through a helper function called `uploaded()` that accepts the full path of the audio as its single argument.

<a name="display-original-audio"></a>
#### Display Original Audio

To display the original audio, you can use the `url()` method present on the `UploadedHelper` class.

```php
<audio controls>
    <source src="{{ uploaded($fullPathToAudio)->url() }}" type="audio/mp3">
</audio>
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$audio = Upload::find($id);

// display the audio in your blade files
<audio controls>
    <source src="{{ $audio->helper->url() }}" type="audio/mp3">
</audio>
```

<a name="download-original-audio"></a>
#### Download Original Audio

To download an uploaded audio, you can use the `download()` method present on the `UploadService` class.

```php
use Varbox\Services\UploadService;

return (new UploadService($fullPathToAudio))->download();
```

<a name="retrieve-audio-path"></a>
#### Retrieve Audio Path

To get the audio path, you can use the `path()` method present on the `UploadedHelper` class.

```php
// get the path to the original audio
uploaded($fullPathToAudio)->path();

// get the full path to the original audio
uploaded($fullPathToAudio)->path(null, true);
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$audio = Upload::find($id);

// get the path to the original audio
$audio->helper->path();

// get the full path to the original audio
$audio->helper->path(null, true);
```

<a name="check-if-audio-exists"></a>
#### Check If Audio Exists

You can check if an uploaded audio actually exists on the disk by using the `exists()` method present on the `UploadedHelper` class.

```php
uploaded($fullPathToAudio)->exists();
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$audio = Upload::find($id);

// check if the audio exists
$audio->helper->exists();
```

<a name="upload-files"></a>
## Upload Files

To upload files you can use the `upload()` method present on the `Varbox\Services\UploadService` class.   

```php
use Varbox\Services\UploadService;

// by using the UploadedFile object
$file = (new UploadService($request->file('file')))->upload();

// by providing a public url
$file = (new UploadService('https://your-file-url.jpg'))->upload();

// by using the $_FILES global variable
$file = (new UploadService($_FILES['file']))->upload();
    
return $file->getPath() . '/' . $file->getName();
```

<a name="file-max-size"></a>
#### File Max Size

When uploading a file, the `UploadService` will automatically check if the file is smaller than the maximum size allowed. 
If the file is bigger, a `UploadException::maxSizeExceeded()` error will be thrown.

To modify the maximum size allowed for files, change the `files -> max_size` value inside the `config/varbox/upload.php` config file.

```php
'files' => [

    /*
    |
    | The maximum size allowed for uploaded normal files in MB.
    |
    | If any integer value is specified, files larger than this defined size won't be uploaded.
    | To allow all files of all sizes to be uploaded, specify the "null" value for this option.
    |
    */
    'max_size' => 2,

]
```

<a name="allowed-file-extensions"></a>
#### Allowed File Extensions

When uploading a file, the `UploadService` will automatically check if the file extension is allowed. 
If the extension is not allowed, a `UploadException::extensionNotAllowed()` error will be thrown.

To modify the list of allowed extensions for files, change the `files -> allowed_extensions` value inside the `config/varbox/upload.php` config file.

```php
'files' => [

    /*
    |
    | The allowed extensions for normal files.
    |
    | You can specify allowed extensions by using an array, or a comma "," separated string of extensions.
    | To allow uploading any audio files, specify the "null" value for this option.
    |
    */
    'allowed_extensions' => null,

]
```

<a name="save-file-to-database"></a>
#### Save Audio To Database

When uploading an audio, the `UploadService` will automatically create a database record for it inside the `uploads` database table. 

To enable or disable saving to the database, change the `database -> save` value inside the `config/varbox/upload.php` config file.

> Disabling this option will cause the "Admin -> Media Library -> Uploads" section to stop working, among other things.

```php
'database' => [

    /*
    |
    | Determine if the uploaded files' details will be saved to the database.
    |
    | This is encouraged, every uploaded file would have more details about it.
    | Also, if this is saved, applying the HasUploads trait on models, unlocks certain query scopes.
    |
    | To disable this, set the value to "false".
    |
    */
    'save' => true,

]
```

<a name="retrieve-files"></a>
## Retrieve Files

To display files you can use the `Varbox\Helpers\UploadedHelper` class.   

This class is also aliased through a helper function called `uploaded()` that accepts the full path of the file as its single argument.

<a name="display-original-file"></a>
#### Display Original File

To display the original file, you can use the `url()` method present on the `UploadedHelper` class.

```php
<a href="{{ uploaded($fullPathToFile)->url() }}">View File</a>
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$file = Upload::find($id);

// display the audio in your blade files
<a href="{{ $file->helper->url() }}">View File</a>
```

<a name="download-original-file"></a>
#### Download Original File

To download an uploaded file, you can use the `download()` method present on the `UploadService` class.

```php
use Varbox\Services\UploadService;

return (new UploadService($fullPathToFile))->download();
```

<a name="retrieve-file-path"></a>
#### Retrieve File Path

To get the file path, you can use the `path()` method present on the `UploadedHelper` class.

```php
// get the path to the original file
uploaded($fullPathToFile)->path();

// get the full path to the original file
uploaded($fullPathToFile)->path(null, true);
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$file = Upload::find($id);

// get the path to the original file
$file->helper->path();

// get the full path to the original file
$file->helper->path(null, true);
```

<a name="check-if-file-exists"></a>
#### Check If File Exists

You can check if an uploaded file actually exists on the disk by using the `exists()` method present on the `UploadedHelper` class.

```php
uploaded($fullPathToFile)->exists();
```

If you're working with a `Varbox\Models\Upload` model you can use the `helper` accessor instead of the `uploaded()` helper function.

```php
use Varbox\Models\Upload;

// fetch the upload record from database
$file = Upload::find($id);

// check if the file exists
$file->helper->exists();
```

<a name="model-usage"></a>
## Model Usage

Besides the fact that you can manage files directly, you also have the possibility of associating files with your `eloquent models`

Your models should use the `Varbox\Traits\HasUploads` trait. 
The trait contains an abstract method `getUploadConfig()` that you must implement yourself.   

Here's an example of how to implement the trait:

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Varbox\Traits\HasUploads;

class YourModel extends Model
{
    use HasUploads;

    /**
     * Get the specific upload config parts for this model.
     *
     * @return array
     */
    public function getUploadConfig()
    {
        return [];
    }
}
```

<a name="specific-model-configurations"></a>
#### Specific Model Configurations

One of the perks of associating files to your eloquent models is that you have the possibility of overwriting the universal upload configurations for that specific model alone.

You can overwrite specific upload configurations by returning the custom upload configuration in the `getUploadConfig()` method present on your models, for those exact configurations present in `config/varbox/upload.php` config file.

> The array returned by your `getUploadConfig()` method must be of the same format as the array returned by the `config/varbox/upload.php` config file, because behind the scenes, the `UploadService` class uses the `array_replace_recursive` method to overwrite specific configuration options.

```php
/**
 * Get the specific upload config parts for this model.
 *
 * @return array
 */
public function getUploadConfig()
{
    return [

        'images' => [
            // overwrite the max size allowed for images
            'max_size' => 2,
            
            // overwrite the allowed extensions for images
            'allowed_extensions' => [
                'jpeg', 'jpg', 'png',
            ],
                
            // disable the thumbnail generation
            'generate_thumbnail' => false,
            
            // add extra image variants for the uploaded image
            'styles' => [
                // "image" is the name of the table column
                // it will hold the full path of the uploaded image
                'image' => [
                    // add a "portrait" image variant for the uploaded image
                    'portrait' => [
                        'width' => '100',
                        'height' => '300',
                        'ratio' => true,
                    ],
                    // add a "landscape" image variant for the uploaded image
                    'landscape' => [
                        'width' => '300',
                        'height' => '100',
                        'ratio' => true,
                    ]
                ],
            ],
        ],

        'videos' => [
            // overwrite the max size allowed for videos
            'max_size' => 5,
            
            // overwrite the allowed extensions for videos
            'allowed_extensions' => [
                'mp4', 'flv'
            ],
        ],
    
        // and the list of overwrites can continue
    ];
}
```

<a name="admin-file-management"></a>
#### Admin File Management

Up until this point you've learned how you can leverage the power of the upload functionality.
   
When it comes to managing files inside the Varbox Admin however, things are much more easier. 
You just have to use the `uploader()` helper method that will generate the entire file manager. 

> The `uploader()` helper function is a wrapper for the `Varbox\Helpers\UploaderHelper` class.

By leveraging that helper method, an upload field will automatically be created for you in your admin form. But that's not all! 
You also don't have to think about the uploading functionality itself, because the `uploader()` will take care of that by using the `Varbox\Controllers\UploadController` behind the scenes.

> The `uploader()` automatically generates for you a file manager to be used inside the admin that supports the following, out of the box: 
> - Search based file listing, split into 4 categories (images, videos, audio and files)
> - Uploading new files for the model record
> - Removing the uploaded file for the model record
> - Viewing the uploaded file for the model record
> - Cropping an uploaded image for the model record

Let's say you have a `App\Post` model and you want to support uploading an `image` for each model record inside the Varbox Admin. 
All you have to do is use the `uploader()` method inside your admin form:

```php
{!! uploader()->field('image')->label('Image')->model($item)->manager() !!}

// field (required) = the name of the database table column where to store the file
// model (required) = the model instance... loaded or not
// manager (required) = this method puts everything together
```

You can also specify a json column attribute. It's also recommended to `cast` the `data` attribute to `array` in your model class.

```php
{!! uploader()->field('data[image]')->label('Image')->model($item)->manager() !!}
```

You can also specify specific file types to be shown by using the `types()` method on the `uploader()` helper function.

```php
{!! uploader()->field('data[image]')->label('Image')->model($item)->types('image', 'video')->manager() !!}
```

You can also specify which extensions to allow for uploading by using the `accept()` method on the `uploader()` helper function.

```php
{!! uploader()->field('data[image]')->label('Image')->model($item)->accept('jpg', 'png')->manager() !!}
```

You can also disable the file manager by using the `disabled()` method on the `uploader()` helper function.

```php
{!! uploader()->field('data[image]')->label('Image')->model($item)->disabled()->manager() !!}
```

<a name="upload-model-methods"></a>
#### Upload Model Methods

The `HasUploads` trait contains a few methods that facilitate the use of the `UploadService` class.   

> These are just wrapper methods for the `UploadService`, thus making it more natural to work directly with an `eloquent model`. You can still use the `UploadService` no problem.

To upload a file for a model, you can use the `uploadFile()` method present on the `HasUploads` trait.

```php
$image = $model->uploadFile($request->file('image'), 'image');

// the second parameter "image" represents the database table column
// in there you should store the full path of the image
```

To download a file uploaded on a model, you can use the `downloadFile()` method present on the `HasUploads` trait.

```php
return $model->downloadFile('image');

// the parameter "image" represents the database table column
// in there it should be stored the full path of the image
```

To show a file uploaded on a model, you can use the `showFile()` method present on the `HasUploads` trait.

```php
return $model->showFile('image');

// the parameter "image" represents the database table column
// in there it should be stored the full path of the image
```

To get an `UploadedHelper` instance for a file uploaded on a model, you can use the `uploadedFile()` method present on the `HasUploads` trait.

```php
$uploaded = $model->uploadedFile('image');
// the parameter "image" represents the database table column
// in there it should be stored the full path of the image
```

<a name="configuration"></a>
## Configuration

The crud configuration file is located at `config/varbox/upload.php`.

For more information on how you can customize the upload functionality, please read the comments from that file.

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

<p class="overwrite-class">Varbox\Services\UploadService</p>

Found in `config/varbox/bindings.php` at `services.upload_service` key.   
This class is used for the entire file upload functionality.

<p class="overwrite-class">Varbox\Models\Upload</p>

Found in `config/varbox/bindings.php` at `models.upload_model` key.   
This class represents the upload model.

<p class="overwrite-class">Varbox\Controllers\UploadController</p>

Found in `config/varbox/bindings.php` at `controllers.upload_controller` key.   
This class is used for interactions with the uploading functionality.

<p class="overwrite-class">Varbox\Controllers\UploadsController</p>

Found in `config/varbox/bindings.php` at `controllers.uploads_controller` key.   
This class is used for interactions with the "Admin -> Media Library -> Uploads" section.

<p class="overwrite-class">Varbox\Requests\UploadRequest</p>

Found in `config/varbox/bindings.php` at `form_requests.upload_form_request` key.   
This class is used for validating any upload when inserting into the database.

<p class="overwrite-class">Varbox\Helpers\UploadedHelper</p>

Found in `config/varbox/bindings.php` at `helpers.uploaded_helper` key.   
This class is used for displaying any uploaded file.

<p class="overwrite-class">Varbox\Helpers\UploaderHelper</p>

Found in `config/varbox/bindings.php` at `helpers.uploader_helper` key.   
This class is used for creating the entire upload manager for the admin.

<p class="overwrite-class">Varbox\Helpers\Varbox\Helpers\UploaderLangHelper</p>

Found in `config/varbox/bindings.php` at `helpers.uploader_lang_helper` key.   
This class is used for creating the entire multi-language upload manager for the admin.

<p class="overwrite-class">Varbox\Filters\UploadFilter</p>

Found in `config/varbox/bindings.php` at `filters.upload_filter` key.   
This class is used for applying the filtering logic.

<p class="overwrite-class">Varbox\Sorts\UploadSort</p>

Found in `config/varbox/bindings.php` at `sorts.upload_sort` key.   
This class is used for applying the sorting logic.

