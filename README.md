# Grimoire Core Tome

`core` is the bootstrap tome for Grimoire source builds. It provides the first managed userland
tools that runes should depend on instead of reaching for host `/bin` tools.

## Current Stage-0 Boundary

The current bootstrap boundary is deliberately small:

- Grimoire supplies managed build tools such as `bash`, `make`, `coreutils`, `sed`, `grep`,
  `gawk`, and `diffutils`.
- The host still supplies the C compiler, linker, platform SDK, and related compiler tools.
- Runes use `ctx.prefix`/`ctx.store_path` as the final package prefix and install into
  `ctx.package_dir` with `DESTDIR`.

## Published Packages

The initial publishable set is:

- `bash`
- `make`
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

Build every rune:

```sh
grm tome build --all --path .
```

The generated `dist/` directory is intentionally ignored by git. Publish its contents to the
package host that `tome.rn` advertises.
