# Expressions

Expression commands take argument which will be evaluated and result of expression replace theee commands in final document.

## Escaped value expression - \\(*\<expression\>*)

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

## Unescaped raw value expression - \\RawValue(*\<expression\>*)

Resulted value replaces command as is without any escaping.

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

## Date expression - \\Date(*\<expression\>*, format: *\<expression\> = default*)

For dates you can use **\\Date(_:format:)** command with optional argument **format**. If format argument is missing, the interpreter will use default value. This value can be changed in `NutConfig.dateDefaultFormat`.

Example for data:

```swift
let data: [String: Any] = [
    "today": Date(), 
    "someFormat": "dd MMM"
]
```

```html
<h1>\Date(today)</h1>
<h2>\Date(today, format: someFormat)</h2>
<h3>\Date(today, format: "dd mm hh")</h3>
```

result: 

```html
<h1>Feb 23 2018</h1>
<h2>23 Feb</h2>
<h3>23 17:05</h3>
```
