# Programming with Objects

## Classes and Objects

> A minimum viable class

```php

class EmailClient
{
    //properties and methods
}

$client1 = new EmailClient();
$client2 = new EmailClient();

echo ($client1 === $client2);//FALSE

```

> Calling a method on an instance

```php

class EmailClient
{
   public function sendMessage()
   {
        echo "sending message..."; 
   }
}

$client = new EmailClient();
$client->sendMessage();

```

> Defining a static method

```php

class EmailClient
{
   public function sendMessage()
   {
        echo "sending message..."; 
   }
   
   public static function receiveMessage()
   {
        echo "Receiving message..."; 
   }
}

$client = new EmailClient();
$client->sendMessage();

$client::receiveMessage();
//OR
EmailClient::receiveMessage();

```


> Defining constructor

```php

class EmailClient
{
    public function __construct(string $clientName)
    {
        $this->setClientName($clientName);
    }
    
   private function setClientName(string $name)
   {
        echo "sending message..."; 
   }
   
   public function getClientObj(string $name)
   {
        return new /stdClass($name);
   }
}

$client = new EmailClient('gmail');
$client->getClientObj();

```

> Throwing Exceptions

```php

class EmailClient
{
    public function __construct()
    {
        throw new RuntimeException('EmailClient should not be instantiated');
    }
   
   public function getClientObj()
   {
        return new /stdClass();
   }
}

try {
    $client = new EmailClient('gmail');
    $client->getClientObj();
 } catch (RuntimeException $exception) {
    echo $exception->getMessage();
 }


```

> Defining a static factory method

```php

class EmailClient
{
   public static function create()
   {
        return new EmailClient();
   }
}

$client1 = EmailClient::create();
$client2 = EmailClient::create();

```

## State

> An Object can contain data. At a give moment of time the data inside an object is referred as **state**. Data will be stored in properties. A property will have a name and type.

### Defining properties and assigning values

```php

class EmailClient
{
    private string $clientName;
    private int $clientPortNumber;
    
   public function __construct(string $clientName, int $clientPortNumber)
   {
        $this->clientName = $clientName;
        $this->clientPortNumber = $clientPortNumber;
   }
}

$client1 = EmailClient::create('gmail', '3302');

```

### Defining constants

```php

class EmailClient
{
    public const string CLIENT_NAME = 'gmail';
    public const int PORT_NUMBER = 3302;
}

$port = EmailClient::PORT_NUMBER;
```

### Mutable vs Immutable Objects

```php

class Mutable
{
    private int $clientNumber;
    
   public function __construct(int $clientNumber)
   {
        $this->clientNumber = $clientNumber;
   }
   
   public function increase()
   {
        $this->clientNumber = $this->clientNumber + 1;
   }
}

class Immutable
{
    private int $clientNumber;
    
   public function __construct(int $clientNumber)
   {
        $this->clientNumber = $clientNumber;
   }
   
   public function increase()
   {
        return new Immutable($this->clientNumber + 1);
   }
}

$mutableObj = new Mutable();
$mutableObj->increase();//Change the state of the object by changing value of property($clientNumber)

$immutableObj = new Immutable();
$immutableObj->increase();//Doesn't change the state of the object instead return new Object with updated value.
```

## Behavior

* Besides state, an object also has behaviors that its clients can make use of. These behaviors are defined as methods ob Object's class.

* Behavior defined in public methods

```php

class EmailClient
{
    private string $clientName;
    
    public function __construct(string $clientName)
    {
        $this->setClientName($clientName);
    }
    
   private function setClientName(string $name)
   {
        $this->clientName = $name;
   }
   
   public function getClientObj(string $name)
   {
        return new /stdClass($name);
   }
}

$client = new EmailClient('gmail');
$client->getClientObj();

```

## Dependencies

If Object **Foo** needs **Bar** to perform part of its job, **Bar** is called a dependency of **Foo**. There are different ways to make **Foo** has access to the **Bar** dependency.

1. It could instantiate **Bar** itself.
2. It could fetch a **Bar** instance from known location.
3. It could get a **Bar** instance injected upon construction.

```php

# 1st Method
class Email
{
    public function send(): void
    {
        $logger = new Logger();
        logger.debug('...');
    }
}

# 2nd Method
class Email
{
    public function send(): void
    {
        $logger = ServiceLocator.getLogger();
        logger.debug('...');
    }
}

# 3rd Method
class Email
{
    private Logger $logger;
    
    public function __construct(Logger $logger): void
    {
        $this->logger = $logger
    }
    
    public function send(): void
    {
        $this->logger.debug('...');
    }
}

```

## Inheritance

### Interfaces
* We can have a class with no properties and no methods, but only method signatures. Such a class is usually called an **interface**.

* A **class** can the implement **interface** and provide the actual implementations of the methods that were defined in the **interface**.

```php

interface Email
{
    public function send(): void
}

class Gmail implements Email
{
    public function send(): void
    {
        //implementation
    }
}

```

### Abstract Classes
* An **interface** doesn't provide any implementation, by an **abstract class** does.

* An **Abstract class** allows you to provide the implementation for some methods any only the signature for some other methods.

* An **Abstract class** can't be instantiated. But has to be extended by a class that provides implementations for the abstract methods.

```php

abstract class Email
{
    public function prepare()
    {
        //Prepare email client
    }
    
    public function send(): void
}

class Gmail extends Email
{
    public function send(): void
    {
        //send email to users
    }
}

```

### Extend and Override

* A class could provide a full implementation foa ll its methods but allow other classes to **extend and override some of its methods**.

* Classes that extend from another class have access to **public and protected methods** of the parent class.

* **Sub Classes** can only **override protected and public methods** of a parent class.

```php

class Email
{
    protected function prepare()
    {
        //Prepare email client
    }
    
    public function send(): void
    {
        //send email to users
    }
    
    private function receive(): void
    {
        //send email to users
    }
}

class Gmail extends Email
{
    public function prepare()
    {
        //Prepare gmail client
    }
    
    public function clone()
    {
        //Allowed
        $this->prepare();
        
        //Allowed
        $this->send();
        
        //Error
        $this->receive();
    }
}

```

### Final Class
> We can actively prevent developers to extend from our classes by adding **final** keyword in front of the class.

```php

final class Gmail
{
    //...
}

//Throws error
class Outlook extends Gmail
{
    //...
}
```

## Polymorphism

> **Polymorphism** means that if a parameter has a certain class as its type, any object that is an instance of that class can be provided as a valid argument.

```php

interface Email {
    public function send(): void;
}

class Gmail {
    public function send(): void
    {
        ...
    }
}

class Orders
{
    public function sendConfirmation(Email $mail){
        $mail.send();
    }
}

```

```php
abstract class Email {
    public function send(): void;
}

class Gmail extends Email {
    public function send(): void
    {
        ...
    }
}

class Orders
{
    public function sendConfirmation(Email $mail){
        $mail.send();
    }
}

```

> **Note** :
> Using subclasses to change the behavior of Objects is often not recommended. In most situations it's better to use polymorphism with an interface parameter type.

## Composition

* Assigning an Object to another Object's property is called **Object composition**. You are building up more complicated Objects out of simpler Objects.

* **Object Composition** can ve combined with Polymorphism to compose your Object out of Other Objects, whose(interface) type you know, but not the actual class.

* Composition can be used with a service Object, making part of its behavior configurable. It can also be used with other types of objects, like entities(models), where composition is used for related child elements.

```php

final class Order
{
    private array $lines;
    
    public function __construct(array $lines){
        $this->lines = $lines;
    }
}

```

> **Note**:
> An Order Object that contains Line Objects could use composition to establish that relationship between an Order and its lines. In that case a client might provide not a single Line Object but a collection of Line Objects.

## Class Organization

* Programming languages offer varying options for organizing classes: directories, namespaces, components, modules, packages etc. 

* Sometimes the language even offers ways to keep classes private to the module or package they are in. Just like with property and method scopes, this can help reduce the potential coupling surface between modules.

> **Warning**: 
> This book doesn't contain specific rules for organizing classes in to larger groups. It focuses on design rules for class themselves.

## Return statements and Exceptions

### Return statement

* When you call a method, it will normally be executed statement by statement from the top until a **return** statement is encountered, or the end of the method is reached.

* If at some point you want **to prevent further execution of a method**, you can insert a **return** statement, making sure the rest of the method is skipped.

```php

final class Email {

    public function send(): void
    {
        if($status !== true) {
            return;
        }
        ...
    }
    
    public function status(): bool
    {
        if($queueStatus !== true) {
            return false;
        }
        ...
    }
}

```

### Exceptions

* Another way to **stop execution** of a method is to `throw an exception`. 

* An exception is a special kind of Object that, when instantiated, collects information about where the object was instantiated and what happened before (stack trace).

* Normally an exception indicates some kind of failure, such as
  * The wrong method arguments were provided.
  * A map has no value for the given key.
  * Some external service in unreachable.

```php

final class Email {

    public function send(): void
    {
        if($status !== true) {
            throw new ResourceNotFoundException('Please provide valid entity');
        }
        ...
    }
} 

```

> A Client can recover  from the exception if it uses **try/catch** block.

```php

$email = new Email();

try {
    $email->send();
} catch(Exception $exception) {
    $this->json(['status' => $exception->getMessage()], 404);
}

```

* Programming languages comes with their own built-in set of exception classes(_Exception_). We can define our own exception classes(_ResourceNotFoundException_) by extending built-in exception classes.

```php

final class ResourceNotFoundException extends Exception {

    public function getMessage(): void
    {
        ...
    }
}

```

## Unit Testing

* In order to see whether our classes/Objects behaving as expected, we should write a script that instantiates our object, calls one of its methods and compares the result to some written expectation.

* **Unit Testing Frameworks** supports this type of Scripted Approach. A framework will look for classes of a specific type(test classes). It will then instantiate each test class, and call each of its methods that are marked as test(methods with @test annotation).

> **Note**:
> The basic structure of each test method is **Arrange-Act-Assert**.

1. **Arrange**: Bring the Object that we are testing in to certain known state.
2. **Act**: Call on of its methods.
3. **Assert**: Make some assertions about the end state.

```php

class Calculator
{

    public function addition(int $a, int $b): int
    {
        return $a + $b;    
    }

}

class CalculatorTest extends TestCase
{

    public function testAdditionReturningNumber(int $a, int $b): int
    {
        $calc = new Calculator();
        $result = $calc->addition(12, 4);
        
        $this->assertEquals(16, $result);
    }

}
```

## Dynamic Arrays

> **Dynamic Arrays** can be used to define list or maps without specifying types for its keys and values.

### Dynamic Arrays as a List

> A **list** is a collection of values with particular Order. A list can be looped over, and you can retrieve a specific value by its index(integer).

```php

$listOfColors = ['red', 'blue', 'green'];

# Looping over list
foreach ($listOfColors as $key => $value) {
    ...
}

# If key is irrelevant
foreach ($listOfColors as $color) {
    ...
}

# Retrieving the value at a specific index
$color1 = $listOfColors[0];
$color2 = $listOfColors[1];

# Adding item to the list
$listOfColors[] = 'yellow';

```

### Dynamic Arrays as a Map

> A **Map** is also a collection of values, but the values have no particular order. Instead, each value can be added to the map with particular key, which is a `string`.

```php

$mapOfCustomer = [
    'name' => 'gopi',
    'from' => 'india'
];

# Looping over a Map
foreach ($mapOfCustomer as $key => $value) {
    ...
}

# Retrieving the value at a specific index
$name = $mapOfCustomer['name'];
$country = $mapOfCustomer['from'];

# Adding item to the Map
$mapOfCustomer['profession'] = 'Software Engineer';

```