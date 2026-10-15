# mach.lang.module

## rec ModuleId

```mach
pub rec ModuleId;
```

a module's index in the session's module table

## val MODULE_NIL

```mach
pub val MODULE_NIL: ModuleId = ModuleId;
```

## rec StableModuleId

```mach
pub rec StableModuleId;
```

a module's identity across sessions, keyed by its fully qualified name

## val STABLE_MODULE_NIL

```mach
pub val STABLE_MODULE_NIL: StableModuleId = StableModuleId;
```

## val BUFFER_BASE

```mach
pub val BUFFER_BASE: u32 = 0x80000000
```

a project numbers its modules from 0 below BUFFER_BASE, and a standalone
editor buffer's module is BUFFER_BASE past its file's index, so no project
module takes a buffer's id and a table keyed by module serves both

## fun buffer_id

```mach
pub fun buffer_id(file: u32) opt[ModuleId];
```

the module of the standalone buffer over the file with index `file`, none
past the last id the buffer range holds

## fun buffer_file

```mach
pub fun buffer_file(m: ModuleId) opt[u32];
```

the index of the file whose standalone buffer is the module `m`, none when
`m` is a project's module or nil

## fun id

```mach
pub fun id(index: u32) ModuleId;
```

## fun index

```mach
pub fun index(m: ModuleId) u32;
```

## fun same

```mach
pub fun same(left: ModuleId, right: ModuleId) bool;
```

## fun is_nil

```mach
pub fun is_nil(m: ModuleId) bool;
```

## fun stable_id

```mach
pub fun stable_id(index: u32) StableModuleId;
```

## fun stable_index

```mach
pub fun stable_index(m: StableModuleId) u32;
```

## fun stable_same

```mach
pub fun stable_same(left: StableModuleId, right: StableModuleId) bool;
```

## fun stable_is_nil

```mach
pub fun stable_is_nil(m: StableModuleId) bool;
```

## fun compose_fqn

```mach
pub fun compose_fqn(alloc: *A.Allocator, itn: *intern.Interner, id_text: str, rel_text: str) res[intern.StrId, fail.Fail];
```

the module name of a source path under a package: the id, then the path without its suffix, its separators dotted

