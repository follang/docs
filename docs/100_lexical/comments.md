---
title: "Comments"
type: "docs"
weight: 300
---

## Normal comments

Comments in FOL code follow the general Rust/C++ style of line (`//`) and block (`/* ... */`) comment forms and are interpreted as a form of whitespace.

{{% placeholder %}}

LINE_COMMENT :
`// (~[/ !] | //) ~\n*` | `//`

BLOCK_COMMENT :
`/* (~[* !] ) ( ~*/)* */` | `/**/`

{{% /placeholder %}}


## Docs comments

Doc comments beginning with exactly three slashes (`///`) are interpreted as a special syntax for doc attributes. 

{{% placeholder %}}

DOC_COMMENT:
`/// (~/ ~[\n]*)?`

{{% /placeholder %}}
