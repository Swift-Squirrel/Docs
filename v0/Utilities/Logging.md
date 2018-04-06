# Logging

You can use `log` variable for custom logging. All these logs are stored in file *Storage/Logs/server.log*

## Log destinations

We are using two destination. Console destination (`consoleLog`) with minimum information level `.debug` and file destination (`fileLog`) with minimum information level `.verbose`. You can modify minimum level and add or remove filters. You can add new destinations in `log` with `log.addDestination(yourNewDestination)`

```swift
import Squirrel
import SquirrelConfig

squirrelConfig.consoleLog.minLevel = .info

log.verbose("Verbose log")
log.debug("Debug log")
log.info("Info log")
log.warning("Warning log")
log.error("Error log")
```
