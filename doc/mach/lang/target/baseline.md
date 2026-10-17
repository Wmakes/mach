# mach.lang.target.baseline

a target's baseline: the selection of masc's catalog for its instruction set the
target stands on before the extensions it adds, spelled as masc spells one (an
isa string such as `rv32imc` on riscv)

masc reads and spells the selection, closes it over the catalog's implications
and says whether the architecture enters it, so mach states none of the grammar.
mach keeps only which catalog rows its code generator emits against, as the
selection bits of the instruction set's extension table

## fun spells

```mach
pub fun spells(arch: *mink_arch.Arch) bool;
```

whether masc's catalog for mink's architecture `arch` spells a selection, as
riscv's isa strings do, so a target of it takes a `baseline`

## fun takes_baseline

```mach
pub fun takes_baseline(arch: *mink_arch.Arch, t: *extension.Table) bool;
```

whether a target of mink's architecture `arch` takes a `baseline`: masc's catalog
for it spells a selection, and `t`, the table mach's code generator emits against,
is that catalog's. aarch64's masc spells, but mach's vocabulary for it is its own
until #4779

## fun entered_by

```mach
pub fun entered_by(a: *A.Allocator, spelling: str) u32;
```

the catalog arch id of the first instruction set whose masc catalog reads
`spelling` as a selection its architecture enters, catalog_arch.UNKNOWN when none
does, as an isa written as the selection it meant names its architecture

## fun select

```mach
pub fun select(a: *A.Allocator, arch: *mink_arch.Arch, t: *extension.Table, spelling: str) res[u64, fail.Fail];
```

the bits of the extension table `t` that the selection `spelling` holds, read by
masc's catalog for mink's architecture `arch` and closed over its implications

## fun spell

```mach
pub fun spell(a: *A.Allocator, arch: *mink_arch.Arch, t: *extension.Table, bits: u64, versioned: bool) res[str, fail.Fail];
```

the spelling masc gives the rows of `t` in `bits` on mink's architecture `arch`,
with each member's revision when `versioned`, as an object's attributes state the
selection it was built for; owned by `a`

