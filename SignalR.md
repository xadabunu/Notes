# SignalR

## Côté serveur

On implémente une classe qui étend la classe abstraite `Hub`

```cs
using Microsoft.AspNetCore.SignalR;

public class MyHub : Hub
{
    public async Task ServerMethod(string s1, string s2)
    {
        await Clients.All.SendAsync("ClientMethod", s1, s2);
    }
}
```

On déclare et implémente ici les méthodes qui seront utilisées depuis nos pages et composants pour utiliser les `web sockets`

`Clients.All.SendAsync` permet de prévenir toutes les connexions d'invoquer la méthode `ClientMethod`.

On définit l'url de notre Hub dans `Program.cs` :

```cs
app.MapHub<MyHub>("/huburl");
```

## Côté page/composant

### En-tête

```cs
@using Microsoft.AspNetCore.SignalR.Client
@inject NavigationManager Navigation
@implements IAsyncDisposable
```

Il est nécessaire d'implémenter `IAsyncDisposable` ainsi que d'implémenter la fonction `DisposeAsync`

```cs
public async ValueTask DisposeAsync()
{
    if (hubConnection is not null)
    {
        await hubConnection.DisposeAsync();
    }
}
```

### Attribut

```cs
private HubConnection? hubConnection;
```

### OnInitializedAync

##### HubConnection

On crée une `HubConnection` en lui précisant l'url du `endpoint` mappé dans  `Program.cs`

```cs
hubConnection = new HubConnectionBuilder()
    .WithUrl(Navigation.ToAbsoluteUri("/huburl"))
    .Build();
```

##### Gestion du message

On définit le comportement du client quand `ClientMethod` est invoqué

```cs
hubConnection.On<string, string>("ClientMethod",
        (s1, s2) =>
    {
        /**
         * gestion des données reçues par le socket
         */
        InvokeAsync(StateHasChanged);
    });
```

##### Lancement connexion

```cs
await hubConnection.StartAsync();
```

## Communication avec le serveur

```cs
if (hubConnection is not null)
{
    await hubConnection.SendAsync("ServerMethod", arg1, arg2);
}
```

`SendAsync` permet d'invoquer la méthode `ServerMethod` du côté du serveur avec les arguments à passer à cette méthode.

## État de connection

```cs
private bool IsConnected =>
        hubConnection?.State == HubConnectionState.Connected;
```
