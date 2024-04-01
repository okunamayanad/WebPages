[title: Relying of configuration features]
[next: Build-your-first-plugin-III]
[previous: Build-your-first-plugin-II]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">Relying on</span> configuration features


### Intro

In Exiled, configuration files (configs) are the backbone of plugin customization, allowing developers to fine-tune plugin behavior without diving into the code.
[br]

Alongside these configs, the `IConfig` interface plays a crucial role in defining the structure and behavior of configuration classes. Let's dive into how both work together.
[br]

### Config definition

Each plugin defines its configuration options by creating a class that represents the structure of the config.
[br]

This class contains properties that correspond to different settings the plugin supports.

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
    public bool IsEnabled { get; set; }
 
    /// <summary>
    /// Gets or sets a value indicating whether debug messages should be displayed in the console or not.
    /// </summary>
    [Description("Whether or not debug messages should be shown in the console.")]
    public bool Debug { get; set; }    

    /// <summary>
    /// Gets or sets the chance of getting a greeting message.
    /// </summary>
    [Description("The chance of getting a greeting message.")]
    public float GreetingMessageChance { get; set; }
} 
```
[br]

### Automatic loading

When the plugin initializes, Exiled automatically loads the config file associated with it.
Exiled handles the loading process behind the scenes, so developers don't need to worry about manually loading configs.
[br]

Once loaded, the plugin can access the configuration settings through a config object provided by Exiled.
These settings can be used to customize the plugin's behavior at runtime, such as determining the chance of a player receiving a greeting message.

```csharp
public void GreetPlayer(Player player)
{
    if (UnityEngine.Random.Range(0f, 100f) < Config.GreetingChance)
    {
        player.Broadcast(Config.GreetingMessageChance);
    }
}
```
[br]

### Modification and saving

- Server administrators can modify the config file directly or through in-game commands provided by the plugin.
- Exiled automatically detects changes to the config file and updates the config object accordingly.
- If the plugin allows users to modify settings in-game, Exiled also handles saving the updated configuration back to the YAML file automatically.
[br]

### Conclusion

Overall, Exiled's automatic config loading simplifies the plugin development process and enhances the user experience by seamlessly managing config loading, usage, modification, and saving operations.