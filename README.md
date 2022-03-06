# cargo-public-items

List public items (the public API) of a Rust library crate by analyzing the rustdoc JSON of the crate. Also supports diffing the public API between releases and commits.

Automatically builds the rustdoc JSON for you, which requires a nightly Rust toolchain to be installed (see [here](https://github.com/Enselic/public_items#compatibility-matrix)).


# Installation

```
cargo install cargo-public-items
```

# Usage

## List public items

This example lists the public items of the library that this cargo subcommand is implemented with. The example assumes that the git repository for the library is checked out to `~/src/public_items`. To print all items that make up the public API of your Rust library crate, simply do this:

```
% cd ~/src/public_items
% cargo public-items
```

and the public API will be printed with one line per item:

```
pub enum public_items::Error
pub enum variant public_items::Error::SerdeJsonError(serde_json::Error)
pub fn public_items::Error::fmt(&self, __formatter: &mut std::fmt::Formatter<'_>) -> std::fmt::Result
pub fn public_items::Error::fmt(&self, f: &mut $crate::fmt::Formatter<'_>) -> $crate::fmt::Result
pub fn public_items::Error::from(source: serde_json::Error) -> Self
pub fn public_items::Error::source(&self) -> std::option::Option<&std::error::Error + 'static>
pub fn public_items::PublicItem::cmp(&self, other: &PublicItem) -> $crate::cmp::Ordering
pub fn public_items::PublicItem::eq(&self, other: &PublicItem) -> bool
pub fn public_items::PublicItem::fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result
pub fn public_items::PublicItem::ne(&self, other: &PublicItem) -> bool
pub fn public_items::PublicItem::partial_cmp(&self, other: &PublicItem) -> $crate::option::Option<$crate::cmp::Ordering>
pub fn public_items::sorted_public_items_from_rustdoc_json_str(rustdoc_json_str: &str) -> Result<Vec<PublicItem>>
pub mod public_items
pub struct public_items::PublicItem
pub type public_items::Result<T> = std::result::Result<T, Error>
```

## Diff public items between commits

To diff two different versions of your API, use `--dif-git-checkouts`. This example prints the public API diff between v0.2.0 and v0.4.0:

```
% cargo public-items --diff-git-checkouts v0.2.0 v0.4.0
Removed from the public API:
============================
(nothing)

Changes to the public API:
==========================
-pub fn public_items::sorted_public_items_from_rustdoc_json_str(rustdoc_json_str: &str) -> Result<Vec<PublicItem>>
+pub fn public_items::sorted_public_items_from_rustdoc_json_str(rustdoc_json_str: &str, options: Options) -> Result<Vec<PublicItem>>

Added to the public API:
========================
+pub fn public_items::Options::clone(&self) -> Options
+pub fn public_items::Options::default() -> Self
+pub fn public_items::Options::fmt(&self, f: &mut $crate::fmt::Formatter<'_>) -> $crate::fmt::Result
+pub struct public_items::Options
+pub struct field public_items::Options::with_blanket_implementations: bool
```

You can also manually do a diff by writing the full list of items to a file for two different versions of your library and then do a regular `diff` between the files.

# Target audience

Maintainers of Rust libraries that want to keep track of changes to their public API.

# Implementation details

This utility is a convenient `cargo` wrapper around the [public_items](https://crates.io/crates/public_items) crate (https://github.com/Enselic/public_items).
