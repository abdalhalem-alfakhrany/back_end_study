# php 8.1
## new features
### Enumerations  
```php
// now we can do this
enum Status {
    case Loading;
    case Loaded;
};

$status = Status::Loading;

echo $status->name . "\n";
```

### Readonly Properties 
```php
class MyClass {
    public readonly $x;
   
    public function __construct($x) {
        $this->x = $x;
    }
}

$myObject = new MyClass(10);
$myObject->x = 10; // not allowed because its readonly
```

### First-class Callable Syntax 
```php
// PHP < 8.1
$foo = [$this, 'foo'];

$fn = Closure::fromCallable('strlen');


// PHP 8.1 now we can do this
$foo = $this->foo();

$s = "hi";
$fn = strlen($s);
```

### New in initializers
```php
// PHP < 8.1
class Service {
    private Logger $logger;
 
    public function __construct(
        ?Logger $logger = null,
    ) {
        $this->logger = $logger ?? new NullLogger();
    }
}


// PHP 8.1
class Service  {
    private Logger $logger;
    
    public function __construct(
        Logger $logger = new NullLogger(),
    ) {
        $this->logger = $logger;
    }
}
```

### Pure Intersection Types 
```php
function count_and_iterate(Iterator $value) {
    if (!($value instanceof Countable)) {
        throw new TypeError('value must be Countable');
    }

    foreach ($value as $val) {
        echo $val;
    }

    count($value);
}

// now we can do this
function count_and_iterate(Iterator&Countable $value) {
    foreach ($value as $val) {
        echo $val;
    }

    count($value);
}
```

### Never return type
```php
function bar($x) : never {
    echo $x;
    exit();
}

function foo($x) : never {
    bar($x + 1);
    echo $x; // this will be detected bey static analyzer as dead code
}
```

### Saner string to number comparisons
```php
class Foo {
    final public const XX = "foo";
}

class Bar extends Foo {
    public const XX = "bar"; // Fatal error
}
```

### Explicit Octal numeral notation 
```php
// PHP < 8.1
016 === 16; // false because `016` is octal for `14` and it's confusing
016 === 14; // true 

// PHP 8.1
0o16 === 16; // false — not confusing with explicit notation
0o16 === 14; // true 
```

### Fibers
```php
// PHP < 8.1
$httpClient->request('https://example.com/')
        ->then(function (Response $response) {
            return $response->getBody()->buffer();
        })
        ->then(function (string $responseBody) {
            print json_decode($responseBody)['code'];
        });

// with fibers we can do it like this and don't care about promises
$response = $httpClient->request('https://example.com/');
print json_decode($response->getBody()->buffer())['code'];
```

### Array unpacking support for string-keyed arrays

```php
$foo = ['a' => 1];
$bar = ['b' => 2];

$fobr = [...$foo, ...$bar];

// it will override the keys already declared also
$fobr = ['a' => 0, ...$foo, ...$bar];
```