# NutView Cache

To make `NutView` fast as possible we are using Cache. You can change cache setting in `NutConfig.NutViewCache`.

attributes and functions:

- `defaultName`: default cache name (FruitsCache)
- `setNutViewCache(name:config:)`: set up cache name and configuration
- `totalDiskSize`: total disk size
- `name`: Cache name
- `path`: Path to cache
- `clear(keepingRootDirectory:)`: Clears caches
- `clearExpired()`: Clears expired caches

```swift
import NutView

// Prints cache name
print(NutConfig.NutViewCache.name)
