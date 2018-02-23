# Control flow statements

Conditional commands are used to control which parts of code should be interpreted and which not. For example *if* or *for* statements.

## If statement - \\if *\<BoolExpression\>* { ... \\}

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

## If let statement - \\if let *\<variable\>* = *\<expression?\>* {

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

## If, If let combinations

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

## For loop statement - \\for *\<variable\>* in *\<collection\>* {

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
