[title: Build your first plugin I]
[next: A-quick-events-overview]
[previous: A-quick-API-overview]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">Build your</span> first plugin (I)


### Intro

Welcome to the "Build Your First Plugin" (BYFP) series! Our goal? To walk you through creating your very first plugin from scratch.
[br]

In this series, we'll start by building a plugin that greets players as they join the server. But wait, there's more! We'll also add a fun twist: players might encounter a surprise when interacting with doors – and we'll even introduce a command for players to meet their fate if they dare.
[br]

What makes this series special? Well, we're all about customization. Every aspect of our plugin will be configurable, giving you full control over its behavior. Get ready to dive into the world of plugin development and harness the power of various API features.
[br]

### Defining an executable plugin

```csharp
/// <summary>
/// It defines the plugin's entrypoint.
/// </summary>
public class GreetingsAndKilling : Plugin<Config>
{
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
}
```

### Implementing API features

```csharp
/// <summary>
/// It defines a generic handler for in-game events.
/// </summary>
internal class GenHandler
{
    /// <summary>
    /// Fired when a player has joined the server.
    /// </summary>
    internal void ___(???, Player player) // We're going to keep the method signature hidden until the next BYFP chapter.
    {
        // We're displaying a broadcast to the player for 7 seconds.
        player.Broadcast(7, $"Hello {player.Nickname}, welcome to my server!");
    }
    
    /// <summary>
    /// Fired after a player interacts with a door.
    /// </summary>
    internal void ___(???, Player player)
    {
        // We're calculating a chance of 15.0 between 0.0 and 100.0 (15%).
        if (UnityEngine.Random.Range(0f, 100f) > 15f)
            return; // We haven't hit the 15% chance, nothing will happen.
    
         // We hit the 15% chance, the player will die.
        player.Kill("Bad luck buddy!");
    }
} 
```

### Recap

With the structure in place, our plugin is shaping up nicely and ready to hit the ground running – well, almost. There's just one tiny detail: the method signatures are deliberately hidden. Why, you ask? Simple. We want you to flex those brain muscles and apply what you've learned from our previous lectures. After all, practice makes perfect!
[br]

But fear not, we've got your back. We've also introduced a handy Unity Engine method for generating random numbers between 0.0 and 100.0. Pretty neat, right? Now, onto the fun part: making everything configurable. From the chance, to messages and other values, it's all up for customization.
[br]

And what's next on the agenda? Events, my friend. Get ready to dive deep into the world of events in our next lecture.