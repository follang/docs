---
title: 'Behaviour'
type: "docs"
weight: 20
---

### Build-in functions
Fol has some build-in functions offered by compiler, and you access them by . (with space/newline/bracket before):

```
var contPoint: ptr[int] = 10;			// make a pointer and asign the memory to value of 10
.print(.pointer_value(contPoint));		// print the dereferenced value of pointer
```


### Macro system
are a very complicated system, and yet can be used as simply as in-place replacement. A lot of build-in macros exist in the language to make the code more easy to type. Below are some system defined macros. 

For example, wherever `$` is before `any` variable name, its replaced with `.to_string`. Or wherever `!` is before `bol` name, its replaced with `.not` but when the same `!` is placed before `ptr` it is replaced with `.delete_pointer`.

```
def '$'(a: any): mac = '.to_string'
def '!'(a: bol): mac = '.not '
def '!'(a: ptr): mac = '.delete_pointer';
def '*'(a: ptr): mac = '.pointer_value';
def '#'(a: any): mac = '.borrow_from';
def '&'(a: any): mac = '.address_of';
```

### Alternatives
Alternatives are used when we want to simplify code. For example, define an alternative, so whenever you write `+var` it is the same as `var[+]`.
```
def '+var': alt = 'var[+]'
def '~var': alt = 'var[~]'
def '.pointer_content': alt = '.pointer_value'
```

### Defaults

Defaults are a way to change the default behaviour of options. Example the default behaviour of `str` when called without options. By defalt `str` is it is saved on stack, it is a constant and not public, thus has `str[pil,imu,nor]`, and we want to make it mutable and saved on heap by default:
```
def 'str': def[] = 'str[new,mut,nor]'
```
### Templates
Templates are supposed to be mostly used for operator overloading. They are glorified functions, hence used with `pro` or `fun` instead of `def`. 

For example here is how the `!=` is defined: 
```
fun '!='(a, b: int): bol = { return .not(.eq(a, b)) }

.assert( 5 != 4 )
```

or define `$` to return the string version of an object (careful, it is `object$` and not `$object`, the latest is a macro, not a template):
```typ[pub] file: obj 
pro (file)'$': str = { return "somestring" }

.echo( file$ )
```
*WITH ALTERNATIVES, MACROS AND TEMPLATES, YOU CAN COMPLETELY MAKE A NEW TYPESYSTEM, WITH ITS OWN KEWORSD AND BEHAVIOUR. HOWEVER, THIS IS NOT SUGGESTED, RELAYING HEAVILY ON IT, THE CODE MIGHT LOOSES THE READABILITY WHEN SOMEONE ELSE WANTS TO USE.*
