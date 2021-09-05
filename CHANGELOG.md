# moka-cht &mdash; Change Log

## Version 0.5.0

### Changed

- Updated the dependencies. ([#1][gh-pull-0001])
- (Internal change) Replaced deprecated `Atomic::compare_and_set_weak` of
  crossbeam-epoch with `Atomic::compare_exchange_weak`. ([#1][gh-pull-0001])


## Version 0.4.2

- Forked from [cht-v0.4.1][cht-v041]. (The MIT License)
- Changed to a dual license of the MIT License and the Apache License (Version
  2.0). ([#1][gh-pull-0001])

### Changed

- Changed the default hasher from aHash to SipHash 1-3. ([#1][gh-pull-0001])

### Removed

- Removed the dependency to aHash crates. ([#1][gh-pull-0001])


<!-- Links -->

[cht-v041]: https://github.com/Gregory-Meyer/cht/tree/v0.4.1

[gh-pull-0001]: https://github.com/moka-rs/moka-cht/pull/1/
