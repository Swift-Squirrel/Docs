# Find queries

You will often need to find objects matching some criteria. Sometimes whole object and sometimes you need to just some attributes from this object. `SquirrelConnector` provides elegant way to do this typo-safe.

```swift
let players: PlayerProjection = Player.find("age" > 50, sortedBy: ["score", .ascending], 
                                            skipping: 100, limitedTo: 40)
```

## Query

When you want to find something in database you often needs to specify searching criteria. This is called query. We are using queries provided by `MongoKitten`. This allow you to create easy to understand queries. `MongoKitten` overloads few operators and this give us possibility to use these operators in standard and intuitive way.

If you want to create query which will find all users with score higher then 10 you can just wirte `"score" > 10` and this will build our query. When we want to matching attributes we can use dot convention and write `"nameInfo.name" == "Johny"`.

```swift
class Player: Model {
    struct NameInfo: Codable {
        var name: String
        var surname: String
    }
    var _id: ObjectId? = nil
    var username: String
    var score: Int
    var nameInfo: NameInfo
}

let q1: Query = "score" > 10 && "score" < 25
let players = try Player.find(q1)

let q2: Query = "nameInfo.name" == "Johny"
let johnys = try Player.find(q2)
```

## Sorting

You can set sorting with `Sort` struct. This can be initialized with dictionary where key is attribute name and value is enum of type `SortOrder`

```swift
let s: Sort = ["score": .ascending, "nameInfo.surname": .descending]

let sortedPlayers = try Player.find(sortedBy: s)
```

## Skiping and limiting

Sometimes you need to get objects with offset and sometimes you don't want to get more than N objects. This can be done with `skipping:` and `limitedTo:` parameters

```swift
// finds 20 players with offset 10
let somePlayers = try Player.find("score" > 15, skipping: 10, limitedTo: 20)
```

## Projection

To return just few attributes istead of whole object from database you just need to set result type of variable where you want to store result of `find` or `findOne` and `SquirrelConnector` takes care of everything. Result type need to conforms to `Projectable`. This means result type must initializable with no parameters (`init()`)

```swift
struct NameInfoScore: Projectable {
    // attributes names must be same as attributes we want to get
    let score: Int
    let nameInfo: Player.NameInfo
    
    init() { // needed by Projectable protocol
        score = 0
        nameInfo = Player.NameInfo(name: "", surname: "")
    }
}
// returns projection of object
let nameInfoScore: NameInfoScore? = try findOne("username" == "lllllll")
print(nameInfoScore)
```

## Distinct
Finds the distinct values for a specified attribute across an object and returns the results in an array.

```swift
let distinctValues: [String] = Player.distinct(on: "username", filtering: "score" < 20)
```
