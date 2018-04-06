# Sessions

HTTP is stateless protocol this means HTTP provide any method to hold context and share it across all clients requests. Thanks to session you can hold data across multiple requests and simulate states. When you start a session **Swift Squirrel** stores browsers informations from request (due to security if this information is missing in request header session can not be established) and generate uniq session ID which sends in response as cookie with key `SquirrelSession`. User with disabled cookies can not hold session.

## Session middleware

You need to add `SessionMiddleware()` in route middlewares to use sessions. You should have only one instance of SessionMiddleware().

```swift
let sessionMiddleware = SessionMiddleware()
let router = Server(globalMiddlewares: [sessionMiddleware])

// or
router.group(middlewares: [sessionMiddleware]) { router in
    ...
}
```

### Customize session middleware

You can init `SessionMiddleware` with two parameters

- sessionBuilder: `SessionBuilder` used to creating and removing sessions. Use default session builder if nil (default: `nil`)
- clearAfterEvery: When given time expires clear all expired sessions. You can set this to `.never`, `.days(_)` or `.seconds(_)` (default: `.days(1)`)

You can change attribute `clearAfter` to change next scheduling (scheduled clean will not be modified with this)

You can clear expired session whenever you want with `clearExpiredSessions()` method.


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

## Session configuration

You can configure session attributes in `SessionConfig` class.

- `sessionName`: session cookie name (default: `"SquirrelSession"`)
- `defaultExpiry` = session expiration in seconds (default: `604800` - one week)
