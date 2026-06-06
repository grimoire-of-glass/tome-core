# Grimoire Core Tome

`core` is the bootstrap tome for Grimoire source builds. It provides the first managed userland
that runes can depend on instead of reaching for host `/bin` tools.

## Current Stage-0 Boundary

The bootstrap boundary is deliberately small — we lean on the host POSIX userland:

- **Host supplies** the POSIX shell (`sh`), POSIX `make`, and the POSIX userland (`sed`, `grep`,
  `awk`, `find`, `cp`, `chmod`, `expr`, `test`, `tr`, `mkdir`, etc.). These are always available
  in managed builds via the POSIX ambient PATH.
- **Host supplies** the C compiler, linker, platform SDK, and related compiler tools.
- **Grimoire supplies** non-POSIX tools that builds explicitly invoke: `bash`, `m4`, `perl`, and
  the GNU toolchain (`autoconf`, `automake`, `libtool`).
- Runes use `ctx.prefix`/`ctx.store_path` as the final package prefix and install into
  `ctx.package_dir` with `DESTDIR`.

## Published Packages

The initial publishable set is:

- `make`
- `bash`
- `m4`
- `perl`
- `coreutils`
- `sed`
- `grep`
- `gawk`
- `diffutils`

`findutils` and `hello` are kept as useful follow-on/package-authoring runes, but they are not
part of the minimal managed build-readiness set yet.

## Building

Build and register one package in `dist/index.nuon`:

```sh
grm tome build bash --path .
```

Build every rune (self-bootstrapping: earlier packages are installed store-only so later runes
can use them as build dependencies):

```sh
grm tome build --all --path .
```

The generated `dist/` directory is intentionally ignored by git. Publish its contents to the
package host that `tome.rn` advertises.
