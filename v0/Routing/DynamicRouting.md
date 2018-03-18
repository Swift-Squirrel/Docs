# Dynamic routes

Dynamic routes are solutions for all routes which are based on same structure but with different parts. For example if you want to see a user with nickname `Tom` probably you want to access his informations with `/users/Tom`. This means whenever you opened page with url `/users/<anyNickname>` you want to check user for existence and returns informations about him. So any part after `/users/` is dynamic which can be stated with `:` as prefix and attribute name if it is structure otherwise attribute type. In this case it would be `/users/:string`

```swift
// We expect string value so we need to use :string
router.get("/users/:string") { (nickname: String) in
    return "getting \(nickname)'s informiatons"
}

// or with using structure 
struct User: Decodable {
    let nickname: String
    let id: Int
}

// When expect structure which conforms to Decodable protocol
// we can use structure attributes as identifier
router.post("/users/:id/:nickname") { (user: User) in
    return "Creating user with id: \(user.id) and nickname: \(user.nickname)"
}
```

In case you dont need to store dynamic value from url use `:` without identifier

```swift
router.get("/route1/:") {
    return "This is '/route/<something>'"
}

router.get("/:/route2") {
    return "We don't need to know first part of URL"
}
```

## Fallback routes

Allows you to match multiple url nesting. Symbol `*` at the end of URL can be substitued to match any route with same prefix match

```swift
router.get("/match/any/*") { (req: Request) in
    return "page: \(req.path)"
}
```

This exmaple matches

- `/match/any/`
- `/match/any/something1`
- `/match/any/something2`
- `/match/any/anything/blah`
- `/match/any/any/any/any/anything`
- `...`
