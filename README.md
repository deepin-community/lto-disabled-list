# lto-disabled-list

A list of source packages not to build with link time optimization (LTO).

The dpkg vendor build feature `optimize/lto` is enabled by default on
amd64, arm64, sw64 and loong64. `Dpkg::Vendor::Debian` consults
`/usr/share/lto-disabled-list/lto-disabled-list` (shipped by this
package) and turns the feature off for listed source packages, using
the same model as Ubuntu.

## List format

    <source-package> any | <arch> [<arch> ...]

- `any` excludes the source package on every architecture; otherwise
  give a space-separated list of architecture tags, e.g. `amd64 arm64`.
- Lines starting with `#` are comments.

## Opting out per build

Set `DEB_BUILD_MAINT_OPTIONS=optimize=-lto` to disable LTO for a
single package build, regardless of the list.

## Current entries

The 116 source packages that ended the UOS archive LTO rebuild on
x86_64 (Shuttle batch task, September 2026) in the APPLY_FAILED or
UPLOAD_GIVEUP state. They failed on amd64 and are tagged `any`
(excluded on every architecture), so that the arm64 / sw64 /
loong64 rebuilds do not trip over them; narrow an entry back to
specific architectures once the package is proven to build with
LTO there.

20 of them are also on Ubuntu's list; the rest are UOS-specific
findings.

## Reference baseline

Ubuntu lto-disabled-list v81 is the reference baseline for future
entries. Do not blindly import entries from it; in particular do
not re-add the qt6-* entries, because deepin builds the Qt6 stack
with LTO by default (explicit `optimize=+lto` in the
deepin-community qt6-* packaging).
