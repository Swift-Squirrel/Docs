# Installation

Swift Squirrel depends just on **swift 4.0**. If you already have swift, check version with `swift --version` in console. If your version is lower or you don't have swift, bellow is all you need for successful installation.

## Swift 4.0

1. Download and install swift compiler from [swift.org](https://swift.org/getting-started/#installing-swift)
2. After installation check version with `swift --version`

## Openssl

- Mac: Openssl is installed
- Ubuntu: run `sudo apt-get install openssl libssl-dev` in terminal

## SquirrelToolbox

**Squirrel Toolbox** is set of useful commands which help you to be effective while developing application based on **Swift Squirrel**.

### Prerequisites
- **git**
    - Mac OS: `brew install git`
    - Ubuntu: `sudo apt-get install git`
- Write permissions to `/usr/local/bin`

### Installation
Open terminal/console and write

```sh
git clone https://github.com/Swift-Squirrel/Squirrel-ToolBox.git
cd Squirrel-ToolBox
make install
./install
```

Type `squirrel help`. You should see help informations which means installations was successful.

**_Note:_** This creates new directory which can be now removed. 

## Checklist

1. `swift --version` prints version 4.0
2. `squirrel help` prints help

From this time you can fully using and enjoy **Swift Squirrel**
