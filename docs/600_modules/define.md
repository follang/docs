---
title: 'Define'
type: "docs"
weight: 10
---

Every file in a folder is part of a package, which means everything in the folder that uses the same package name, share the same scope. To define a package we use:
```
def package_name: mod[] = {
    // implementation
}
```
## Main module

If a package is an executable, it should have `pro[] main: int` to tell the compiler where to start. 
```
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
