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

