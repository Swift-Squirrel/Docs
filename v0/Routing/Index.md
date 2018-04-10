# Routing

Routing handles all requests and gives back right responses. **Swift Squirrel** provides default behaviour when serving files from *Public* and *Storage/Public* folders. For example imagine this folder structure

```
Project root
│
├── Public
│   ├── index.html
│   └── Images
│       └── squirrel.png
│
└── Storage
    └── Public
        └── file.txt

```

When you run a **Swift Squirel** app, you can access these files directly with route `127.0.0.1:8000/`,  `127.0.0.1:8000/index.html`, `127.0.0.1:8080/Images/squirrel.png`, `127.0.0.1:8000/Storage/file.txt`

## Custom routes

The most simple example is a handler which returns same response for every request to specified URI.

```swift
import Squirrel
let router = Server()

router.get("/") {
    return "Hello, World!"
}
```

First we need to construct router (the `Server` class is a router too) then we can add new routes.
**Swift Squirrel** provides 5 http methods which you can handle. 

- GET (`router.get`)
- POST (`router.post`)
- PUT (`router.put`)
- DELETE (`router.delete`)
- PATCH (`router.patch`)

In these methods you can directly access in closure

- Request data (`Request`)
- Custom data structure created from request data (`Decodable`)
- Session if exists (`Session`)
- Custom data structure created from session data (`SessionDecodable`)
- Custom data structure created from provided closure (`(Request) throws -> T`)

These overloads give you great ability to let **Swift Squirrel** check all needed parameters for you and you can work directly with parameters you need. **Swift Squirrel** provides all combinations of these parameters so you can specify only what you need.

### Response without request context

Imagine a situation when you just want to say hello to all customers who visit your site at `/hello` url. To view page content, browsers use `GET` method. This means we use `.get` method. First argument is url to handle and second is handler for given url and method.

```swift
router.get("/hello") {
    return "Hello."
}
```

### Response based on request

If you want to have access to user's request use closure with type `(Request) -> Any`. This example returns visitors Host header from request if a user provides it.

```swift
router.get("/host") { (request: Request) in
    return request.headers[.host] ?? "Host is not set"
}
```

### Response based on data

If a request contains query parameters (parameters at the end of url starting with `?`) or you are using dynamic routing you can construct structure/object from given parameters and use it in the handler.

```swift
// Note expecting type has to conforms Decodable protocol
struct Person: Decodable {
    let name: String
    let age: UInt
}

// Query parameters
// For example route 127.0.0.1:8080/person?name=Tom&age=21
route.put("/person") { (person: Person) in
    return "\(person.name) is \(person.age)"
}

// Dynamic routing
// for example route 127.0.0.1:8080/person/Tom/21
server.get("/person/:name/:age") { (person: Person) in
    return "\(person.name) is \(person.age)"
}

// Combination
// for example route 127.0.0.1:8080/person/21?name="Tom"
server.get("/person/:age") { (person: Person) in
    return "\(person.name) is \(person.age)"
}
```

If you are using post method given type in closure can be constructed from values in request body

### Response based on request and data

```swift
server.get("/user/:string") { (request: Request, user: String) in
    return "\(user) \(request.getHeader(for: .host))"
}
```

## Return types

Handler can return `Any`. This means if your return type is not `ResponseProtocol` class, **Swift Squirrel** takes these steps to parse it to `Response`

```swift
switch any {
    case let response as ResponseProtocol:
        return response
    case let string as String:
        return try Response(html: string)
    case let presentable as SquirrelPresentable:
        return try Response(presentable: presentable)
    default:
        return try Response(object: any) // json representation of object mirrors
}
```
