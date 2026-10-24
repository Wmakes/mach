# mach.lang.driver.passes

## val LOWERED_SURFACE

```mach
pub val LOWERED_SURFACE: query.Kind = query.Kind;
```

a module's source with what lowering omits cut away, keyed by its stable id

## val LOWER

```mach
pub val LOWER: query.Kind = query.Kind;
```

a module's lowered unit, keyed by its back-half key; the codegen product spends it

## val CODEGEN

```mach
pub val CODEGEN: query.Kind = query.Kind;
```

a module's object, mink's, keyed by its back-half key

## val LINK

```mach
pub val LINK: query.Kind = query.Kind;
```

the link's inputs as revisions, so a link reruns only when one moved

## val LINK_CONFIG

```mach
pub val LINK_CONFIG: query.Kind = query.Kind;
```

what the link is configured with beyond the build context, set by the build

## fun run_resolve_pass

```mach
pub fun run_resolve_pass(p: *project.Project) err[fail.Fail];
```

a rejected module is one module's answer, so the pass folds it and carries
on over the rest; an internal failure ends the pass where it met it

## fun run_sema_pass

```mach
pub fun run_sema_pass(p: *project.Project) err[fail.Fail];
```

a rejected module is one module's answer, so the pass folds it and carries
on over the rest; an internal failure ends the pass where it met it

## fun retain_modules

```mach
pub fun retain_modules(p: *project.Project, rejected: bool) err[fail.Fail];
```

every module the load reached, gated out or not, is held under the session's active retainer: the
project borrows each one's parse. a load closes its retainer's round unless a caller keeps the round
open across builds, and then whatever no retainer holds is retired

## fun back_half_key

```mach
pub fun back_half_key(stable: session.StableModuleId, test_object: bool) u64;
```

a module's lowering and codegen products are keyed by its stable id, and by the
test bit for its test object

## fun back_half_key_is_test

```mach
pub fun back_half_key_is_test(key: u64) bool;
```

## fun back_half_key_for

```mach
pub fun back_half_key_for(p: *project.Project, m: *project.ModuleEntry) u64;
```

the normal object's key, the same in every build that compiles the module

## fun test_key_for

```mach
pub fun test_key_for(m: *project.ModuleEntry) u64;
```

## fun has_test_object

```mach
pub fun has_test_object(p: *project.Project, m: *project.ModuleEntry) bool;
```

the module gets a test object in this build: a test build, and a module whose
source declares a test or a `#[testing]` declaration

## fun codegen_reusable

```mach
pub fun codegen_reusable(p: *project.Project, mid: session.ModuleId) res[bool, fail.Fail];
```

## fun test_build

```mach
pub fun test_build(p: *project.Project) bool;
```

## fun run_lower_pass

```mach
pub fun run_lower_pass(p: *project.Project) err[fail.Fail];
```

## fun prepare_lower_pass

```mach
pub fun prepare_lower_pass(p: *project.Project) err[fail.Fail];
```

## fun acquire_codegen_inputs

```mach
pub fun acquire_codegen_inputs(p: *project.Project) err[fail.Fail];
```

the modules that still lower here are those without a product restored under this
operation's snapshot

## fun acquire_test_inputs

```mach
pub fun acquire_test_inputs(p: *project.Project) err[fail.Fail];
```

the test operation's inputs: the typed definitions and each test object's lowered unit,
unless the load restored its object from `obj/`

## fun run_test_object_pass

```mach
pub fun run_test_object_pass(p: *project.Project) err[fail.Fail];
```

## fun run_codegen_pass

```mach
pub fun run_codegen_pass(p: *project.Project) err[fail.Fail];
```

## fun test_codegen_reusable

```mach
pub fun test_codegen_reusable(p: *project.Project, mid: session.ModuleId) res[bool, fail.Fail];
```

the test object's codegen product is current or cached, so no worker generates it

## fun compile_object

```mach
pub fun compile_object(p: *project.Project, u: *me_lower.Unit) res[Object, fail.Fail];
```

the unit `u` compiled into an object, which owns its storage in the session's backing
allocator. compiling spends the unit's module, so a unit compiles once

## fun object_handle_decode

```mach
pub fun object_handle_decode(e: query.QueryView) *Object;
```

## fun run_link_pass

```mach
pub fun run_link_pass(p: *project.Project) res[bool, fail.Fail];
```

## fun prepare_object_cache

```mach
pub fun prepare_object_cache(p: *project.Project) err[fail.Fail];
```

key the loaded modules and read `obj/` under their keys (driver/cache),
reported as the cache phase carved out of resolve, which it precedes

## fun module_in_current_project

```mach
pub fun module_in_current_project(p: *project.Project, fqn: intern.StrId) bool;
```

