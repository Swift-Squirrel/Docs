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

## Commands
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
