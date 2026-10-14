# Contributing to Mach

Thank you for your interest in contributing to Mach. Be respectful,
constructive, and professional: treat Mach like a passion project and its
community like family.


## Building

Mach is self-hosting, so building it needs an existing Mach compiler. Install
the latest [release](https://github.com/briar-systems/mach/releases) for your
host and put `mach` on `PATH`; CI seeds from the same archive
(`.github/scripts/bootstrap.sh`, see [Fixpoint](#fixpoint)).

```bash
git clone https://github.com/briar-systems/mach.git
cd mach
bash .github/scripts/seed-build.sh mach a
./a dep pull .
./a build .
```

A 6.x release reads only the previous manifest keys, so it builds `a` through
`seed-build.sh` (see [Fixpoint](#fixpoint)), and `a` builds the compiler. It is
written to `out/<target>/<profile>/bin/mach`, or `bin/mach.exe` on Windows. A
default Linux x86_64 build writes `out/linux-x86_64/debug/bin/mach`.


## Fixpoint

A change to the compiler has to reach the self-host fixpoint: the compiler it
builds must build itself byte for byte. CI does not check it, so run it
locally. `.github/scripts/bootstrap.sh` is the one way: it reads `RUNNER_OS`
and `RUNNER_TEMP` and writes `MACH` to the file `GITHUB_ENV` names, so a local
run names them. From the repository root:

```bash
mkdir -p out/boot
RUNNER_OS=Linux RUNNER_TEMP=$PWD/out/boot GITHUB_ENV=$PWD/out/boot/env \
    bash .github/scripts/bootstrap.sh --fixpoint
```

The script installs the seed release named on the `seed` line of
`.github/bootstrap-chain`, never whichever `mach` is on `PATH`, and walks the
chain through each pinned mach commit. The last step builds `a` from your tree,
`a` builds `b`, and `b` builds `c`. It fails when `cmp b c` differs and
otherwise leaves `c` as the compiler, with its path in `out/boot/env`. Use `RUNNER_OS=macOS` or
`Windows` on those hosts (the outputs are then `a.exe`, `b.exe` and `c.exe` on
Windows), and `--profile release` for the release fixpoint. Run the unit
suites with `c`, as in [Testing and formatting](#testing-and-formatting),
because the test code may use language the seed release predates. Without
`--fixpoint` it stops at `b`, which is what CI builds before `mach test`.

`-o` names a canonical path inside the project root: relative, `/`-separated,
with no `.` or `..` component. `-o ../a`, `-o ./a` and an absolute path are
refused with `-o must name a canonical path inside the project root`, so keep
the stage outputs in the checkout. `.gitignore` covers `a`, `b` and `c`.


## Testing and formatting

Run the tests and the formatter through the compiler you just built, never
the seed on `PATH`:

```bash
out/linux-x86_64/debug/bin/mach test .
out/linux-x86_64/debug/bin/mach test . -p release
out/linux-x86_64/debug/bin/mach test . -a tests
out/linux-x86_64/debug/bin/mach test . -a tests -p release
out/linux-x86_64/debug/bin/mach fmt .
```

`mach test .` runs the tests in the compiler's own closure. The suites that
live in modules the compiler never reaches (`src/lang/driver/tests/` and the
rest) are reached by the `tests` library artifact, whose entry `src/lib/tests.mach` `use`s each of them, so a new
test-only module is added there.

The tree is canonical: `mach fmt .` must leave it unchanged before a pull
request is opened (`mach fmt --check .` reports what differs). `.` reaches only
the compiler's own source, so the test sources are held to it one operand at a
time: each file under `test/cases` and `test/lib`, and each project under
`test/link/cases` (its `mach.toml`) and `test/f16proof`; the command is under
[Checks CI does not run](#checks-ci-does-not-run). The fuzz corpus is retained input and is left as it is.

Two tests in `src/lang/driver/tests/hygiene.mach` hold the source to its import
and surface rules. A module is imported under the same name in every file, and
a name imports one module in every file, but for the arms of a
declaration-level `$if`. A `pub` declaration is named in some other file, or
its doc comment says what reaches it, as in `reached only by mach-lsp`. The
code that leaves for mirl, masc and mink is exempt.

The API reference is held the same way: `doc/mach` and `doc/README.md` are what `mach doc .`
writes, so a change to a doc-comment or to the module tree regenerates them in
the same pull request, and the docs check below fails when the committed pages
differ from a fresh generation.

### Checks CI does not run

CI is the build and the unit suites (`mach test .` and `mach test . -a tests`)
and nothing else. Every other check is a local command, run from the repository
root with the compiler you built (`$MACH`, the path in `out/boot/env`, or
`out/<host>/debug/bin/mach`). A pull request that touches what a check covers
runs that check before it is marked ready. Each command exits non-zero on
failure.

| check | command |
|---|---|
| formatter | the block below |
| committed reference | `"$MACH" doc . -o out/docgen -q && diff -r out/docgen/mach doc/mach && diff out/docgen/README.md doc/README.md` |
| documented flags are accepted | `python3 .github/scripts/doc-flags.py "$MACH" doc` |
| doc and source markers cite open issues | `GH_TOKEN=$(gh auth token) GH_REPO=briar-systems/mach python3 .github/scripts/doc-markers.py doc/language src` |
| release notes selection | `bash .github/scripts/release-notes-test.sh` |
| `doc/language` code blocks | `MACH="$MACH" bash test/run.sh --docs` |
| warm rebuilds match clean | `MACH="$MACH" bash test/run.sh --incremental` |
| codegen corpus, one target | `MACH="$MACH" bash test/run.sh --target x86_64-linux` |
| codegen corpus, every target | `MACH="$MACH" bash test/run.sh --qemu` |
| listings reassemble | `MACH="$MACH" bash test/run.sh --target x86_64-linux --asm` |
| debug info verifies | `MACH="$MACH" bash test/run.sh --dwarf --target x86_64-linux` |
| link cases (leaving with the linker) | `MACH="$MACH" bash test/run.sh --link` |
| f16 proof | `MACH="$MACH" bash test/run.sh --f16proof --target x86_64-linux --shard 0/64` |
| cross-build the compiler | `"$MACH" build . --target darwin-x86_64 --profile release -o out/cross-darwin-x86_64` (also `darwin-aarch64`, `linux-riscv64`) |
| fixpoint | see [Fixpoint](#fixpoint) |

The formatter holds `.`, each corpus case and library file, and each project
under `test/link/cases` and `test/f16proof`:

```bash
rc=0
"$MACH" fmt --check . || rc=1
for f in $(find test/cases test/lib -name '*.mach' | sort); do "$MACH" fmt --check "$f" || rc=1; done
for p in $(find test/link/cases -name mach.toml | sort) test/f16proof; do "$MACH" fmt --check "$p" || rc=1; done
exit "$rc"
```

`test/run.sh` needs `cc`, and the corpus columns, `--dwarf`, `--asm` and
`--link` need the readers they cite (`spirv-val`, `llvm-dwarfdump`, GNU as,
qemu); it names a missing one and exits 2. See [test/README.md](test/README.md)
for the runner, its per-case timeout and the exit contract.

### Test policy

What earns a test, and where it sits, is set by the
[test policy](doc/language/test.md#test-policy).


## Branching

- `main` holds tagged releases only. It takes integration merges from `dev`.
- `dev` is the integration branch and the target of every pull request.
- `feat/<issue>` and `fix/<issue>` branch off `dev` and return to it.
- `hotfix/<issue>` branches off `main` and merges into both `main` and `dev`.


## Commits

Commits are small, self-contained, and conventional. The issue number is the
scope:

```
fix(#1234): brief description

Longer explanation if needed.
```

The types are `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `style`,
`ci`, and `perf`. A breaking change marks the type with `!` before the colon:
`feat(#139)!: brief description`. A change with no issue uses `chore: ...` with
no scope, and a release commit is `chore(release): <version>`.

For example, `chore: update dependencies`.


## Pull requests

- Open as a draft targeting `dev`; mark it ready when it is done.
- Link the issue with `Closes #N`.
- Leave `CHANGELOG.md` alone. The changelog is written from the merged
  commits when `dev` is released to `main`.
- Say briefly what changed and which tests you ran. Every pull request runs
  the build and the unit suites on the linux runners. The windows, darwin and
  release lanes run when the CI workflow is dispatched with a `lane`, which is
  worth doing before a windows or darwin change reaches `dev`, and `lane=all`
  runs green on `dev` before every release. Every other check is local, see
  [Checks CI does not run](#checks-ci-does-not-run).
- Merge with a merge commit. Never rebase or fast-forward.
- When the target is not the default branch, close the linked issue by hand
  after the merge.


## Issues

File issues through the templates. Issues use an orthogonal, faceted tagging system across six sets:

- SemVer magnitude: `patch`, `minor`, `major`
- Kind of work: `feature`, `fix`, `removal`, `chore`, `performance`
- Where (domain or location): `testing`, `tooling`, `doc` (omitted when touching core compiler code)
- Severity and state: `critical`, `blocked`, `parked`, `security`
- Discussion: `discussion` (design proposals, RFCs, and open debates)

Tags mix and match across sets (for example, `patch`, `fix`, `tooling`). When opening an issue, select the applicable tags in the sidebar. There are no milestones. Work in flight is an open draft pull request, `parked` marks an issue deliberately set aside until something changes, and every other open issue is backlog. `blocked` is a tag that records state. Themes are epic issues with native sub-issues. SemVer impact comes from the `major`, `minor` and `patch` tags, never from a milestone.


## Versioning

Mach adheres strictly to [semantic versioning](https://semver.org/) (`vMAJOR.MINOR.PATCH`):
- `MAJOR`: breaking changes to the language grammar, compiler interface, or supported runtime contracts.
- `MINOR`: backward-compatible new language features, compiler flags, target additions, and optimizations.
- `PATCH`: backward-compatible bug fixes, documentation, and internal refactors.

The standard library is versioned separately in its own repository. SemVer impact is tracked directly on issues and pull requests via conventional commit types and issue tags. A release bump updates both `[project].version` in `mach.toml` and `MACH_VERSION` in `src/lang/version.mach`. Tags are created on `main` after the integration merge from `dev`.


## License

By contributing, you agree that your contributions will be licensed under the
[MIT License](LICENSE).
