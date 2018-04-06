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
