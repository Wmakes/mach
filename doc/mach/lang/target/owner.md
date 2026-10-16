# mach.lang.target.owner

the owners' row each mach target means

a mach target cell, an operating system, an instruction set and a calling
convention, names one target row of mirl's, which is the tuple of rows the
lower layers own: mink's architecture and system, mirl's convention and masc's
baseline. the cell states that tuple as its owners spell it and mirl resolves
it, so mach states which row it means and none of the row's facts. a cell no
owner has a row for yet keeps mach's own facts until one does (#4779)

## fun row_for

```mach
pub fun row_for(a: *A.Allocator, os_id: u32, arch_id: u32, abi_id: u32) res[opt[*mirl_target.Target], fail.Fail];
```

the mirl row the cell of `os_id`, `arch_id` and `abi_id` means, none for a cell no
owner has a row for yet

## fun scalars_of

```mach
pub fun scalars_of(a: *A.Allocator, row: *mirl_target.Target) res[layout.Scalars, fail.Fail];
```

the extents mirl's data layout of `row` gives every primitive. a pointer mirl
holds logically, with no representation in memory, has none

