# NutView

NutView is simple view concept where you divide every page to smaller reusable parts. 

- **View** - main content different for every page
- **Layout** - page layout which is same on multiple pages
- **Subview** - reusable parts used by multiple **Views** and **Layouts**

## Installation

In your *Package.swift* add this line into `dependencies: [ ]`

```swift
.package(url: "https://github.com/Swift-Squirrel/NutView.git", from: "1.0.2")
```

In console use command `swift package update` to download dependencies. After resolving dependencies you can import package with 

```swift
import NutView
```

## Directory structure

NutView use two important directories. 

- First (default name: "**Nuts**") contains another three subdirectories with *.nut* files (*Views*, *Layouts*, *Subviews*). In theese three directories you can add another directories or add and edit *.nut* files.
- Second (defualt name: "**Fruits**") contains generated files from your *.nut* files. (Don't change content of this directory)

You can change this directories with

```swift
NutConfig.nuts = "SomeDir/SomeAnotherDir1/NutsDir"
NutConfig.fruits = "SomeDir/SomeAnotherDir2/FruitDir"
```

## Views
Contains page specific content. For example if we have blog and we want to have page with certain post our view will contain only post information and not layout, header, footer etc...

*Views/Index.nut*

```html
\Layout("Default")
\Title("Hi " + \(name)")
<h1>Hi \(name + " " + surname)</h1>
```

## Subview
Contains reusable parts of page. For example header or footer.

*Subviews/Head.nut*

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="/some.css">
<script src="/js/some.js"></script>
```

## Layout
This is actualy our web layout. You can refer to layout from Views which pin View content to Layout at the place where `\View()` is.

*Layouts/Default.nut*

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        \Subview("Head")
    </head>
    <body>
        \View()
    </body>
</html>
```

## Usage

Using NutView is really simple.

```swift
import NutView

struct User: Codable {
    let name: String
    let surname: String
}

server.get("/:name/:surname") { (user: User) in
    return try View("Index", with: user)
}
```

For this code and files mentioned above is possible result this: 

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="/some.css">
        <script src="/js/some.js"></script>
    </head>
    <body>
        <h1>Hi Taylor Swift</h1>
    </body>
</html>
```
