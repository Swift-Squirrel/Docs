# Response

As response you can use `Any` object or `Response` class. This is way to send any data to client.

## Inits

```swift
// Response without body
let r11 = Response(status: .ok)

// With headers set in constructor

let r21 = Response(status: .ok, headers: ["version": "1.0"], body: data)
let r22 = Response(status: .ok, headers: [.contentType(.png)], body: pngData)

// Response from file
let r31 = try Response(pathToFile: Path("~/files/privateImage.jpg")

// JSON
let r41 = try Response(json: "{\"status\":\"ok\"}")

struct Person: Encodable {
    let name: String
}
let r42 = try Response(encodable: Person(name: "Tom"))

// Force download
let r51 = Response(download: fileData, name: "Best file.txt")
let r52 = try Response(download: Path("~/files/fileToDownload")
```

If you want to use custom object as response just use `SquirrelPresentable` protocol

```swift
import SquirrelCore

struct Person {
    let name: String
    let age: UInt
}

extension Person: SquirrelPresentable {
    var representAs: Representation {
        return .html
    }
    func present() throws -> Data {
        let str = "<h1>\(name)</h1><h2>\(age)</h2>"
        guard let data = str.data(using: .utf8) else {
            throw DataError(kind: .dataDecodingError(string: str))
        }
        return data
    }
}

let response = try Respone(presentable: Person(name: "Bob", age: "43"))
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
response.cookies["cartID"] = "1234567890"
```
