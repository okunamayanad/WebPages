[title: Build your first plugin III]
[next: Commands-and-permissions]
[previous: Relying-on-configuration-features]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">Build your</span> first plugin (III)


### Intro

Welcome back to "Build Your First Plugin" (BYFP)!
[br]

In this installment, we're taking our plugin development to the next level by integrating configuration features provided by Exiled. By doing so, we'll make our plugin more flexible and customizable without the need for manual code changes, saving valuable time and effort.
[br]

Gone are the days of recompilation and tedious code tweaks. With Exiled's configuration solution, we can effortlessly fine-tune our plugin's behavior to suit various server environments, all with just a few simple configuration adjustments.
[br]

So, let's dive right in and explore how to harness the power of configuration to enhance our plugin development experience!
[br]

### Defining a `IConfig` object

We're adding three values:
- `GreetingBroadcast`
- `DeadlyInteractionChance`
- `DeadlyInteractionReason`

```csharp
/// <summary>
/// Defines the contract for basic config features.
/// </summary>
public class Config : IConfig
{
    /// <summary>
    /// Gets or sets a value indicating whether the plugin is enabled or not.
    /// </summary>
    [Description("Whether or not this plugin is enabled.")]
    public bool IsEnabled { get; set; } = true;
  
    /// <summary>
    /// Gets or sets a value indicating whether debug messages should be displayed in the console or not.
    /// </summary>
    [Description("Whether or not debug messages should be shown in the console.")]
    public bool Debug { get; set; } = false;  
 
    /// <summary>
    /// Gets or sets the <see cref="Broadcast"/> to be shown when a player joins the server.
    /// </summary>
    [Description("The broadcast to be shown when a player joins the server.")]
    public Broadcast GreetingBroadcast { get; set;} = new(7, $"Hello %Player%, welcome to my server!"); // %Player% will be replaced with the actual player's name.
 
    /// <summary>
    /// Gets or sets the chance of getting a deadly interaction with doors.
    /// </summary>
    [Description("The chance of getting a deadly interaction with doors.")]
    public float DeadlyInteractionChance { get; set; } = 15f;
 
    /// <summary>
    /// Gets or sets the deadly interaction death's reason.
    /// </summary>
    [Description("The deadly interaction death's reason.")]
    public string DeadlyInteractionReason { get; set; } = "Bad luck buddy!";
} 
```

### Implementing a `IConfig` object

- We're going pass the config instance from the plugin's main class, or the entrypoint.

```csharp
/// <summary>
/// It defines a generic handler for in-game events.
/// </summary>
internal class GenHandler
{
    private Config config;

    /// <summary>
    /// Initializes a new instance of the <see cref="GenHandler"/> class.
    /// </summary>
    internal GenHandler(Config instance)
    {
        // We're going pass the config instance from the plugin's main class, or the entrypoint.
        config = instance;
    }

    /// <summary>
    /// Fired when a player has joined the server.
    /// </summary>
    internal void OnVerified(VerifiedEventArgs ev)
    {
        Broadcast message = config.GreetingBroadcast;
 
        // We're replacing %Player% with the actual player's name, if present in the broadcast's content.
        message.Content.Replace("%Player%", ev.Player.Nickname);
 
        ev.Player.Broadcast(message);
    }
    
    /// <summary>
    /// Fired after a player interacts with a door.
    /// </summary>
    internal void OnInteractingDoor(InteractingDoorEventArgs ev)
    {
        // We're calculating a chance of the specified value between 0.0 and 100.0 (Config::DeadlyInteractionChance).
        if (UnityEngine.Random.Range(0f, 100f) > config.DeadlyInteractionChance)
            return; // We haven't hit the specified chance, nothing will happen.
    
         // We hit the specified chance, the player will die.
        ev.Player.Kill(config.DeadlyInteractionReason);
    }
} 
```
[br]

### Instantiating a `IConfig` object

- Let's instantiate the `IConfig` and pass it to the `GenHandler` constructor.

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
        // We're passing the Config instance to our GenHandler.
        GenHandler = new(Config);
  
        Exiled.Events.Handlers.Player.Verified += GenHandler.OnVerified;
        Exiled.Events.Handlers.Player.InteractingDoor += GenHandler.OnInteractingDoor;
    }
 
    /// <inheritdoc>
    protected override void UnsubscribeEvents()
    {
        Exiled.Events.Handlers.Player.Verified -= GenHandler.OnVerified;
        Exiled.Events.Handlers.Player.InteractingDoor -= GenHandler.OnInteractingDoor;
 
        // We're going to free unused memory.
        GenHandler = null;
    }
}
```


### Recap

In our journey through the "Build Your First Plugin" series, we've accomplished a significant milestone: making our plugin entirely configurable and dynamic. With this achievement, we've unlocked a world of possibilities, allowing server administrators to fine-tune the plugin's behavior without the need for code changes.
[br]

Everything is almost ready to go – we've covered all bases except for one crucial component: the command implementation as planned in these series. But fear not, because in the next lecture, we'll dive deep into implementing commands and permissions. This final step will ensure that our plugin is not only loadable and executable but also user-friendly and fully equipped to enhance the server experience.
[br]

So, get ready to put the finishing touches on our project and witness the culmination of our efforts in creating a powerful and versatile plugin!