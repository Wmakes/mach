# mach.lang.target.extension

the extension vocabulary of an instruction set

an instruction set's extensions are masc's: its catalog names every extension
and level and says what each implies, and mach states only which rows of it the
code generator emits against and the selection bit each carries. every surface
that spells an extension reads the catalog: the manifest's `[target.X].extensions`,
`$mach.build.extensions.<name>`, `#[extensions(...)]` and the inline-asm refusals.
a name is a key of the catalog's name index, an identifier, so it can be a comptime
member and a field of a runtime feature record. a bit is the isa's own: the same bit
means different things on different instruction sets, and only a name crosses
between them. spir-v's vocabulary names vulkan device features, which no masc
catalog states, so its table states its names itself until one does

## rec Extension

```mach
pub rec Extension;
```

one extension the code generator emits against

handle: its row of the table's masc catalog
name: in a table with no catalog, the name it is spelled by, nil otherwise
bit: the bit it carries in a selection
implies: in a table with no catalog, the bits it brings, 0 otherwise
target_only: nil when a function may admit the extension alone with
             `#[extensions]`; otherwise why only a target selects it

## rec Table

```mach
pub rec Table;
```

the extensions an instruction set's code generator emits against

source: where its names are read from, for a diagnostic
catalog: masc's catalog of the instruction set's extensions, nil for a vocabulary
         no masc catalog states
rows: the extensions, each a row of the catalog
count: how many
float: the bits a value of a floating-point width needs to be held in float
         registers, nil when every selection holds them

## def FloatRequirementFn

```mach
pub def FloatRequirementFn: fun(u32) u64
```

the bits of a table a floating-point value of `bits` bits needs held

## fun float_absent

```mach
pub fun float_absent(t: *Table, bits: u32, selected: u64) u64;
```

the bits a floating-point value of `bits` bits needs that `selected` lacks, 0
when it lacks none

## val MAX_SELECTED

```mach
pub val MAX_SELECTED: u32 = 64
```

the most rows a table holds: one per bit

## fun row_name

```mach
pub fun row_name(t: *Table, i: u32) str;
```

the name of row `i` of `t`

## fun row_of

```mach
pub fun row_of(t: *Table, name: str, len: usize) *Extension;
```

the row of `t` the name `name[0..len]` names alone, nil when it names none of them

## fun known

```mach
pub fun known(t: *Table, name: str, len: usize) bool;
```

whether `name[0..len]` is a key of the table's catalog, or in a table with no catalog
the name of a row

## fun bit_of

```mach
pub fun bit_of(t: *Table, name: str, len: usize) u64;
```

the bits `t` gives the extension `name[0..len]` names, 0 when it names none the code
generator emits against or names a level

## fun level_of

```mach
pub fun level_of(t: *Table, name: str, len: usize) u64;
```

the bits of the members of the level the catalog names `name[0..len]`, 0 when it
names none

## fun close

```mach
pub fun close(t: *Table, bits: u64) u64;
```

`bits` closed over what each selected extension implies: every extension a selected
one brings, to a fixpoint. a selection is closed once, at target resolution, so a
level is a bundle of bits and never an axis of its own

## fun admitted

```mach
pub fun admitted(selected: u64, function: u64) u64;
```

the extensions a body may emit: the target's selection and whatever its
`#[extensions(...)]` admits beyond it. one predicate for the inliner and the
encoder: an instruction needing `required` is emitted only into a body whose
admitted set holds it

## fun admits

```mach
pub fun admits(admitted_bits: u64, required: u64) bool;
```

## fun domain

```mach
pub fun domain(t: *Table) u64;
```

every bit the table names

## fun first_name

```mach
pub fun first_name(t: *Table, bits: u64) str;
```

the first name the table gives any bit of `bits`, nil when it names none

## fun is_name

```mach
pub fun is_name(name: str, len: usize) bool;
```

an extension name is spelled by the lexer's own rules: one identifier token,
never a keyword, so every surface that names it parses it the same way

## fun is_spelling

```mach
pub fun is_spelling(name: str, len: usize) bool;
```

what a manifest's `extensions` entry may spell: a name or a level

## fun table_valid

```mach
pub fun table_valid(t: *Table) bool;
```

a table is well formed: every bit a single nonzero bit, no bit or row repeated, every
row of its catalog and named by an identifier, and in a table with no catalog every
implied bit another row of the table

## fun levels_append

```mach
pub fun levels_append(b: *textbuild.TextBuilder, t: *Table) err[textbuild.Error];
```

the catalog's levels whose members the code generator emits against any of,
`, `-separated, appended to `b`

## fun bits_append

```mach
pub fun bits_append(b: *textbuild.TextBuilder, t: *Table, bits: u64) err[textbuild.Error];
```

the names of every row whose bit is in `bits`, `, `-separated, appended to `b`

## fun names_append

```mach
pub fun names_append(b: *textbuild.TextBuilder, t: *Table) err[textbuild.Error];
```

the table's names, `, `-separated, appended to `b`

## val VOCABULARY_CAP

```mach
pub val VOCABULARY_CAP: u32 = 8
```

the most vocabularies a description carries: one per registered instruction set

## rec Vocabulary

```mach
pub rec Vocabulary;
```

every extension vocabulary the registered instruction sets declare, each table
once in registration order: what a name in `#[extensions(...)]` may mean beside
the selected instruction set's own table

## fun vocabulary_empty

```mach
pub fun vocabulary_empty() Vocabulary;
```

## fun vocabulary_add

```mach
pub fun vocabulary_add(v: *Vocabulary, t: *Table) bool;
```

`t` joins `v` unless it is empty or already there; false when `v` is full

## fun sources_append

```mach
pub fun sources_append(b: *textbuild.TextBuilder, v: *Vocabulary) err[textbuild.Error];
```

where the names of every table of `v` are read from, `, `-separated

## fun vocabulary_append

```mach
pub fun vocabulary_append(b: *textbuild.TextBuilder, v: *Vocabulary) err[textbuild.Error];
```

the names of every table of `v`, `, `-separated

