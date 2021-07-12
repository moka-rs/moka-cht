# moka-cht

[![GitHub Actions][gh-actions-badge]][gh-actions]
[![crates.io release][release-badge]][crate]
[![docs][docs-badge]][docs]
[![dependency status][deps-rs-badge]][deps-rs]
[![license][license-badge]](#license)

moka-cht provides a lock-free hash table that supports fully concurrent lookups,
insertions, modifications, and deletions. The table may also be concurrently resized
to allow more elements to be inserted. moka-cht also provides a segmented hash table
using the same lock-free algorithm for increased concurrent write performance.

[gh-actions-badge]: https://github.com/moka-rs/moka-cht/workflows/CI/badge.svg
[release-badge]: https://img.shields.io/crates/v/moka-cht.svg
[docs-badge]: https://docs.rs/moka-cht/badge.svg
[deps-rs-badge]: https://deps.rs/repo/github/moka-rs/moka-cht/status.svg
[license-badge]: https://img.shields.io/crates/l/moka-cht.svg

[gh-actions]: https://github.com/moka-rs/moka-cht/actions?query=workflow%3ACI
[crate]: https://crates.io/crates/moka-cht
[docs]: https://docs.rs/moka-cht
[deps-rs]: https://deps.rs/repo/github/moka-rs/moka-cht

## Usage

Add this to your `Cargo.toml`:

```toml
moka-cht = "0.5"
```

Then in your code:

```rust
use moka_cht::HashMap;
use std::{sync::Arc, thread};

let map = Arc::new(HashMap::new());

let threads: Vec<_> = (0..16)
    .map(|i| {
        let map = map.clone();

        thread::spawn(move || {
            const NUM_INSERTIONS: usize = 64;

            for j in (i * NUM_INSERTIONS)..((i + 1) * NUM_INSERTIONS) {
                map.insert_and(j, j, |_prev| unreachable!());
            }
        })
    })
    .collect();

let _: Vec<_> = threads.into_iter().map(|t| t.join()).collect();
```

## License

moka-cht is distributed under either of

- The MIT license
- The Apache License (Version 2.0)

at your option.

See [LICENSE-MIT](LICENSE-MIT) and [LICENSE-APACHE](LICENSE-APACHE) for details.

## Credits

moka-cht is a fork for [cht v0.4.1][cht-v041]. We have created this fork to provide
better integration with [Moka cache][moka-cache] via a non default Cargo feature.

cht is authored by Gregory Meyer and its v0.4.1 and earlier versions are licensed
under the MIT license.

[cht-v041]: https://github.com/Gregory-Meyer/cht/tree/v0.4.1
[moka-cache]: https://github.com/moka-rs/moka
