# php 8.0
## new features
### Named arguments 
```php
$arg1 = "bar";
$arg2 = 1;

foo($arg1, $arg2)
    |
    v
foo(arg1: $arg1, arg2: $arg1)
```

### Attributes 
```php
class MyClass {
    #[attr(arg1, arg2)]
    public function foo() {}
}
```

### Constructor property promotion 
```php
// normiez class properties declaration
class MyClass {
    public $foo;
    public $bar;

    public function __constructor($foo, $bar) {
        $this->foo = $foo;
        $this->bar = $bar;
    }
}

// lords class properties declaration be like
class MyClass {

    public function __constructor(
            public $foo = "hi";
            public $bar = 0;
    ) {
    }
}

```

### Union types
```php
class MyNumber {
    public int|float $x;
    public function __construct($x){
        $this->x = $x;
    }
}

class MyNumber {
    public function __construct(public int|float $x) {}
}
```

### Match expression
```php
$x = match($y) {
    "foo" => "bar"
    0 => -1;
}
```

### Nullsafe operator 
###### most interesting one
```php
$country =  null;

if ($session !== null) {
  $user = $session->user;

  if ($user !== null) {
    $address = $user->getAddress();
  
    if ($address !== null) {
      $country = $address->country;
    }
  }
}

// now we can do this
$country = $session?->user?->getAddress()?->country;
```

### Saner string to number comparisons
```php

// in php 7 this is true
0 == 'foo';

// in php 8 this is false
0 == 'foo';

// ===
```

### Consistent type errors for internal functions
```php

// PHP 7
strlen([]); // Warning: strlen() expects parameter 1 to be string, array given

array_chunk([], -1); // Warning: array_chunk(): Size parameter expected to be greater than 0


// PHP 8
strlen([]); // TypeError: strlen(): Argument #1 ($str) must be of type string, array given

array_chunk([], -1); // ValueError: array_chunk(): Argument #2 ($length) must be greater than 0
```
