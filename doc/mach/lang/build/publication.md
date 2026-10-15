# mach.lang.build.publication

what the links a warm host ran wrote: for each artifact path, the revision
of the link query that wrote it and the digest of the bytes written, so a
later build at the same revision reuses an artifact nobody touched. the host
that keeps the session between builds keeps this beside it

## rec Publications

```mach
pub rec Publications;
```

keyed by the artifact path, interned in the session the links ran in

## fun init

```mach
pub fun init(alloc: *A.Allocator) Publications;
```

## fun dnit

```mach
pub fun dnit(p: *Publications);
```

## fun record

```mach
pub fun record(p: *Publications, itn: *intern.Interner, path: str, revision: query.Revision, digest: *[32]u8) err[fail.Fail];
```

the link at `revision` wrote `digest` to `path`

## fun at

```mach
pub fun at(p: *Publications, itn: *intern.Interner, path: str, revision: query.Revision) res[opt[[32]u8], fail.Fail];
```

the digest the link at `revision` wrote to `path`, none when it wrote none there

