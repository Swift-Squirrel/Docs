# Commands

Commands has prefix **\\** for example **\\(**_expression_**)**, **\\for**, **\\if**, **\\Title(**_..._**)**

**List of all commands**

- __\\(__*\<expression\>*__)__
- __\\RawValue(__*\<expression\>*__)__
- __\\Date(__*\<expression\>*__, format:__ *\<expression\>* = default__)__
- __\\if__ *\<expression\>* __{__, __\\if let__ *\<variable\>* __=__ *\<expression?\>* __{__
- __\\for__ *\<variable\>* __in__ *\<collection\>* __{__
- __\\Title(__*\<expression\>*__)__
- __\\Head(__*\<expression\>*__)__
- __\\View()__
- __\\Subview(__*\<expression\>*__)__
- __\\Layout(__*\<expression\>*__)__

## Expressions

Expression commands takes argument which will be evaluated and result of expression replace theese commands in final document.

### Escaped value expression - \\(*\<expression\>*)

Result of expression is checked for special html characters (\<, \>, &, ...) and theese characters will be replaced for escaped sequences. This prevents browser to evaluate result as html tags.

Example for data:

```swift
let data: [String: Any] = [
    "name": "Timmy", 
    "age": 14,
    "dataFromUser": "<script>alert(\"Buy oranges!\")</script>"
]
```

```html
<h1>\(name + " is " + String(age))</h1>

<h2>Escaped data from bad user, browser will not run script<\h2>
\(dataFromUser)
```

result:

```html
<h1>Timmy is 14</h1>

<h2>Escaped data from bad user, browser will not run script<\h2>
&lt;script&gt;alert(&quot;Buy oranges!&quot;)&lt;/script&gt;
```

### Unescaped raw value expression - \\RawValue(*\<expression\>*)

Resulted value replace command as is without any escaping.

Example for data:

```swift
let data: [String: Any] = [
    "name": "Timmy", 
    "age": 14,
    "dataFromUser": "<script>alert(\"Buy oranges!\")</script>"
]
```

```html
<h1>\RawValue(name + " is " + String(age))</h1>

<h2>Not escaped data from bad user, browser will RUN script<\h2>
\RawValue(dataFromUser)
```

result:

```html
<h1>Timmy is 14</h1>

<h2>Not escaped data from bad user, browser will RUN script<\h2>
<script>alert("Buy oranges!")</script>
```

### Date expression - \\Date(*\<expression\>*, format: *\<expression\> = default*)

For dates you can use **\\Date(_:format:)** command with optional argument **format**. If format argument is missing interpreter will use default value. This value can be changed in `NutConfig.dateDefaultFormat`.

Example for data:

```swift
let data: [String: Any] = [
    "today": Date(), 
    "someFormat": "dd mm"
]
```

```html
<h1>\Date(today)<\h1>
<h2>\Date(today, format: someFormat)</h2>
<h3>\Date(today, format: "dd mm hh")</h3>
```

result: 

```html
<h1></h1>
<h2></h2>
<h3></h3>
```

## Conditional commands

Conditional commands are used to control which parts of code should be interpreted and which not. For example *if* or *for* statements.

### If statement - \\if *\<BoolExpression\>* { ... \\}

This command will interprete commands in **{ \\}** block only if expression is true. You can use **else** or **else if** of course

Example for data:

```swift
let data: [String: Any] = [
    "ok": true, 
    "fail": false,
    "number": 6
]
```

```html
<h1>
\if number > 0 {
    \(number) is greater than 0
\} else if number < 0 {
    \(number) is lower than 0
\} else {
    \(number) is equal 0
\}
</h1>    
```

result: 

```html
<h1>
    6 is greater than 0
</h1>
```

### If let statement - \\if let *\<variable\>* = *\<expression?\>* {

If let check if expression is nil or not. Commands in body will interprete only if expression is not nil which is same as in *swift*.

Example for data:

```swift
let data: [String: Any] = [
    "nickname": "Thommas"
]
```

```html
\if let name = nickname {
    <h1>Hello \(name)</h1>
\}
\if let age = userAge {
    <h2>You are \(age)
\} else {
    <h2>I don't know your age :(</h2>
\}
```

result: 

```html
<h1>Hello Thommas</h1>
<h2>I don't know your age :(</h2>
```

### If, If let combinations

When you want to combine if and if let statements you can do it with comma 

Example for data:

```swift
let data: [String: Any] = [
    "nickname": "Thommas"
]
```

```html
\if let name = nickname, name == "Thommas" {
    <h1>Hello Thommas</h1>
\} else {
    <h1>You are not Thommas</h1>
\}
```

result: 

```html
<h1>Hello Thommas</h1>
```

### For loop statement - \\for *\<variable\>* in *\<collection\>* {

You can iterate over collection of type **Array** or **Dictionary** with **\\for** command

Example for data:

```swift
let data: [String: Any] = [
    "animals": ["cat", "dog"],
    "persons": [
        "Adam": 33,
        "Peter": 45
    ]
]
```

```html
Animals
<ul>
\for animal in animals {
    <li>\(animal)<\li>
\}
</ul>
<dl>
\for (name, age) in persons {
    <dt>\(name)</dt>
    <dd>\(age)</dd>
\}
</dl>
```
result: 

```html
<ul>
    <li>cat</li>
    <li>dog</li>
</ul>
<dl>
    <dt>Adam</dt>
    <dd>33</dd>
    
    <dt>Peter</dt>
    <dd>45</dd>
</dl>
TODO
```

## HTML head commands

Theese commands modify content of html head tag (if exists otherwise it creates this tag).

### Title - \\Title(*\<expression\>*)

Add title tag to head

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

### Head - \\Head(*\<expression\>*)

Add result of expression to html head tag. This can be useful in situation where some css or javascript is specific for one Subview or View and you don't want to include theese files in header subview

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
