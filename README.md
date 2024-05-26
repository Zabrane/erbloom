# erbloom
Safe and Fast Bloom Filter + FBFs for Erlang

![CI](https://github.com/Vonmo/erbloom/workflows/CI/badge.svg?branch=master)

[Online Documentation](https://hexdocs.pm/erbloom/)

## Features:
* [Bloom filter structure](https://en.wikipedia.org/wiki/Bloom_filter) (type: `bloom`)
* [Forgetful Bloom Filters](http://dprg.cs.uiuc.edu/docs/fbf_cac15/fbfpaper-2.pdf) (type: `fbf`)

## Supported OS:
* linux
* macos
* windows

## Deps definition:
mix.exs:
`{:erbloom, "~> 2.1.0-rc.2"}`

rebar.config:
`{erbloom, "2.1.0-rc.2"}`

erlang.mk:
`dep_erbloom = hex 2.1.0-rc.2`

rust 1.76.0:
```bash
## Installing multiple versions of Rust on Mac using Homebrew:
## https://www.chrisjmendez.com/2022/02/22/installing-multiple-versions-of-rust-on-your-mac-using-homebrew/
## https://users.rust-lang.org/t/how-to-install-older-version-toolchain/43223
## Ubuntu/Debian:
> apt install rustup
## macOS:
> brew install rustup

> rustup-init
> rustup install 1.76.0
> rustup default 1.76.0

## Check RUST version:
> rustc --version
rustc 1.76.0 (07dca489a 2024-02-04)
```

## Using as a lib
1. Add deps in rebar.conf:
  ```
  {deps, [
      {erbloom, ".*", {git, "https://github.com/Vonmo/erbloom.git", {tag, "v2.0.2"}}}      
  ]}
  ```
2. Now you can create a new filter instance:
  `{ok, Filter} = bloom:new(9585059,1000000).`
   or filter with wanted rate of false positives: 
   `bloom:new_optimal(1000000, 0.55).`
3. Create a new forgetful filter:
   `{ok, Filter} = bloom:new_forgetful(BitmapSize, ItemsCount, NumFilters, RotateAfter).`
   or with fp_rate:
   `bloom:new_forgetful_optimal(ItemsCount, NumFilters, RotateAfter, FpRate).`
3. Set a new element
  `bloom:set(Filter, "somekey").`
4. Check up element
  `bloom:check(Filter, "anotherkey").`
5. Serialize
   `{ok, Binary} = bloom:serialize(Filter).`
6. Deserialize
   `bloom:deserialize(Binary).`

## Docker environment
* `make build_imgs` - build docker images
* `make up` - run sandbox
* `make down` - terminate sandbox
* `make tests` - run tests
* `make lint` - linter
* `make xref` - xref analysis
* `make prod` - generate release for target
* `make doc` - generate documentation from EDoc

##
Without docker you must install erlang >=20.1 and rust >=1.23 on your machine. After you can run these goals:
**release:**
`rebar3 as prod release`

**test:**
`rebar3 as test ct`
