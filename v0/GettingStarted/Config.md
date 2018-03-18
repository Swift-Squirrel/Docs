# Configuration file

You can partialy configure Swift Squirrel framework from *squirrel.yml* file in project root directory. This file is automatically loaded after you run your application. All attributes in *squirrel.yml* are optional and you can skip them.

example of *squirrel.yml*

```yaml
---
server:
  domain: example.com
  serverRoot: /Path/To/Server/
  port: $PORT
```

**_Note:_** You can use `$NAME` to load enviromental variables.

- `domain`: domain name used with cookies (default: 127.0.0.1)
- `serverRoot`: path to server (default: current directory)
- `port`: Server port (default: 8000)

## SquirrelConfig class

Swift Squirrel configuration is stored in `squirrelConfig`. To acces this class you need to import `SquirrelConfig` in your code. For example this code will prints server's port

```swift
import SquirrelConfig

// prints port 
print(squirrelConfig.port)
```

`SquirrelConfig` attributes

- `session`: Direcotry with sessions (*Storage/Sessions*)
- `storage`: Storage directory (*Storage*)
- `domain`: Domain name
- `logFile`: Logging file (*Storage/Logs/server.log*)
- `serverRoot`: Server root directory (Current diretory)
- `webRoot`: Public directory visible for clients (*Public*)
- `cache`: Directory for cache (*Storage/Caches*)
- `port`: Server port
- `storageViews`: Storage used for NutView (*Storage/Fruits*)
- `publicStorage`: Public storage, client can access to files in this directory with route /Storage/* (*Storage/Public*)
- `views`: View files directory (*Resources/NutViews*)
