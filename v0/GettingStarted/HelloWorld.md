# Hello, World!

We are going to create simple web application to greet every client who visits our pages. First we create project, set **Swift Squirrel** as dependency then write our *main.swift* which will contains **just 6 lines**.

### Content of *main.swift*

```swift
// import Swift Squirrel framework
import Squirrel

// Create server
let server = Server()

// Add HTTP get listener
server.get("/") {
    return "Hello!"
}

// Run server
try server.run()
```

## Create project

First we need to create a new directory where we will develop our app. Create it and enter it.

```sh
$ mkdir HelloWorld && cd HelloWorld
```

To create swift project run this command:

```sh
$ swift package init --type executable
```

you should see something like this

```
Creating executable package: HelloWorld
Creating Package.swift
Creating README.md
Creating .gitignore
Creating Sources/
Creating Sources/HelloWorld/main.swift
Creating Tests/
```

### Set up dependencies

At this time you have an empty package without dependencies. You need to add Squirrel as dependency so open *Package.swift* in text editor (vim, nano, sublime, ...) and change content to this:

```swift
// swift-tools-version:4.0

import PackageDescription

let package = Package(
    name: "HelloWorld",
    dependencies: [
        .package(url: "https://github.com/Swift-Squirrel/Squirrel.git", from: "1.0.0"),
    ],
    targets: [
        .target(
            name: "HelloWorld",
            dependencies: ["Squirrel"]),
    ]
)
```

Now we need to resolve our dependencies with:

```sh
$ swift package resolve
```

### Generate Xcode project

If you are on Mac OS you can generate Xcode project with:

```sh
$ swift package generate-xcodeproj
```

## Setting up App

At this time we have everything we need to create our app. Open *Sources/HelloWorld/main.swift*, remove all its content and at the first line write:

```swift
import Squirrel
```

This tells swift that we want to use functions from the `Squirrel` package in this file.

### Server

The `Squirrel` provides server class which can listen at given port and respond to clients requests. 

You can init `Server` with four arguments

```swift
Server(base: String = "/",
    port: UInt16 = 8000,
    serverRoot: Path = "Public",
    globalMiddlewares: [Middleware] = [])
```

- `base` - Prefix of all routes used with this `Server` instance, default is "/"
- `port` - Port to bind and listen for requests, default value is from `SquirrelConfig` class (8000)
- `serverRoot` - Server root directory, default value is from `SquirrelConfig` (Public)
- `globalMiddlewares` - Array of middlewares used on all requests

As you see all arguments have default value. This means we can ignore them and init server with `Server()`

Add these lines under `import Squirrel`

```swift 
let server = Server()
```

### Request handling

We need to tell to `server` which urls and HTTP methods are valid for our app. In this example we want to accept requests only at root url (`/`) and allow only HTTP `GET` method. To all request which match these condition we want to respond with **Hello!**. This can be done with `Server` method

```swift
func get(_ url: String, 
    middlewares: [Middleware] = [], 
    handler: @escaping () throws -> Any)
```

Method name `get` represents allowed HTTP method. The first argument is valid url, second array of Middlewares applied at all request mathing HTTP method and url and the last is closure representing handler. Result from this closure is used as response for a client.

Add this under `let server = Serve()`

```swift
server.get("/") {
    return "Hello!"
}
```

### Start listening for requests

We configured our `server` to handle GET requests at root url. Now we need to start listening for requests. Add this at the end of your file:

```swift
try server.run()
```

Now *main.swift* contains everything to handle requests. Your code should be similar to code mentioned at the top of this page.

## Run server app

You can build and run your application with typing `swift run` into console or run project in Xcode by changing target to `HelloWorld` at right left next to stop button and then press play button. This can take a while if you are building app first time. It's because all downloaded dependencies needs to be built too. But don't worry. Next build will be swift.

```sh
$ swift run
```

This will run a server and print to console something like this:

```
19:52:12.755 INFO Config.createDir():233 - creating folder: /HelloWorld/Storage/Logs
19:52:12.760 INFO Config.createDir():233 - creating folder: /HelloWorld/Public
19:52:12.760 INFO Config.createDir():233 - creating folder: /HelloWorld/Storage/Fruits
19:52:12.760 INFO Config.createDir():233 - creating folder: /HelloWorld/Resources/NutViews
19:52:12.760 INFO Config.createDir():233 - creating folder: /HelloWorld/Storage/Cache
19:52:12.760 INFO Config.createDir():233 - creating folder: /HelloWorld/Storage/Public
19:52:12.761 INFO Config.createDir():233 - creating folder: /HelloWorld/Storage/Sessions
19:52:12.761 DEBUG RouteTree.add():44 - Adding route for method GET in route: /
19:52:12.761 INFO Server.run():68 - Server is running on port 8000
```
First lines inform you about creating important files. Second line from the botton informs you that server will handle `GET` methods at `/` and the last tells you at which port is server listening.

Now open your browser and visit site: `127.0.0.1:8000`. You should see `Hello!` message sent from our server. Lovely <3 To stops server use CTLR+C (console) or in Xcode stop button (CMD+.)
