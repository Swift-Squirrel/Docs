# HTML head and body modifying commands

These commands modify content of html head or body tag (if exists, otherwise it creates this tag).

## Title

```
\Title(<expression>)
```

Add title tag to head.

Example for data:

```swift
let data: [String: Any] = [
    "myTitle": "My Page"
]
```

```html
\Title("This is " + myTitle)
This is some content.
```

result: 

```html
<head>
    <title>This is My Page<\title>
</head>
<body>
    This is some content.
</body>
```

## Head

```
\Head(<expression>)
```

Add result of expression to html head tag. This can be useful in situation where some css is specific for one Subview or View and you don't want to include these files in header subview.

example: 

```html
\Head("<link rel=\"stylesheet\" type=\"text/css\" href=\"specific.css\">")
Adding specific css file.
```

result: 

```html
<head>
    <link rel="stylesheet" type="text/css" href="specific.css">
</head>
<body>
    Adding specific css file.
</body>
```

## Body

```
\Body(<expression>)
```

Add result of expression at the end of body tag. This can be useful in situation where some javascript is specific for one Subview or View.

example: 

```html
\Body("<script src='myjs.js'></script>")
Adding specific js file.
```

result: 

```html
<head>
</head>
<body>
    Adding specific js file.
    <script src='myjs.js'></script>
</body>
```
