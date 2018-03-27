# Squirrel JSON wrapper

**SquirrelJSON** is package already in **Squirrel**. This package provides you wrapper over JSON, encoding and decoding functions. 

```swift
import SquirrelJSON
```

## JSON struct

You can initialize JSON with literars or with many of `.init()` options. Then you can acces to properties or encode JSON to `Data`.

### Inits

You can init JSON from dictionary, array or another literal.

```swift
import SquirrelJSON

let jsonDictionary: JSON = [
    "name": "Tom",
    "age": 22
]
```

Init from object

```swift
struct User {
    let name: String
}
let asd: JSON? = JSON(from: User(name: "Tom"))
```

Init from `Encodable` object

```swift
let users = [
    User(name: "Tom"),
    User(name: "Bob"),
    User(name: "Tim")
]

let usersJSON = try JSON(users)
```

### Getting attributes

You can directry get these attributes

- `string`
- `dictionary`
- `array`
- `int`
- `double`
- `bool`
- `date`

These attributes returns optionals. If you want to get value directly use postfix `Value`, for example `stringValue`, `boolValue`. If value can not be represented in expected type this will return default value.

Decoding JSON to object can be achieved with `decode(to:)` function.

```swift
struct Paper: Codable {
    let color: String
    let size: Int
}

let paper = Paper(color: "white", size: 10)
let json = try JSON(paper)
let optionalDic = json.dictionary
let dic = json.dictionaryValue
print(dic["color"].stringValue) // white
print(dic["size"].int) // optional 10

let paperFromJSON = try json.decode(to: Paper.self)
print(paperFromJSON)
```
