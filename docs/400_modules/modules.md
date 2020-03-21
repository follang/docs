---
title: 'Modules'
type: "docs"
weight: 10
---

## Imoprting modules

### System libraries
This is how including other libraries works, for example include `fmt` module from standard library:
```
use fmt: mod[std] = {fmt};

def main: mod[init] = {
    fmt::log.warn("Last warning!...")
}
```
To use only the `log` namespace of `fmt` module:
```
use log mod[std] = {fmt::log};

def main: mod[init] = {
    pro[] main: int = {
        log.warn("Last warning!...")
    }
}
```
But let's say you only wanna use ONLY the `warn` functionality of `log` namespace from `fmt` module:
```
use warn mod[std] = {fmt::log.warn};

def main: mod[init] = {
    pro[] main: int = {
        warn("Last warning!...")
    }
}
```
---
### Local libraries
To include a local package (example, package name `bender`), then we include the folder where it is, followed by the package name (folder is where files are located, package is the name defned with mod[])

```
use bend: mod[loc] = {../folder/bender};
```
Then to acces only a namespace:
```
use space: mod[loc] = {../folder/bender::space};
```

## Definig modules
### Main module
Every file in a folder is part of a package, which means everything in the folder that uses the same package name, share the same scope. Two pakages can't exist in same folder. 
```
def shko: mod[] = {
    // implementation
}
```
If a package is an executable, it should have `pro[] main: int` to tell the compiler where to start. 
```
use log mod[std] = {fmt::log};

def shko: mod[] = {
    pro[] main: int = {
        log.warn("Last warning!...")
    }
}
```

To save the fingers and some brackets, we use a short hand of package declaration at the very top of the page (like golang):
```
def shko: mod[]

use log mod[std] = {fmt::log};

pro[] main: int {
    log.warn("Last warning!...")
}
```
