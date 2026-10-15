# mach.lang.session

## fwd module.ModuleId

```mach
fwd module.ModuleId
```

forwards [`mach.lang.module.ModuleId`](module.md#rec-moduleid)

## fwd module.MODULE_NIL

```mach
fwd module.MODULE_NIL
```

forwards [`mach.lang.module.MODULE_NIL`](module.md#val-module_nil)

## fwd module.StableModuleId

```mach
fwd module.StableModuleId
```

forwards [`mach.lang.module.StableModuleId`](module.md#rec-stablemoduleid)

## fwd module.STABLE_MODULE_NIL

```mach
fwd module.STABLE_MODULE_NIL
```

forwards [`mach.lang.module.STABLE_MODULE_NIL`](module.md#val-stable_module_nil)

## val FILE_TEXT

```mach
pub val FILE_TEXT: query.Kind = query.Kind;
```

a file's text as the session loaded it, keyed by the file's index. the source map and the
editor hold it across builds, so no released module takes it

## rec Session

```mach
pub rec Session;
```

## fun init

```mach
pub fun init(alloc: *A.Allocator) res[Session, fail.Fail];
```

## fun init_over

```mach
pub fun init_over(alloc: *A.Allocator, base: *intern.Interner) res[Session, fail.Fail];
```

a session whose names are layered over `base`: every id `base` has issued
names the same string here, so what was planned in another session, its
manifests and its cells, is read here as it was planned. `base` outlives the
session and takes no new names while it lives

## fun drop_query_failure

```mach
pub fun drop_query_failure(s: *Session);
```

release the text and the places of the last query failure the session holds

## fun adopt_failure_text

```mach
pub fun adopt_failure_text(s: *Session, text: str);
```

hold the text an operation handed back, allocated from the query database, as the held
failure's text; the session owns it from here

## fun hold_failure

```mach
pub fun hold_failure(s: *Session, failure: fail.Fail, lost: *bool) fail.Fail;
```

hold a failure where the session presents it: its text and the places it points at are
copied into the session's slot, replacing what the slot held, and the failure returned reads
the held copies. text the presentation already owns stays there. a copy the allocator
refused leaves the failure without that part and sets `lost`

s: the session
failure: the failure to hold; one with no text or places empties the slot
lost: set when part of the failure could not be held
ret: the failure as the session holds it

## fun dnit

```mach
pub fun dnit(s: *Session);
```

## fun reset_module_registry

```mach
pub fun reset_module_registry(s: *Session);
```

## fun module_count

```mach
pub fun module_count(s: *Session) u32;
```

one past the highest project module id the session has an identity for

## fun register_module_root_project

```mach
pub fun register_module_root_project(s: *Session, mid: module.ModuleId, root: bool) err[fail.Fail];
```

## fun module_is_root_project

```mach
pub fun module_is_root_project(s: *Session, mid: module.ModuleId) bool;
```

a module the loader never declared is not the root project's: a dependency's
module, or one loaded outside a project, exports nothing of its own

## fun module_fqn

```mach
pub fun module_fqn(s: *Session, mid: module.ModuleId) intern.StrId;
```

## fun stable_module_id

```mach
pub fun stable_module_id(s: *Session, fqn: intern.StrId) res[module.StableModuleId, fail.Fail];
```

## fun stable_module_id_lookup

```mach
pub fun stable_module_id_lookup(s: *Session, fqn: intern.StrId) module.StableModuleId;
```

## fun register_stable_mid

```mach
pub fun register_stable_mid(s: *Session, stable: module.StableModuleId, mid: module.ModuleId) err[fail.Fail];
```

## fun module_id_for_stable

```mach
pub fun module_id_for_stable(s: *Session, stable: module.StableModuleId) module.ModuleId;
```

## fun reset_type_projection

```mach
pub fun reset_type_projection(s: *Session) err[fail.Fail];
```

## fun register_module_identity

```mach
pub fun register_module_identity(s: *Session, mid: module.ModuleId, fqn: intern.StrId, stable: module.StableModuleId) err[fail.Fail];
```

give the module `mid` its name and stable identity; a project's module also
counts toward module_count, a standalone buffer's does not

## fun unregister_module_identity

```mach
pub fun unregister_module_identity(s: *Session, mid: module.ModuleId, stable: module.StableModuleId);
```

## fun next_parse_incarnation

```mach
pub fun next_parse_incarnation(s: *Session) res[u64, fail.Fail];
```

## fun load_source

```mach
pub fun load_source(s: *Session, path: str, text: str) res[lang_source.FileId, fail.Fail];
```

## rec PreparedOverlay

```mach
pub rec PreparedOverlay;
```

## fun prepare_overlay

```mach
pub fun prepare_overlay(s: *Session, path: str, text: str, remove: bool) res[PreparedOverlay, fail.Fail];
```

the caller exclusively borrows overlay state through commit or discard

## fun discard_overlay

```mach
pub fun discard_overlay(prepared: *PreparedOverlay);
```

## fun commit_overlay

```mach
pub fun commit_overlay(prepared: *PreparedOverlay) bool;
```

## fun set_overlay

```mach
pub fun set_overlay(s: *Session, path: str, text: str) err[fail.Fail];
```

## fun clear_overlay

```mach
pub fun clear_overlay(s: *Session, path: str) res[bool, fail.Fail];
```

## fun overlay_get

```mach
pub fun overlay_get(s: *Session, path: str) opt[str];
```

