# Page layout

You can divide your page to multiple files and use these commands to join all files into final page.

## Views
Contains page specific content. For example if we have a blog and we want to have page with certain post our view will contain only post information and not layout, header, footer etc...

*Views/Index.nut.html*

```html
\Layout("Default")
\Title("Hi " + \(name)")
<h1>Hi \(name + " " + surname)</h1>
```

## Subview
Contains reusable parts of page. For example header or footer.

*Subviews/Head.nut.html*

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="/some.css">
<script src="/js/some.js"></script>
```

## Layout
This is actually our web layout. You can refer to layout from Views which pin View content to Layout at the place where `\View()` is.

*Layouts/Default.nut.html*

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
