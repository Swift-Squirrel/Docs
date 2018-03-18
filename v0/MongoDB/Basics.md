# Squirrel Connector basics

Here you can learn how to store, remove and modify your classes stored in MongoDB.

## Model protocol

To make class storable in MongoDB with `SquirrelConnector` you need to conforms it with protocol `Model`. This means to add `var _id: ObjectId? = nil` in your class. Note that this attribute has prefix **_** (underscore) this means you should never change this attribute explicitli. This is **automatically** set to value when you store object into database and set back to nil when you delete object from database.

```swift
class User: Model {
    var _id: ObjectId? = nil // You need to add this line

    var name: String
    var surname: String
}
```

**_Note:_** `Model` protocol depends on `Codable` protocol. This means all your attributes needs to conforms it too.

## Working with database

At first you need to add `import SquirrelConnector` at the start of your file. Then you need to be sure you are connected to database with `Connector.setConnector(...)`. 

### Saving model

Your class can be stored to database with `.save()` method. This stores class as document to collection with lowercased name of class with 's' at the end. For example User class will be stored to collection with name users.

```swift
// init user (taylor._id is nil)
let taylor = User(name: "Taylor", surname: "Swift")

// store user to database 
try taylor.save()
// now is taylor._id with value
```

### Removing model

You can remove mobject from database with `remove()` method. This will find object in database removes it and then set `_id`  to `nil`.

```swift
// taylor._id is not nil
try taylor.remove()
//taylor._id is nil
```

### Finding model

`SquirrelConnector` provides two methods to find objects in database. 

- `find()`: will find all objects mathing some criteria and returns array
- `findOne()`: will find first object matching criteria and returns optional

```swift
// Finds all users
let users = try User.find()
print(users)

// Finds first object with name Taylor and surname Swift
let taylorSwift = try User.findOne("name" == "Taylor" && "surname" == "Swift")
```

**_Note:_** `MongoKitten` uses special syntax to build a query. `"name" == "Taylor"` will not match strings but build query witch will match all objects with attribute `name` and value `"Taylor"`. Thanks to this you can write simple to understand queries.

You can learn more about finding in **Find queries** section

### Drop all documents in collection

You can remove all documents in collection at once with `drop()` method

```swift
//
let john = User(name: "John", surname: "Smith")
try john.save()
var user = User.findOne()
// False, database is not empty, user is not nil
print(user == nil)

// Drops all users
try User.drop()

user = User.findOne()

// True, database is empty, user is nil
print(user == nil)
```
