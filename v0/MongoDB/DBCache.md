# SquirrelConnector Cache

To make `SquirrelConnector` fast as possible we are using Cache. You can change cache setting in `SquirrelConnectorCache`.

attributes and functions:

- `defaultName`: default cache name (ProjectionCache)
- `setProjectionCache(name:config:)`: set up cache name and configuration
- `totalDiskSize`: total disk size
- `name`: Cache name
- `path`: Path to cache
- `clear(keepingRootDirectory:)`: Clears caches
- `clearExpired()`: Clears expired caches

```swift
import SquirrelConnector

// Prints cache name
print(SquirrelConnectorCache.name)
