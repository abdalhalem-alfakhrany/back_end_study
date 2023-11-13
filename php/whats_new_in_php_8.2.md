# php 8.2
## new features
### Readonly classes 
```php
readonly class MyClass {
    public string $foo;
    public string $bar;

    public function __construct($foo, $bar)
    {
        $this->foo = $foo;
        $this->bar = $bar;
    }
}
// all class properties will be readonly 
```

### Disjunctive Normal Form (DNF) Types
```php

// PHP < 8.2
class Foo {
    public function bar(mixed $entity) {
        if ((($entity instanceof A) && ($entity instanceof B)) || ($entity === null)) {
            return $entity;
        }

        throw new Exception('Invalid entity');
    }
}


// PHP 8.2
class Foo {
    // intersection types must be grouped by brackets
    public function bar((A&B)|null $entity) {
        return $entity;
    }
}
```
### Allow null, false, and true as stand-alone types
```php
class AClassWithUbNormalTypes
{
    public function alwaysFalse(): false { /* ... */ }

    public function alwaysTrue(): true { /* ... */ }

    public function alwaysNull(): null { /* ... */ }
}
```
### Constants in traits 
```php
trait MyTrait {
    public const FOO = "bar";
}

class MyClass {
    use MyTrait;
}

MyClass::FOO;
```

### Deprecate dynamic properties
```php
class User
{
    public $name;
}

$user = new User();
$user->last_name = 'Doe'; // Deprecated notice

$user = new stdClass();
$user->last_name = 'Doe'; // Still allowed
```
