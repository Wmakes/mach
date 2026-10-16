# mach.lang.target.extension.riscv

## val I

```mach
pub val I:        u64 = 1
```

## val M

```mach
pub val M:        u64 = 2
```

## val A

```mach
pub val A:        u64 = 4
```

## val F

```mach
pub val F:        u64 = 8
```

## val D

```mach
pub val D:        u64 = 16
```

## val C

```mach
pub val C:        u64 = 32
```

## val ZICSR

```mach
pub val ZICSR:    u64 = 64
```

## val ZIFENCEI

```mach
pub val ZIFENCEI: u64 = 128
```

## val ZKT

```mach
pub val ZKT: u64 = 256
```

data-independent execution latency: the listed M, Zb and base operations run in
time independent of their operand values (riscv-crypto scalar spec, Zkt)

## val ZICOND

```mach
pub val ZICOND: u64 = 512
```

the integer conditional operations czero.eqz and czero.nez, the branch-free
select

## val ZFHMIN

```mach
pub val ZFHMIN: u64 = 1024
```

the half-precision float extensions: Zfhmin moves, loads, stores and
converts binary16 in the f registers, and Zfh, which brings it, adds the
arithmetic, comparison and integer conversions. both need F

## val ZFH

```mach
pub val ZFH:    u64 = 2048
```

## val ALL

```mach
pub val ALL:    u64 = I | M | A | F | D | C | ZICSR | ZIFENCEI | ZKT | ZICOND | ZFHMIN | ZFH
```

## val TABLE

```mach
pub val TABLE: extension.Table = extension.Table;
```

## fun widen

```mach
pub fun widen(bits: u64, added: u64) u64;
```

a selection widened by names from a manifest list and closed over what they
imply: d brings f, f brings zicsr

## fun float_requirement

```mach
pub fun float_requirement(bits: u32) u64;
```

the bits a floating-point width needs passed in float registers: zfhmin for 16-bit
values, f for 32-bit ones, f and d for 64

## fun has

```mach
pub fun has(bits: u64, required: u64) bool;
```

