### 1. What is the SOLID principle?

SOLID is a set of 5 design principles used for writing clean, maintainable, and scalable object-oriented code:

S — Single Responsibility Principle
A class should have only one reason to change.

O — Open/Closed Principle
Classes should be open for extension but closed for modification.

L — Liskov Substitution Principle
A child class should be able to replace its parent class without breaking the application.

I — Interface Segregation Principle
Many small interfaces are better than one large interface.

D — Dependency Inversion Principle
Depend on abstractions (interfaces), not on concrete implementations.

### 2. What is OOP in PHP?

OOP (Object-Oriented Programming) in PHP is based on the following core concepts:

Encapsulation

Inheritance

Polymorphism

Abstraction

Traits

Interfaces

Example:

```php
class User {
private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }

}
```

### 3. What is ORM?

ORM (Object Relational Mapping) maps database tables to PHP objects.

Laravel uses Eloquent ORM.

```php
$user = User::where('id', 1)->first();
```

This means:

users table → User model

Row → Object

### 4. What is Service Container in Laravel?

Laravel's Service Container is responsible for managing dependencies and performing dependency injection.

```php
$this->app->bind(Service::class, function() {
return new Service();
});
```

It allows Laravel to automatically resolve class dependencies.

### 5. Output Based Question

```php
$a = 5;
$b = $a;
$a++ + $a++;
echo $a;
```

✅ Output: 7

Explanation:

$a++ → uses old value then increments

Two increments = final value becomes 7

### 6. What is Inheritance? Give an example

Inheritance allows one class to inherit properties and methods of another class.

```php
class Car {
public $brand;

    public function __construct($brand) {
        $this->brand = $brand;
    }

    public function startEngine() {
        echo "Engine started on " . $this->brand;
    }

}

class Vehicle extends Car {
public function honk() {
echo "Horn for " . $this->brand;
}
}

$vehicle = new Vehicle("Toyota");
$vehicle->startEngine();
$vehicle->honk();
```

### 7. What is the difference between include, require, include_once, require_once?

include require
Shows warning if file not found Shows fatal error
Script continues Script stops

\_once versions prevent multiple inclusions of same file.

### 8. What is the difference between == and === in PHP?

Operator Meaning
== Compares values only
=== Compares value + type
5 == "5" // true
5 === "5" // false

### 9. What is a Trait in PHP?

Traits are used to reuse code in multiple classes because PHP does not support multiple inheritance.

```php
trait Logger {
public function log($msg){
echo $msg;
}
}


class User {
use Logger;
}
```

### 10. What is an Interface in PHP?

Interfaces define methods but do NOT implement them.

```php
interface PaymentGateway {
public function makePayment();
}

class Paypal implements PaymentGateway {
public function makePayment(){
echo "Paid via PayPal";
}
}
```

### 11. What is Abstract Class?

Cannot create object of abstract class

Can contain abstract + non-abstract methods

```php
abstract class Animal {
abstract public function sound();
}

class Dog extends Animal {
public function sound() {
return "Bark";
}
}
```

### 12. What is Dependency Injection?

Dependency Injection means passing dependencies instead of creating them inside the class.

```php
class OrderController {
public function \_\_construct(PaymentService $service) {
$this->service = $service;
}
}
```

This improves testability and flexibility.

### 13. What is Middleware in Laravel?

Middleware filters HTTP requests.

Examples:

Authentication

Logging

Role-based permission

```php
Route::get('/admin', function(){})->middleware('auth');
```

### 14. What is Migration in Laravel?

Migrations are like version control for database.

```cmd
php artisan make:migration create_users_table
php artisan migrate
```

### 15. What is Seeder in Laravel?

Seeders are used to insert dummy or test data.

```cmd
php artisan db:seed
```

### 16. What is CSRF in Laravel?

CSRF = Cross-Site Request Forgery

Laravel prevents it using a token:

@csrf

### 17. What is API Resource in Laravel?

Used to transform model data into JSON.

php artisan make:resource UserResource

### 18. Difference between GET and POST

    GET POST
    Data in URL Data in Body
    Limited length Unlimited
    Less secure More secure

### 19. What is the N+1 Query Problem?

When a loop runs multiple DB queries:

Bad:

```php
foreach($users as $user){
echo $user->posts;
}
```

Good:

```php
User::with('posts')->get();
```

### 20. What is Route Model Binding?

Laravel automatically injects model based on route parameter.

```php
Route::get('/user/{user}', function(User $user) {
return $user;
});
```

### 21. What is Mass Assignment in Laravel?

Used to insert multiple records at once.

```php
protected $fillable = ['name', 'email'];
User::create($data);
```

### 22. What is Composer?

Composer is a dependency manager for PHP.

composer install
composer update

### 23. What is pagination in Laravel?

    User::paginate(10);

### 24. What is an accessor & mutator?

Accessor:

```php
public function getNameAttribute($value) {
  return strtoupper($value);
}
```

Mutator:

```php
public function setNameAttribute($value) {
  $this->attributes['name'] = strtolower($value);
}
```

### 25. What is a Job & Queue?

Queue is used for background processing:

Emails

Push notifications

Excel exports

php artisan queue:work
