# Server Requirements

Besides [Laravel's Requirements](https://laravel.com/docs/7.x#server-requirements) the [VarBox](/) platform doesn't impose any new ones.   

> However, there are a few things required only when using certain features from the platform:

##### [Video Uploads](/docs/{{version}}/upload-files#upload-videos)

Requires `ffmpeg` and `ffprobe`

##### [Persistent Query Caching](/docs/{{version}}/query-cache#cache-all-queries)

Requires `redis` or `memcached`
