# RabbitMQ

## 1 - Mise en place

### Créer une `connexion`

```cs
var factory = new ConnectionFactory
{
    HostName = "localhost",
    UserName = "guest",
    Password = "guest"
};

using var connection = factory.CreateConnection();
```

ou

```cs
var factory = new ConnectionFactory();

using var connection = factory.CreateConnection("localhost");
```

### Créer un `channel`

```cs
using var channel = connection.CreateModel();
```

### Déclarer une `Queue`

```cs
channel.QueueDeclare(
    queue: "firstexample",
    durable: false,
    exclusive: false,
    autoDelete: true
);
```

## 2 - Envoi d'un message

### Encoder un `message` en `Bytes`

```cs
const string message = "this is my first message";

var bytesMessage = Encoding.UTF8.GetBytes(message);
```

### Publier un `message` : `BasicPush`

```cs
channel.BasicPublish(
    exchange: string.Empty,
    routingKey: "firstexample",
    basicProperties: null,
    body: bytesMessage
);
```

## Réception d'un message

### Créer un `consumer`

```cs
var consumer = new EventingBasicConsumer(channel);
```

### Gérer la réception d'un `message` : `BasicConsume`

```cs
consumer.Received += (_, eventArgs) =>
{
    var body = eventArgs.Body.ToArray();
    var message = Encoding.UTF8.GetString(body);

    Console.WriteLine($"Received message: {message}");
};
```

### Consommer les `messages` : `BasicConsume`

```cs
channel.BasicConsume(
    queue: "firstexample",
    autoAck: true,
    // -> false pour gérer manuellement les accusés de réception
    consumer: consumer
);

channel.BasicAck(deliveryTag: eventArgs.DeliveryTag, multiple: false);
```

## 4 - Performance / Optimisation de la réception

### Répartir les `messages` entre les `consumers` : `BasicQos` (quality of service)

```cs
channel.BasicQos(prefetchSize: 0, prefetchCount: 1, global: false);
```

> `prefetchSize`: taille max message 0 = pas de limite
> `prefetchCount` : nombre de `messages `reçu
> `global` : le réglage est-il pour tous les `Consumers`

### Lier une `Queue` à un `Exchange` : `QueueBind`

```cs
channel.QueueBind(
    queue: "customer_queue",
    exchange: "app_exchange",
    routingKey: "order"
);
```

### Concurrence des `Consumers`

Nombre de `Consumer` concurrent : `ConsumerDispatchConcurrency`

```cs
var numberOfCores = Environment.ProcessorCount;

factory.ConsumerDispatchConcurrency;

using var connection = factory.CreateConnection("localhost");
```

### `async` consumer

Si le `consumer` exécute une lambda async

```cs
factory.DispatchConsumersAsync = true;

// ...

var consumer = new AsyncEventingBasicConsumer(channel);

consumer.Received += async (_, eventsArgs) =>
{
    var body = eventArgs.Body.ToArray();
    var message = Encoding.UTF8.GetString(body);

    await SimulerTraitementLong();

    channel.BasicAck(ea.DeliveryTag, false);
};
```
