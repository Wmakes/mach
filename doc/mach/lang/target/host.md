# mach.lang.target.host

the host a mach target cell runs on

a cell's host is mink's row for its system, under the environment the os runs
under, for its architecture, held under the traits its calling convention
states, as riscv's float abi picks one of linux's riscv64 rows. what a host
says of the cell, the capabilities it offers and the instruction set floor
every machine it runs has, is read from that row and stated nowhere in mach

## fun of

```mach
pub fun of(a: *A.Allocator, os_id: u32, arch_id: u32, abi_id: u32) res[*mink_os.Host, fail.Fail];
```

mink's host of the cell of `os_id`, `arch_id` and `abi_id`, refused where mink
names no system or architecture for it or no host of the system holds

## fun caps

```mach
pub fun caps(a: *A.Allocator, os_id: u32, arch_id: u32, abi_id: u32) res[mink_os.Caps, fail.Fail];
```

the capabilities the host of the cell offers an image that states no oldest
system version, so it runs on the oldest the system states

## fun floor

```mach
pub fun floor(a: *A.Allocator, os_id: u32, arch_id: u32, abi_id: u32) res[opt[str], fail.Fail];
```

the instruction set floor of the cell's host, in masc's selection spelling,
none where the system bounds the machine not at all, as a freestanding one

