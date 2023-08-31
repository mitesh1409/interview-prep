# # Laravel Interview Qs & As

Reference:
[InterviewBit Laravel Interview Questions](https://www.interviewbit.com/laravel-interview-questions/)

---

Index
1. What is the latest Laravel version?
2. Define Composer
3. Templating engine used in Laravel
4. What are available databases supported by Laravel?
5. What is an artisan?
6. How to define environment variables in Laravel?
7. Can we use Laravel for Full Stack Development (Frontend + Backend)?
8. How to put Laravel applications in maintenance mode?
9. Displaying Unescaped Data OR {{ }} vs {!! !!} in Blade templates.

---

## #1 What is the latest Laravel version?

As of writing this the latest Laravel version is 9.x.

---

## #2 Define Composer.

**What is Composer?**

Just like there is "npm" for Node.js, we have "Composer" for PHP.
It is inspired by Node.js' "npm" and Ruby's "bundler".

Composer is a dependency management tool for PHP.
Refer [Composer > Getting Started](https://getcomposer.org/doc/00-intro.md)

Using Composer we can easily manage third party dependencies in our project.
We can install, remove or update third party packages using Composer.

All the third party packages are installed in the "vendor" directory.


**composer.json**
It describes all the dependencies of your project with their required version.

We can have two types of dependencies,
- Development
    These are described under "require-dev" key.
- Production
    These are described under "require" key.

This file is version controlled.


**composer.lock**
This is created/updated whenever `composer update` command is executed.

It contains all of the packages and their exact versions installed.
Locks project dependencies.

This file is version controlled.


**composer update**
At the start we need to run `composer update`.
This is the first time when someone needs to install project dependencies he/she needs to run `composer update`.
This resolves all the dependencies listed in composer.json file, saves all dependencies and their exact versions to composer.lock file.

After initial setup, whenever we need to update project dependencies to their latest versions, we need to run `composer upadte`.


**composer install**
`composer install` command is used to install project dependencies.
It installs all the dependencies with their exact same version as listed in composer.lock file.

Committing composer.lock to version control is important because it will cause anyone who sets up the project to use the exact same versions of the dependencies that you are using. Your CI server, production machines, other developers in your team, everything and everyone runs on the same dependencies.


**CLI Commands**
install
The install command reads the composer.json file from the current directory, resolves the dependencies, and installs them into vendor.
`composer install`
If there is a composer.lock file in the current directory, it will use the exact versions from there instead of resolving them. This ensures that everyone using the library will get the same versions of the dependencies.

If there is no composer.lock file, Composer will create one after dependency resolution.

update
In order to get the latest versions of the dependencies and to update the composer.lock file, you should use the update command. This command is also aliased as upgrade as it does the same as upgrade does if you are thinking of apt-get or similar package managers.
`composer update`

This will resolve all dependencies of the project and write the exact versions into composer.lock.

require
The require command adds new packages to the composer.json file from the current directory. If no file exists one will be created on the fly.
`composer require vendor/package:version`

For the development dependencies
`composer require --dev vendor/package:version`

remove
The remove command removes packages from the composer.json file from the current directory.
composer remove vendor/package vendor/package2

Autoloader Optimization
composer dump-autoload -o (or --optimize)
Optimizes PSR0 and PSR4 packages to be loaded with classmaps too, good for production.
Refter [Autoloader Optimization](https://getcomposer.org/doc/articles/autoloader-optimization.md)

reinstall
The reinstall command looks up installed packages by name, uninstalls them and reinstalls them. This lets you do a clean install of a package if you messed with its files, or if you wish to change the installation type using --prefer-install.
`composer reinstall`

Refer [CLI Commands](https://getcomposer.org/doc/03-cli.md)


**Version Constraints**

Summary with examples

```
"require": {
    "vendor/package": "1.3.2", // exactly 1.3.2

    // >, <, >=, <= | specify upper / lower bounds
    "vendor/package": ">=1.3.2", // anything above or equal to 1.3.2
    "vendor/package": "<1.3.2", // anything below 1.3.2

    // * | wildcard
    "vendor/package": "1.3.*", // >=1.3.0 <1.4.0

    // ~ | allows last digit specified to go up
    "vendor/package": "~1.3.2", // >=1.3.2 <1.4.0
    "vendor/package": "~1.3", // >=1.3.0 <2.0.0

    // ^ | doesn't allow breaking changes (major version fixed - following semver)
    "vendor/package": "^1.3.2", // >=1.3.2 <2.0.0
    "vendor/package": "^0.3.2", // >=0.3.2 <0.4.0 // except if major version is 0
}
```

Refer [Version Constraints](https://getcomposer.org/doc/articles/versions.md)

---

## #3 Templating engine used in Laravel

**Blade**

All Blade templates are compiled into plain PHP code and 
cached until they are modified, 
meaning Blade adds essentially zero overhead to your application.

Compiled view files are stored in "storage/framework/views" directory.

Blade template files use the .blade.php file extension and 
are typically stored in the "resources/views" directory.

Laravel allows us to write plain PHP templates as well. Using Blade is not mandatory.

Refer [Blade Templates](https://laravel.com/docs/9.x/blade)

---

## #4 What are available databases supported by Laravel?

Laravel provides first-party support for four databases:
- MySQL 5.7+
- PostgreSQL 9.6+
- SQLite 3.8.8+
- SQL Server 2017+

Refer [Database: Getting Started](https://laravel.com/docs/9.x/database)

---

## #5 What is an artisan?

Artisan is the command line interface included with Laravel.

Artisan exists at the root of your application as the artisan script and provides a number of helpful commands that can assist you while you build your application.

To view a list of all available Artisan commands, you may use the list command:
`php artisan list`

Refer [Artisan Console](https://laravel.com/docs/9.x/artisan)

Some artisan commands:

`php artisan down`
Puts the application into maintenance mode.

`php artisan up`
Brings the application out of maintenance mode.

`php artisan key:generate`
Set the application key.

`php artisan test`
Run the application tests.

`php artisan serve`
Serve the application on the PHP development server.

`php artisan migrate`
Run the database migrations.

`php artisan db`
Start a new database CLI session.

`php artisan cache:clear`
Flush the application cache.

`php artisan make:command`
Create a new Artisan command.

`php artisan make:controller`
Create a new controller class.

`php artisan make:model`
Create a new Eloquent model class.

`php artisan make:factory`
Create a new model factory class.

`php artisan make:job`
Create a new job class.

`php artisan make:middleware`
Create a new middleware class.

---

## #6 How to define environment variables in Laravel?

The environment variables can be defined in the .env file in the project directory.

In a fresh Laravel installation, the root directory of your application will contain a .env.example file 
that defines many common environment variables. During the Laravel installation process, this file will
automatically be copied to .env.

Some of the examples of environment variables are APP_ENV, DB_HOST, DB_PORT etc.

params defined in the .env file can be accessed using env() helper function.

**.env Vs config**

If the value of a parameter remains same across all the environments (dev, test, sandbox, prodution etc.),
then it should be defined in a config file.

If the value of a parameter is different across all the environments (dev, test, sandbox, prodution etc.),
then it should be defined in a .env file.

**Environment File Security**

Your .env file should not be committed to your application's source control,
since each developer / server using your application could require a different environment configuration.

Furthermore, this would be a security risk in the event an intruder gains access to your source control
repository, since any sensitive credentials would get exposed.

Refer [Configuration](https://laravel.com/docs/9.x/configuration)

---

## #7 Can we use Laravel for Full Stack Development (Frontend + Backend)?

Yes we can.

Laravel can serve as an API backend to a JavaScript single-page application or mobile application.

OR

Laravel can serve as a full stack framework.
By "full stack" framework we mean that you are going to use Laravel to route requests to your application
and render your frontend via "Blade" templates or using a single-page application hybrid technology like
"Inertia.js".
This is the most common way to use the Laravel framework.

---

## #8 How to put Laravel applications in maintenance mode?

`php artisan down`
Puts the application into maintenance mode.

`php artisan up`
Brings the application out of maintenance mode.

Purpose of putting an application into maintenance mode:
- Software updates
- Bug fixes
- Scheduled maintenance activities
- Outage of 3rd party services used by the application
etc.

**Bypassing Maintenance Mode**
Even while in maintenance mode, you may use the secret option to specify a maintenance mode bypass token:

`php artisan down --secret="1630542a-246b-4b66-afa1-dd72a4c43515"`
After placing the application in maintenance mode, you may navigate to the application URL matching this token and Laravel will issue a maintenance mode bypass cookie to your browser:

https://example.com/1630542a-246b-4b66-afa1-dd72a4c43515
When accessing this hidden route, you will then be redirected to the / route of the application. Once the cookie has been issued to your browser, you will be able to browse the application normally as if it was not in maintenance mode.

**Customize maintenance mode template**
You may customize the default maintenance mode template by defining your own template 
at resources/views/errors/503.blade.php.

**Maintenance mode & Queues**
While your application is in maintenance mode, no queued jobs will be handled.
The jobs will continue to be handled as normal once the application is out of the maintenance mode.

---

## #9 Displaying Unescaped Data OR {{ }} vs {!! !!} in Blade templates.

By default, Blade {{ }} statements are automatically sent through PHP's `htmlspecialchars` function 
to prevent **XSS attacks**.
If you do not want your data to be escaped, you may use the following syntax:

```
Hello, {!! $name !!}.
```

> Be very careful when echoing content that is supplied by users of your application. You should typically use the escaped, double curly brace syntax to prevent XSS attacks when displaying user supplied data.

[Laravel Docs - Displaying Unescaped Data](https://laravel.com/docs/9.x/blade#displaying-unescaped-data)
