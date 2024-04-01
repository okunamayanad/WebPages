 
[title: A Quick API Overview]
[next: Build-your-first-plugin-I]
[previous: Setting-up-the-environment]
[authors: naounderscore]
[date: 29/03/2024]

## <span class="md-span">A quick</span> API overview


### Introduction to Exiled Framework

Exiled is a comprehensive framework that offers a multitude of classes designed to streamline plugin development processes. These classes serve various purposes, from facilitating interactions with the base game's elements to providing tools for plugin development.
[br]

### Core Components

The cornerstone of Exiled is the `API::Core`, a robust structure that underpins many functionalities within the framework. Although intricate, we'll delve deeper into its intricacies later on. Additionally, dynamic event dispatchers play a vital role in handling events and delegates efficiently, allowing for seamless event management. However, a detailed discussion on these will follow in subsequent sections.
[br]

### Key Classes

Two primary classes merit our immediate attention:
`Plugin<T>` and `Player`.
[br]

#### Player Class

The `Player` class represents the entities actively engaged in the game, essentially the players themselves. It encapsulates essential player-related functionalities, offering a convenient interface for interacting with player data and actions.
[br]

#### Plugin<T> Class

The `Plugin<T>` class empowers developers to create custom plugins by deriving from it. This class not only makes the plugin executable but also ensures its validity, providing a structured approach to plugin development. 
[br]

### Extension Methods

Exiled provides a plethora of extension methods tailored to streamline operations with various types, both native and custom. These methods enhance the developer's workflow by offering simplified yet powerful functionalities for common tasks.
[br]

### Enums

In addition to classes and extension methods, Exiled furnishes a collection of enums designed to augment project development. These enums cater to diverse needs, such as role assignments, effects, door states, and more. They offer a standardized approach to handling enumerations, promoting consistency and clarity in codebases.
[br]

### The `Plugin<T>` Class

The `Plugin<T>` class offers various overridable properties and methods crucial for plugin functionality:
[br]

#### Properties:

- `Name`: The name of the plugin.
- `Prefix`: A short name prefix for the plugin.
- `Author`: The author of the plugin.
- `Version`: The version of the plugin.
- `RequiredExiledVersion`: The minimum required version of Exiled to run the plugin.
- `IgnoreRequiredVersionCheck`: A flag to ignore the required Exiled version check.
[br]

#### Methods:

- `void OnEnabled()`: Fired when the plugin is enabled.
- `void OnDisabled()`: Fired when the plugin is disabled.
- `void OnReloaded()`: Fired when the plugin is reloaded.
[br]

To ensure the plugin is validated and loaded correctly, it's crucial to set the appropriate values for these properties and implement the methods correctly. Additionally, when overriding these methods, ensure to call the default implementation at the end.

```csharp
public class MyPlugin : Plugin<MyConfig>
{
}
```

```csharp
public override string Name => "MyPlugin"; // The name of the plugin

public override string Prefix => "MP"; // Prefix for MyPlugin

public override string Author => "NaoUnderscore"; // Author name

public override Version Version => new Version(1, 0, 0); // Plugin version

public override Version RequiredExiledVersion => new Version(9, 0, 0); // Minimum required Exiled version

// We are not going to override IgnoreRequiredVersionCheck as it's set to false by default
```

```cs
public override void OnEnabled()
{
    Log.Info($"{Name} has been enabled!");
    
    base.OnEnabled(); // Call default implementation
}

public override void OnDisabled()
{
    Log.Info($"{Name} has been disabled!");

    base.OnDisabled(); // Call default implementation
}
```

### The `Player` Class

The `Player` class is a powerhouse with a plethora of properties and methods, making it a cornerstone for interacting with players in your game. While it's impossible to cover everything here due to its extensive nature, we highly recommend exploring the XML documentation or generated docs for comprehensive insights into its capabilities.

With the `Player` class, you wield complete control over player interactions. Each member is intuitively named, making it easy to grasp its purpose. Here are a few examples:

- `Player::IsInventoryFull`: Check if the player's inventory is full.
- `Player::IsInPocketDimension`: Determine if the player is currently in a pocket dimension.
- `Player::RemoveHandcuffs()`: Free the player from handcuffs.
- `Player::DropItem(Item, bool)`: Drop an item from the player's inventory.
[br]Build-your-first-plugin-I

Rest assured, each member is extensively documented, providing clear explanations of its functionality and usage.
[br]

In the future, we'll delve into more advanced topics such as derived classes from `Player` and defining custom `Player` classes to enhance integration with custom and private features.
[br]

### Conclusion

In essence, Exiled presents a robust framework equipped with essential tools and components for plugin development. By leveraging its classes, extension methods, and enums, developers can expedite development processes while ensuring code integrity and functionality.
