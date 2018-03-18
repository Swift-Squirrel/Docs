# MongoDB Connector

It is commont to have your application connected with some kind of database. Swift Squirrel provides you simple connector for MongoDB. This does not mean you can not use any other database provider such as MySQL or different MongoDB connector. SquirrelConnector is abstraction over [MongoKitten](https://github.com/OpenKitten/MongoKitten). To use SquirrelConnector you need to setup dependencies in your *Package.swift*.

## Installation

In your *Package.swift* add this line into `dependencies: [ ]`

```swift
.package(url: "https://github.com/Swift-Squirrel/Squirrel-Connector.git", from: "0.1.12")
```

In console use command `swift package update` to download dependencies. After resolving dependencies you can import package with 

```swift
import SquirrelConnector
```


## Setup connector

Before you use connector you need to connect your application with database. This can be done with one of three provided functions.

```swift
Connector.setConnector(url:)
// or
Connector.setConnector(host: port: dbname:)
// or
Connector.setConnector(username: password: host: port: dbname:) 
```

If this function return True it means your application is succesfully connected to database and you can use it.
