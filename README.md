# laravel-src
SRC stands for Service Repository Controller.
With this package, you can swiftly set up these essential layers using just a single command.
Additionally, you have the flexibility to disable any of the three layers individually or opt to automatically generate a model,
migration and/or policy as well.

## Usage
### Installation

To install the package, use Composer:
```
composer require sheryan-eu/laravel-src
```

After installation, publish the configuration file to set default class paths:
```
php artisan vendor:publish --tag=laravel-src
```

You can customize the default class paths in the generated `src.php` config file to fit your needs.

### Command usage

The base command is:
```
php artisan make:src {name} {?namespace}
```

Executing this command will swiftly generate a controller along with its associated service (with interface),
as well as a repository (with interface) in one go.
Each element seamlessly integrates with the others through automatic dependency injection, ensuring immediate usability.

After generation, simply include the generated service and/or repository in your `AppServiceProvider.php`.


#### Example

Executing the command:
```
php artisan src:gen profile user
```

will generate the following files:

- `app/Http/Controllers/User/ProfileController.php`
- `app/Interfaces/Services/User/IProfileService.php`
- `app/Interfaces/Repositories/User/IProfileRepository`
- `app/Repositories/User/ProfileRepository`
- `app/Services/User/ProfileService`

**Note**: The command automatically capitalizes the first character, so `profile` will become `Profile`.
If you want your classes to have a name like `ProfilePicture`, you'll need to ensure correct capitalization yourself.

It's crucial to register your service and repository within your `AppServiceProvider.php` file:
```php
<?php

declare(strict_types=1);

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use App\Services\User\ProfileService;
use App\Repositories\User\ProfileRepository;
use App\Interfaces\Services\User\IProfileService;
use App\Interfaces\Repositories\User\IProfileRepository;

class AppServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        app()->bind(IProfileService::class, ProfileService::class);
        app()->bind(IProfileRepository::class, ProfileRepository::class);
    }
}
```

This snippet binds the interfaces to their respective implementations,
allowing seamless interaction with these classes throughout your application.
Ensure these bindings are included within the register method of the `AppServiceProvider` class.

#### Example output

After execution, the generated controller file will be located at `app/Http/Controllers/User/ProfileController.php`.
Below is an example of how the generated controller may appear:
```php
<?php

declare(strict_types=1);

namespace App\Http\Controllers\User;

use App\Http\Controllers\Controller;
use App\Interfaces\Services\User\IProfileService;

class ProfileController extends Controller
{
    /**
     * @var IProfileService
     */
    private IProfileService $profileService;

    /**
     * @param IProfileService $profileService
     */
    public function __construct(IProfileService $profileService)
    {
        $this->profileService = $profileService;
    }
}
```

### Command options

Below are the available options for customizing the generation process, enabling you to tailor the command to your specific needs:

| Name          | Type                                                                                   | Required | Description                                                                         | Default |
|---------------|:----------------------------------------------------------------------------------------:|:----------:|:------------------------------------------------------------------------------------| -------- |
| `name`        | string                                                                                 | *true*     | Base name for all generated items                                                   |  |
| `namespace`   | string                                                                                 | *false*    | Namespace for the generated items                                                   | 
| `--model`     |  | *false*    | Automatically create a model                                                        | `false`
| `--migration` |                                                                                  | *false*    | Automatically create a migration                                                    | `false`
| `--policy`    |                                                                                  | *false*    | Automatically create a policy                                                       | `false`
| `--nc`        |                                                                                  | *false*    | Disable generation of the controller                                                      | `false`
| `--resource`  |                                                                                  | *false*    | Generate a resourceful controller | `false`
| `--ns`        |                                                                                  | *false*    | Disable generation of the service and service interface                                   | `false`
| `--nr`        |                                                                                  | *false*    | Disable generation of the repository and repository interface                             | `false`

For more information, you can use: `php artisan make:src --help`.

## Adapted from Scrumble
This product was inspired by [Scrumble](https://www.scrumble.nl) and their original [implementation](https://github.com/scrumble-nl/laravel-csr/). While the idea originated from them, the package has been independently developed.

## Personal Use Disclaimer
This package is intended for personal use only. If you have suggestions or improvements, please direct them to the original implementation. Contributions to this package are not accepted.

### Postcardware
According to the postcardware concept, if you use the software for your project(s) we would appreciate to receive a postcard of your hometown. Please send it to:

Sheryan
Hoofdstraat 9A
4941 DC Raamsdonksveer
The Netherlands
