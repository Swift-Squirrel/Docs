# Sessions

HTTP is stateless protocol this means HTTP provide any method to hold context and share it across all clients requests. Thanks to session you can hold data across multiple requests and simulate states. When you start a session **Swift Squirrel** stores browsers informations from request (due to security if this information is missing in request header session can not be established) and generate uniq session ID which sends in response as cookie with key `SquirrelSession`. User with disabled cookies can not hold session.

## Session middleware

You need to add `SessionMiddleware()` in route middlewares to use sessions.

```swift
let router = Server(globalMiddlewares: [SessionMiddleware()])

// or
router.group(middlewares: [SessionMiddleware()]) { router in
    ...
}

```

## Start session

A session can be established with `newSession()` method called on `Request`. This means you need to have `Request` class in handling closure. `newSession()` sets flag `isNew` (you should never change value of `isNew`). Thanks to this flag `SessionMiddleware` sends `SquirrelSession` cookie.

```swift
router.get("/session/start") { (request: Request) in
    let session = try request.newSession()
    return "Session start successful"
}
```

## Get a session

```swift
router.get("/session") { (request: Request) in
    var session = try request.session()
    return "Session exists"
}
```

### Set/get session data

At the start of file add `import SquirrelJSON`.

Session data is a dictionary of type [String: JSON] where JSON is **SquirrelJSON** representation.

```swift
import SquirrelJSON

...

server.get("/session") { (request: Request) in
    var session = try request.session()

    session["nickname"] = JSON("Tom")

    let name = session["nickname"]!.string!
    return name
}

```

## Stop session

To stop a session and remove all data set `shouldRemove` flag to true and `SessionMiddleware` takes care of everything important.

```swift
router.delete("/session/delete") { (request: Request) in
    var session = try request.session()
    session.shouldRemove = true
    return "Session removed"
}
```
