[title: Build your first plugin II]
[next: Relying-on-configuration-features]
[previous: A-quick-events-overview]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">Build your</span> first plugin (II)


### Intro

Welcome back to "Build Your First Plugin" (BYFP)!
[br]

In this lesson we're delving into the technical intricacies of events. Events serve as the backbone of our plugin architecture, enabling seamless communication between different components.
[br]

In this lecture, we'll harness the power of events to implement two crucial features: the "die-on-chance" mechanic and player greetings. By leveraging events, we'll be able to execute these functionalities with precision and efficiency.
[br]

### Implementing events

```csharp
/// <summary>
/// It defines a generic handler for in-game events.
/// </summary>
internal class GenHandler
{
    /// <summary>
    /// Fired when a player has joined the server.
    /// </summary>
    internal void OnVerified(VerifiedEventArgs ev)
    {
        // We're displaying a broadcast to the player for 7 seconds.
        ev.Player.Broadcast(7, $"Hello {ev.Player.Nickname}, welcome to my server!");
    }
    
    /// <summary>
    /// Fired after a player interacts with a door.
    /// </summary>
    internal void OnInteractingDoor(InteractingDoorEventArgs ev)
    {
        // We're calculating a chance of 15.0 between 0.0 and 100.0 (15%).
        if (UnityEngine.Random.Range(0f, 100f) > 15f)
            return; // We haven't hit the 15% chance, nothing will happen.
    
         // We hit the 15% chance, the player will die.
        ev.Player.Kill("Bad luck buddy!");
    }
} 
```
[br]

### Un/Subscribing events

```csharp
/// <summary>
/// It defines the plugin's entrypoint.
/// </summary>
public class GreetingsAndKilling : Plugin<Config>
{
    /// <inheritdoc>
    internal GenHandler GenHandler { get; private set; }
 
    /// <inheritdoc>
    public override string Name => "Greetings and Killing";
    
    /// <inheritdoc>
    public override string Prefix => "G&K";
    
    /// <inheritdoc>
    public override string Author => "NaoUnderscore";
    
    /// <inheritdoc>
    public override Version Version => new Version(1, 0, 0);
    
    /// <inheritdoc>
    public override Version RequiredExiledVersion => new Version(9, 0, 0);
 
    /// <inheritdoc>
    public override void OnEnabled()
    {        
        base.OnEnabled();
    }
    
    /// <inheritdoc>
    public override void OnDisabled()
    {
        base.OnDisabled();
    }
 
    /// <inheritdoc>
    protected override void SubscribeEvents()
    {
        GenHandler = new();
 
        Exiled.Events.Handlers.Player.Verified += GenHandler.OnVerified;
        Exiled.Events.Handlers.Player.InteractingDoor += GenHandler.OnInteractingDoor;
    }
 
    /// <inheritdoc>
    protected override void UnsubscribeEvents()
    {
        Exiled.Events.Handlers.Player.Verified -= GenHandler.OnVerified;
        Exiled.Events.Handlers.Player.InteractingDoor -= GenHandler.OnInteractingDoor;
    }
}
```


### Recap

We've successfully subscribed to events, marking a significant milestone in our plugin development journey. With events in place, our plugin is now primed and ready to run – well, almost.
[br]

Before we can hit the ground running, there's one more crucial step: defining a configuration class. This class will allow us to customize our plugin's behavior, making it more versatile and user-friendly. But don't worry, we'll tackle this task head-on in the next lecture.