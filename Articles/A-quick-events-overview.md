[title: A brief summary about Events]
[next: Build-your-first-plugin-II]
[previous: Build-your-first-plugin-I]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">A brief summary</span> about events


### Intro

Events in C# are a fundamental part of the language's event-driven programming paradigm. They allow objects to communicate with each other by signaling when certain actions or conditions occur. In simple terms, an event is a message sent by an object (the sender) to notify other objects (the receivers) that something significant has happened. Events play a crucial role in facilitating interactions between different components of the game and plugins.
[br]

### Event definition

In Exiled, events are typically defined as delegates with specific signatures. These delegates act as templates for defining the structure of events.
[br]
For example, an event to notify when a player joins the server might be defined as follows:
```csharp
public static Event<VerifiedEventArgs> Verified { get; set; } = new();
```
[br]

### Event registration

Objects interested in receiving notifications about specific events can subscribe to them. In Exiled, plugins often subscribe to events using event handlers. 
[br]

For example, a plugin might subscribe to the `Verified` to perform certain actions whenever a player joins the server:
```csharp
// Context: Plugin<T>
public override void SubscribeEvents()
{
    Exiled.Events.Handlers.Player.Verified += OnVerified;
}
 
// Context: Generic
abstract void OnVerified(VerifiedEventArgs ev);
```
[br]

### Event triggering

When the conditions for an event occur, the object responsible for triggering the event (the event sender) invokes the event. In Exiled, the framework handles the triggering of events based on game events or specific conditions.
[br]
For example, when a player joins the server, Exiled triggers the `Verified` event, notifying all subscribed plugins:
```csharp
public static void OnVerified(VerifiedEventArgs ev) => Verified.InvokeSafely(ev);
```
[br]

### Event handling

When an event is triggered, all subscribed event handlers (methods) are executed in the order they were subscribed. This allows objects to respond to events and perform specific actions accordingly.
[br]
In Exiled, plugins define event handler methods to respond to events:
```csharp
private void OnVerified(VerifiedEventArgs ev)
{
    Log.Info($"{ev.Player.Nickname} has joined the server.");

    // Perform actions when a player joins the server
}
```
[br]

### Conclusion
Overall, events in C# and the Exiled framework provide a powerful mechanism for decoupling components, enabling flexible and extensible application designs. They allow plugins to react to various game events and perform actions accordingly, enhancing the gameplay experience for players and administrators alike.
