---
title: "Keywords"
type: "docs"
weight: 100
---

Fol has a number of restricted groups of keywords:

## BK (build-in keywords)

{{% placeholder %}} 

BK_AS              `as`
BK_IN              `in`
BK_IS              `is`
BK_DO              `do`
BK_GO              `go`

BK_OR              `or`
BK_AND             `and`

BK_IF              `if`
BK_ELSE            `else`
BK_FOR             `for`
BK_CASE            `case`
BK_LIKE            `like`
BK_TYPE            `type`
BK_LOOP            `loop`
BK_NONE            `none`

BK_THIS            `this`
BK_SELF            `self`

BK_BREAK           `break`
BK_RETURN          `return`
BK_YEILD           `yeild`
BK_PANIC           `panic`
BK_REPORT          `report`
BK_CHECK           `check`
BK_ASSERT          `assert`

BK_TRUE            `true`
BK_FALSE           `false`

{{% /placeholder %}}


{{% placeholder %}}

BUILD-IN KEYWORDS - BK:
`(BK_AS|BK_IN|...)`

{{% /placeholder %}}


## AK (assignment keywords)

{{% placeholder %}} 

AK_USE             `use`
AK_DEF             `def`
AK_VAR             `var`
AK_FUN             `fun`
AK_PRO             `pro`
AK_TYP             `typ`

{{% /placeholder %}}


{{% placeholder %}}

ASSIGNMENT KEYWORDS - AK:
`(AK_USE|AK_DEF|...)`

{{% /placeholder %}}



## TK (type keywords)

{{% placeholder %}} 

TK_INT             `int`
TK_FLT             `flt`
TK_CHR             `chr`
TK_BOL             `bol`

TK_ARR             `arr`
TK_SEQ             `seq`
TTKVEC             `vec`
TK_SET             `set`
TK_MAP             `map`
TK_MAT             `mat`

TK_STR             `str`
TK_NUM             `num`

TK_OPT             `opt`
TK_MUL             `mul`
TK_ANY             `any`

TK_PTR             `ptr`
TK_ERR             `err`
TK_NON             `non`

TK_REC             `rec`
TK_LST             `lst`
TK_ENM             `enm`
TK_UNI             `uni`
TK_CLS             `cls`

TK_STD             `std`
TK_MOD             `mod`
TK_BLK             `blk`

{{% /placeholder %}}

{{% placeholder %}}

TYPE KEYWORDS - TK:
`(TK_INT|TK_FLT|...)`

{{% /placeholder %}}



{{% notice warn %}}

Note that all of the **type keywords** are of three characters long. It is recomanded that new identifiers not to be of the same number of characters, as one day in the future that same identifier can be used s a keyword in FOL compiler.

{{% /notice %}}

## OK (option keywords)

{{% placeholder %}}

OK_PUB              `pub`
OK_EXP              `exp`

{{% /placeholder %}}


{{% placeholder %}}

OPTION KEYWORDS - OK:
`((OK_PUB|OK_EXP|...),?)*`

{{% /placeholder %}}

## Assigning

{{% placeholder %}}

`(`*WS*`)*(\W)?(`*AK*`)(\[(`*OK*`)?\])?`

`(`*WS*`)*(`*AK*`)`
| `(`*WS*`)*\W(`*AK*`)`
| `(`*WS*`)*(`*AK*`)(\[\])`
| `(`*WS*`)*\W(`*AK*`)(\[\])`
| `(`*WS*`)*(`*AK*`)(\[(`*OK*`)\])`
| `(`*WS*`)*\W(`*AK*`)(\[(`*OK*`)\])`

{{% /placeholder %}}
