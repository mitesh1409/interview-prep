# PHP Interview Questions & Answers

## #1 Magic methods in PHP

Magic methods in PHP are special methods that start with a double underscore (__).
Using magic methods you can add special behaviors to your classes without needing to explicitly call them.
PHP automatically invokes these methods in specific situations.

List of magic methods:

- __construct()
- __destruct()
- __call()
- __callStatic()
- __get()
- __set()
- __isset()
- __unset()
- __sleep()
- __wakeup()
- __serialize()
- __unserialize()
- __toString()
- __invoke()
- __set_state()
- __clone()
- __debugInfo()

### __serialize()/__sleep(), __unserialize()/__wakeup()

serialize() => __serialize()/__sleep()
unserialize() => __unserialize()/__wakeup()

When serializing objects using `serialize()`, PHP will attempt to call the member functions `__serialize()` or
`__sleep()` prior to serialization. This is to allow the object to do any last minute clean-up, etc. prior to
being serialized.

Likewise, when the object is restored using `unserialize()` the `__unserialize()` or `__wakeup()` member
function is called.

Note: Either `__serialize()` or `__sleep()` will be called. Similarly either `__unserialize()` or `__wakeup()` will be called. Not both.

The intended use of `__sleep()` is to commit pending data or perform similar cleanup tasks. Also, the function is useful if a very large object doesn't need to be saved completely.

`__serialize()` can be used to pre-process data before serialization.

References
[Magic Methods](https://www.php.net/manual/en/language.oop5.magic.php)
[serialize a value](https://www.php.net/manual/en/function.serialize.php)

---

## #2 serialize & unserialize

Serialization is the process of converting a data structure or object into a format that can be easily stored or transmitted, and then reconstructed later. The PHP `serialize()` function takes a PHP value (like an array, object, or string) and returns a string representation of it.

You can restore the serialized object using `unserialize()`.

`serialize()` Generates a storable representation of a value.
Returns a string containing a byte-stream representation of value that can be stored anywhere.

### Use Cases for Serialization

The primary use case for serialization is to **persist data** in a state that can be easily stored and retrieved later. This is particularly useful for:

* **Caching:** Storing complex data structures (like a user's session data or the result of a database query) in a file or a key-value store like Redis or Memcached. This avoids the need to re-create the data on every page load, significantly improving performance.

* **Session Management:** Saving the state of a user's session. PHP's built-in session handler uses a form of serialization to store the `$_SESSION` superglobal on disk.

* **Inter-process Communication:** Passing complex data between different PHP scripts or processes.

* **Background Jobs:** Storing job data in a queue (e.g., Redis Queue or RabbitMQ) that will be processed by a worker script later. The job data often contains objects or arrays that need to be serialized to be stored in the queue.

### When and Why to Use `serialize()`

You should use `serialize()` when you need to **store a complex PHP value in a place that only accepts strings**. The function ensures that all the data types, relationships, and structure of the original value are preserved. The corresponding `unserialize()` function can then perfectly reconstruct the original variable.

Here are a couple of examples:

#### Example 1: Caching an Object

Let's say you have a `User` object and you want to cache it to avoid hitting the database every time.

```php
class User {
    public $id;
    public $name;

    public function __construct($id, $name) {
        $this->id = $id;
        $this->name = $name;
    }
}

// Imagine this is from a database query
$user = new User(1, 'Alice');

// Serialize the object to a string
$serializedUser = serialize($user);

// Store the string in a cache file or database
file_put_contents('cache/user_1.cache', $serializedUser);

// Later, retrieve the string from the cache
$cachedData = file_get_contents('cache/user_1.cache');

// Unserialize the string back into a full object
$cachedUser = unserialize($cachedData);

// Now you have the original object back
echo "User ID: {$cachedUser->id}, Name: {$cachedUser->name}";
```

In this example, the `serialize()` function takes the `User` object and converts it into a string that stores its class name, properties, and values. This allows us to save the object's **state** to a file and retrieve it later without losing any information. The `unserialize()` function then performs the reverse operation.

#### Example 2: Storing an Array in a Cookie

Cookies can only store string data. If you need to save an array of settings for a user, you must first serialize it.

```php
// An array of user preferences
$preferences = [
    'theme' => 'dark',
    'notifications' => true,
    'language' => 'en_US'
];

// Serialize the array to a string
$serializedPreferences = serialize($preferences);

// Now you can store this string in a cookie
// setcookie('user_prefs', $serializedPreferences);

// When the user returns, you retrieve the cookie
// $cookieValue = $_COOKIE['user_prefs'];

// And then unserialize it back into an array
$retrievedPreferences = unserialize($serializedPreferences);

// You can now access the original array values
echo "User's theme is: " . $retrievedPreferences['theme'];
```

Without `serialize()`, you would have to manually loop through the array and store each key-value pair in a separate cookie, which is inefficient and complicated. `serialize()` handles all of this automatically, making it the ideal tool for such tasks.

### Magic methods

Magic methods like `__sleep()`, `__wakeup()`, `__serialize()`, and `__unserialize()` are useful during serialization to **manage an object's state** and ensure it can be properly stored and restored. They allow you to control which properties are serialized and perform any necessary setup or cleanup tasks.

#### Use Case 1: Excluding Properties from Serialization

Sometimes, an object has properties that you don't want to save, such as a **database connection handler**, a large file handle, or a temporary cache. These resources cannot be serialized and would break the process. `__sleep()` is used to explicitly exclude them.

**Example:**
A `DatabaseConnection` class might have a resource that cannot be serialized. You can use `__sleep()` to tell PHP to only serialize the properties you care about, like the host and username, and not the connection itself.

```php
class DatabaseConnection {
    private $host;
    private $user;
    private $password;
    private $connection; // This resource should not be serialized

    public function __construct($host, $user, $password) {
        $this->host = $host;
        $this->user = $user;
        $this->password = $password;
        $this->connect();
    }

    private function connect() {
        // Imagine this creates a database connection resource
        $this->connection = new stdClass();
        echo "Connected to the database.\n";
    }

    // Called before serialization
    public function __sleep() {
        // Return an array of property names to serialize
        return ['host', 'user', 'password'];
    }

    // Called after unserialization
    public function __wakeup() {
        // Re-establish the connection after the object is restored
        $this->connect();
    }
}

// Create the object and connect
$db = new DatabaseConnection('localhost', 'root', 'secret');

// Serialize the object. __sleep() is called, excluding `connection`.
$serializedDb = serialize($db);

// Simulate storing and then retrieving the serialized data
$restoredDb = unserialize($serializedDb);

// The connection is re-established by __wakeup()
// Output:
// Connected to the database.
// Connected to the database.
```

In this example, `__sleep()` ensures that only the `host`, `user`, and `password` properties are included in the serialized string. When the object is unserialized, `__wakeup()` is called, which then uses those properties to **re-establish the database connection**. This prevents a fatal error and ensures the object is usable after it's restored.

#### Use Case 2: Data Pre-processing for Serialization

The `__serialize()` and `__unserialize()` methods (available since PHP 7.4) offer a more modern and powerful alternative to `__sleep()` and `__wakeup()`. They provide full control over the serialization process by allowing you to return an associative array of data to be serialized.

**Example:**
You can use `__serialize()` to perform cleanup or data conversion before saving, and then `__unserialize()` to perform the reverse on retrieval.

```php
class ShoppingCart {
    private $items = [];

    public function addItem($name, $price) {
        $this->items[$name] = $price;
    }

    // New in PHP 7.4
    public function __serialize() {
        // Transform the data before serialization
        $total = array_sum($this->items);
        return [
            'item_count' => count($this->items),
            'items' => $this->items,
            'total_price' => $total
        ];
    }

    // New in PHP 7.4
    public function __unserialize(array $data) {
        // Restore the object from the serialized data
        if (isset($data['items'])) {
            $this->items = $data['items'];
        }
    }
}

$cart = new ShoppingCart();
$cart->addItem('Laptop', 1200);
$cart->addItem('Mouse', 25);

// __serialize() is called, returning the transformed data.
$serializedCart = serialize($cart);

// The serialized string will contain 'item_count' and 'total_price'
echo $serializedCart . "\n"; 
// Example output: O:12:"ShoppingCart":3:{s:10:"item_count";i:2;s:5:"items";a:2:{s:6:"Laptop";i:1200;s:5:"Mouse";i:25;}s:11:"total_price";i:1225;}

// When unserializing, __unserialize() restores the object.
$restoredCart = unserialize($serializedCart);

// Check if the original data is intact
var_dump($restoredCart);
```

In this example, `__serialize()` calculates and includes the `total_price` and `item_count` in the serialized data. This is useful if you want to save a summary of the object's state without having to recompute it every time you unserialize. `__unserialize()` then simply restores the `items` array, as the other properties were only for serialization and don't need to be stored in the object's state.

---

## #3 OOP > class

### class

A class is a blueprint for an object/instance.
In other words a class defines how an object/instance should be constructed/created.

A class consists of

- constants
- properties/variables
- methods/functions

PHP treats objects in the same way as references or handles, meaning that each variable contains an object
reference rather than a copy of the entire object.

### Readonly classes

As of PHP 8.2.0, a class can be marked with the readonly modifier. Marking a class as readonly will add the readonly modifier to every declared property, and prevent the creation of dynamic properties. Moreover, it is impossible to add support for them by using the AllowDynamicProperties attribute. Attempting to do so will trigger a compile-time error.

### new

To create an instance of a class, the new keyword must be used.

## Properties and Methods

Class properties and methods live in separate "namespaces", so it is possible to have a property and a method with the same name. Referring to both a property and a method has the same notation, and whether a property will be accessed or a method will be called, solely depends on the context, i.e. whether the usage is a variable access or a function call.

```php
<?php

class Foo
{
    public $bar = 'property';
    
    public function bar() {
        return 'method';
    }
}

$obj = new Foo();
echo $obj->bar, PHP_EOL, $obj->bar(), PHP_EOL;

```

