# mach.lang.target.catalog.arch

the instruction set catalog: the architectures of mink's catalog mach has a
target for, each with its id and its fingerprint tag. it is an explicit projection: a row names mink's architecture
row and is spelled by it, and an architecture of mink's no row names (x86,
arm64e, wasm32, ...) is no instruction set here and no `$mach.arch` tag

## val UNKNOWN

```mach
pub val UNKNOWN: u32 = 0
```

## val X86_64

```mach
pub val X86_64:  u32 = 1
```

## val AARCH64

```mach
pub val AARCH64: u32 = 2
```

## val RISCV64

```mach
pub val RISCV64: u32 = 3
```

## val SPIRV

```mach
pub val SPIRV:   u32 = 5
```

4 was the withdrawn MOS 6502 target; no catalog row carries it

## val RISCV32

```mach
pub val RISCV32: u32 = 6
```

## def DwarfRegFn

```mach
pub def DwarfRegFn: fun(i32) i32
```

## def CvRegFn

```mach
pub def CvRegFn: fun(i32) i32
```

## val VERSION

```mach
pub val VERSION: u8 = 2
```

version 2: catalog id 4 is reserved with no row and the tags after it moved up

## fun id_for

```mach
pub fun id_for(name: str) u32;
```

## fun host_id

```mach
pub fun host_id() u32;
```

the instruction set the compiler itself was built for, UNKNOWN when the
catalog has no row for it

## fun owner_of

```mach
pub fun owner_of(id: u32) *mink_arch.Arch;
```

mink's architecture row the instruction set `id` means, nil for an unknown id

## fun name_for

```mach
pub fun name_for(id: u32) str;
```

## fun count

```mach
pub fun count() usize;
```

## fun name_at

```mach
pub fun name_at(index: usize) str;
```

## fun fingerprint_tag

```mach
pub fun fingerprint_tag(id: u32) u8;
```

