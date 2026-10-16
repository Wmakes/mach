# `def` — type alias

`def` introduces a new name for an existing type. The alias and the
underlying type are interchangeable; there is no nominal distinction.

## Grammar

```mach fragment
def NAME: TYPE;
#[DECORATOR(...)] def NAME;
```

## Examples

```mach
pub def Age:    i64; # alias for a primitive
pub def BinOp:  fun(i64, i64) i64; # alias for a function type
pub def Anon:   rec { x: i64; y: i64; }; # inline record
pub def Choice: uni { a: i64; b: f64; }; # inline union
```

Aliases may name any type: primitives, pointers, arrays, function types,
records, unions, tags, or other aliases. `def` is a module-scope declaration;
there is no function-scope alias. An alias of a tag constructs, tests and
copies as the tag:

```mach
use std.types.result.res;

tag ParseError: u8 { invalid; overflow; }
def R: res[i64, ParseError];

fun parse(x: i64) R {
    if (x < 0) { ret R.err{ParseError.invalid{}}; }
    ret R.ok{x};
}
```

## A `def` with no type

A `def` that states no type is a declaration whose type something else
supplies: a decorator that defines it. `#[handle(...)]` declares a type the
owning target mints ([types.md](types.md#handles)), and `#[abi_type(...)]` a C
type whose layout the target declares ([types.md](types.md#abi-types)):

```mach fragment
#[abi_type("va_list")]
pub def VaList;
```

Without such a decorator, `def NAME;` names nothing and is refused at the
declaration (`def.no_type`). The refusal is the only diagnostic it produces:
a use of the name is not reported again.

## Stdlib aliases

Mach has no compiler-known type aliases. Names like `usize` and `str` live
in stdlib as ordinary `def`s — `def usize: u64;` (or `u32`, target-
conditional via `$if`) and `def str: *u8;`. A module that wants the
shorthand imports the appropriate stdlib module.

## See also

- [types.md](types.md) - the type grammar def references
- [rec.md](rec.md), [uni.md](uni.md), [tag.md](tag.md) - aggregate forms commonly aliased
