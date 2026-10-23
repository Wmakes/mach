# mach.lang.target.owner

the owners' target each mach target cell means

a mach target cell, an operating system, an instruction set and a calling
convention, names one target of mirl's, assembled from the five parts of its
tuple the lower layers own: mink's architecture, system and environment, the
architecture's convention and masc's baseline. the cell states every part as
its owners spell it and mirl assembles it, so mach states which target it
means and none of its facts. a cell no owner assembles yet keeps mach's own
facts until one does (#4779)

## rec Owned

```mach
pub rec Owned;
```

what the owners state of one cell for mach's front end, read once when the
target resolves

scalars: the extents of every primitive and the vector registers
nans: the nan each float format's operations return

## fun owned_for

```mach
pub fun owned_for(a: *A.Allocator, os_id: u32, arch_id: u32, abi_id: u32, spelled: opt[str], t: *extension.Table, bits: u64) res[opt[Owned], fail.Fail];
```

what the owners state of the cell of `os_id`, `arch_id` and `abi_id` standing on
the selection the target spells, `spelled`, else the cell's own, under the
selection `bits` of mach's extension table `t`, none for a cell no owner
assembles yet

