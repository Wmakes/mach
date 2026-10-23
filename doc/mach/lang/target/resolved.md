# mach.lang.target.resolved

the description of a resolved target: plain facts the front end, the
middle end and every backend read, importing nothing from the backend. the
member vtables the description was resolved from stay with the backend's
binding (target.binding)

## def AsmReturnsFn

```mach
pub def AsmReturnsFn: fun(str) bool
```

whether an asm body returns from the function that holds it

## def IntImmFitsFn

```mach
pub def IntImmFitsFn: fun(u64, u32) bool
```

whether one instruction materializes the integer `value`, read at `bits`
(at most 64): the rule the middle end hoists a loop's constants by

## rec Target

```mach
pub rec Target;
```

## rec Nans

```mach
pub rec Nans;
```

the nan a target's float operations return in each format, binary16, binary32
and binary64 at the index of mirl's format, which mirl's folder reads

## fun nans_none

```mach
pub fun nans_none() Nans;
```

no format's rule stated, as where no target is selected

## fun nans_of_rule

```mach
pub fun nans_of_rule(r: float.NanRule) Nans;
```

the rule of a target cell no owner assembles yet, read from mach's own (#4779)

## fun nans_at

```mach
pub fun nans_at(n: *Nans, w: float.FloatWidth) mirl_capability.Nans;
```

the rule of the format of width `w`, binary64 for any other width

## fun nans_held

```mach
pub fun nans_held(n: *Nans) bool;
```

whether any format's rule is stated, as it is wherever a target is selected

## fun layout_machine

```mach
pub fun layout_machine(tgt: *Target) layout.Machine;
```

the layout machine of a resolved target. total over what resolve produces,
which always selects an instruction set: a nil target is a caller defect

## fun dit_mode_guaranteed

```mach
pub fun dit_mode_guaranteed(tgt: *Target) bool;
```

whether the target guarantees the data-independent-timing mode a DIT_MODE
row needs: mink's host of the target declares it

## fun ct_mul_admitted

```mach
pub fun ct_mul_admitted(tgt: *Target) ct.CtMulMask;
```

the constant-time multiply cells this target admits: the one decision the
lowering gate, the oblivious validators and `$mach.build.ct_mul` all read

## rec Facts

```mach
pub rec Facts;
```

what a program may ask of its target while it compiles: the catalog ids, the machine's
widths and float support, the va_list shape, the instruction and type table, the
constant-time multiply cells it admits, the extensions it selects and every vocabulary
they are read against, what a float conversion makes of a NaN, and whether `asm` reaches
an assembler

## fun facts

```mach
pub fun facts(tgt: *Target) Facts;
```

## fun facts_none

```mach
pub fun facts_none() Facts;
```

the facts where no target is selected: an 8-byte pointer, 128-bit vectors, floats, and no
table, extension, multiply cell or NaN rule to read

## fun ct_mul_dit_cells

```mach
pub fun ct_mul_dit_cells(tgt: *Target) ct.CtMulMask;
```

the cells the instruction set declares only under its data-independent-timing
mode, whether or not the os guarantees the mode: a secret multiply in one of
them is what makes a program need the mode at start, and what the refusal
names when the os declares nothing

