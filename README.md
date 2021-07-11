# moka-cht

[![crates.io](https://img.shields.io/crates/v/moka-cht.svg)](https://crates.io/crates/moka-cht)
[![docs.rs](https://docs.rs/moka-cht/badge.svg)](https://docs.rs/moka-cht)

moka-cht provides a lock-free hash table that supports fully concurrent lookups,
insertions, modifications, and deletions. The table may also be concurrently
resized to allow more elements to be inserted. moka-cht also provides a segmented
hash table using the same lock-free algorithm for increased concurrent write
performance.

## Usage

In your `Cargo.toml`:

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
                map.insert_and(j, j, |prev| assert_eq!(prev, None));
            }
        })
    })
    .collect();

let _: Vec<_> = threads.into_iter().map(|t| t.join()).collect();
```

## License

moka-cht is licensed under the MIT license.

## Credits

moka-cht is a fork of [cht v0.4.1][cht-v041], which is authored by Gregory Meyer.
cht v0.4.1 is licensed under the MIT license.

[cht-v041]: https://github.com/Gregory-Meyer/cht/tree/v0.4.1
