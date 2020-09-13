# Release Notes

- [Versioning Scheme](#versioning-scheme)
- [Varbox 2.x](#varbox-2.x)

<a name="versioning-scheme"></a>
## Versioning Scheme

<p id="first-p">
Varbox follows <a href="https://semver.org" target="_blank" rel="noreferrer">semantic versioning</a>. 
We're trying to release major platform releases as soon as a new Laravel version is released, but that's also dependable on the [third-party packages](/docs/{{version}}/third-party-packages) the platform uses.
Minor and patch releases may be released as often as every week. Minor and patch releases should never contain breaking changes.
</p>

<a name="varbox-2.x"></a>
## Varbox 2.x

Varbox 2.x continues the improvements made in Varbox 1.x by supporting Laravel 8.x.

#### Models Directory

Varbox 2.x complies with the changes in Laravel 8.x regarding [the new Models directory](https://laravel.com/docs/8.x/releases).

Now the `varbox:install` artisan command will look for your `User` model inside the `app/Models` directory.

#### Database Seeds

Varbox 2.x complies with the changes in Laravel 8.x regarding [seeder namespaces and directory renaming](https://laravel.com/docs/8.x/upgrade#seeder-factory-namespaces)

#### PHPUnit Deprecated Methods

Varbox 2.x now uses `assertFileDoesNotExist` instead of `assertFileNotExists` in its internal tests.
