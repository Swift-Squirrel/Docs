# HTTP Abstraction

Swift Squirrel provides abstraction over HTTP protocol. HTTP protocol is based on two types of messages. Request message from client to server and response message from server to client. When you creating routes you can access to request message with argument in closure. To create response message, you can return anything or construct `Response` class.

```swift
import Squirrel

let router = Server()
router.get("/") { (request: Request) in
    // You can access request sent from client
    print(request.headers)
    
    // returning request as response
    return request
}
```

