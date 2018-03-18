# Request

The request holds all data sent from client to server.

## HTTP header

Request class has an attribute `headers` of type `HTTPHead` which holds all headers from the client. You can access them with subscript.

```swift
let host = request.headers["host"]
```
or typo-safe way

```swift
let host = request.headers[.host]
```

## Parameters

Swift Squirrel supports three types of parameters. First is taken from url if you specify some parts as dynamic in routing function .

```swift
// for example /users/421
router.get("/users/:id") { (request: Request) in
    let urlParameters: [String: String] = request.urlParameters
    print(urlParameters["id"]!)
    // or
    let id = request.getURLParameter(for: "id")!
    print(id)
    return id
}
```

Another type is query parameters taken from url query.

```swift
// for example /query?name=Tom&age=41&adult
router.get("/query") { (request: Request) in
    // note value is optional because query parameter doesn't have to have value
    let queryParameters: [String: String?] = request.queryParameters
    print(queryParameters)
    
    guard let name = request.getQueryParameter(for: "name") else {
        return "No name"
    }
    return "Hello \(name)"
}
```

Last type are post parameters. This has values only if request is http post and content-type is `application/x-www-form-urlencoded` or `multipart/form-data`.

```swift
router.post("/post") { (request: Request) in
    guard let contentType = request.headers[.contentType] else {
        throw HTTPError(status: .badRequest)
    }
    
    if contentType == .formUrlencoded { // application/x-www-form-urlencoded
        let postParameters = request.postParameters
        print(postParameters)
        guard let name = request.getPostParameter(for: "name") else {
            return "No name"
        }
        return name
    } else if contentType == .formData { // multipart/form-data
        let parts: [String: Multipart] = request.postMultipart
        return parts
    } else {
        throw HTTPError(status: .unsupportedMediaType)
    }
}
```

## Cookies

To get cookies sent with request use

- `func getCookie(for:) -> String?`
- `var cookies: [String: String] { get }`

## Other informations

- HTTP method: `request.method`
- acceptEncoding: `request.acceptEncoding`
- path without queries: `request.path`
- original path from request: `request.originalPath`
