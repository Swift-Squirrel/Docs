# Stream

You can stream data to client with `StreanResponse` class. All you need to do is return this class from route and call `send` method in closure to stream data.

## Inits

`StreamResponse` takes 4 parameters.

- `status`: HTTP status (default: .ok)
- `contentType`: Content type (default: nil)
- `headers`: HTTP header used in response
- `streamClosure`: Closure where you send data in stream

```swift
// Response without body
server.get("/stream") {
    let streamResponse = StreamResponse(status: .ok, contentType = .html) { stream in
        try stream.send("This ")
        try stream.send("is ")
        try stream.send("sent ")
        try stream.send("in ")
        try stream.send("chunks ")
    }
    return streamResponse
}
```

## HTTP header

Response class has an attribute `headers` of type `HTTPHead` which holds all headers for the client. You can access them with subscript.

```swift
response.headers["location"] = "/foo"
```
typo-safe way

```swift
respone.headers[.location] = "/foo"
```

or you can use `set` method

```swift
response.headers.set(to: .location(location: "/foo"))
```

## Cookies

Cookies are used to hold context between HTTP requests. You can set value to key and browser will stores this information and send it to you with every request.

```swift
response.setCookies("cartID", to: "1234567890")
response.cookies(for: "cartID")
```
