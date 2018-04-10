# Middlewares

Middleware is a handler which can be used to filter or modify user's `Request` before it is passed to your route handler.

## Swift Squirrel middlewares

**Swift Squirrel** provides few default middlewares.

### Session

SessionMiddleware handles all session logic as sending cookie with session id to user and checks if session exists for request.

```swift
SessionMiddleware()
```

### Page protection

ProtectedPageMiddleware adds `no-cache` header to response which stops browser from storing pages in history. This means you should not be able to use back button in your browser if your page is protected with this middleware.

```swift
ProtectedPageMiddleware()
```


## Defining custom middleware

You can define your own middleware just with implementing one method.

```swift
public protocol Middleware {
    func respond(to request: Request, next: AnyResponseHandler) throws -> Any
}
```

For example if you have API and you want to put your API version to header of all responses your code may be similar to this:

```swift
struct APIVersionMiddleware: Middleware {
    let version = "1.0"
    func respond(to request: Request, next: (Request) throws -> Any) throws -> Any {
    
        // getting result from next middleware/handler
        let anyResponse = try next(request)    
        
        // getting Response class from anyResponse
        let response = try Response.parseAnyResponse(any: anyResponse)
        
        // setting Version header
        response.setHeader(for: .version, to: version)
        return response
    }
}
```

## Using middleware

Middlewares can be used in `Router.group(middlewares:routes:)`

```swift
let server = Server() // Server init

server.group(middlewares: [APIVersionMiddleware()]) { (router) in

    // use APIVersionMiddleware
    router.get("/posts") {
        return "Something"
    }

    // chaining middlewares: APIVersionMiddleware + AnotherMiddleware1 + AnotherMiddleware2
    router.group(middlewares: [AnotherMiddleware1(), AnotherMiddleware2()]) { (router) in
    
        router.post("/posts/another") {
            return try Response(json: "{\"name\":\"Tom\"}")
        }
    }
}
```

You can set global middleware while constructing `Server`

```swift
let server = Server(globalMiddlewares: [APIVersionMiddleware()])
```


