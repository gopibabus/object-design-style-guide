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

> Defining properties and assigning values

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

> Defining constants

```php

class EmailClient
{
    public const string CLIENT_NAME;
    public const int PORT_NUMBER;
}

$port = EmailClient::PORT_NUMBER;
```

> Mutable vs Immutable Objects

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
