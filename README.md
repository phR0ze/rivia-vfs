# rivia-vfs
[![license-badge](https://img.shields.io/crates/l/rivia-vfs.svg)](https://opensource.org/licenses/MIT)
[![build](https://github.com/phR0ze/rivia-vfs/workflows/build/badge.svg?branch=main)](https://github.com/phR0ze/rivia-vfs/actions)
[![codecov](https://codecov.io/gh/phR0ze/rivia-vfs/branch/main/graph/badge.svg?token=VRLD36UB7E)](https://codecov.io/gh/phR0ze/rivia-vfs)
[![crates.io](https://img.shields.io/crates/v/rivia-vfs.svg)](https://crates.io/crates/rivia-vfs)
[![Minimum rustc](https://img.shields.io/badge/rustc-1.30+-lightgray.svg)](https://github.com/phR0ze/rivia-vfs#rustc-requirements)

***Ergonomic facade for the Rivia Virtual FileSystem***

### Quick links
* [Usage](#usage)
  * [Rustc requirments](#rustc-requirements)
* [Contribute](#contribute)
  * [Dev Environment](#dev-environment)
    * [Automatic version](#automatic-version)
  * [Testing](#testing)
    * [Test in container](#test-in-container)
* [License](#license)
  * [Contribution](#contribution)
* [Backlog](#backlog)
* [Changelog](#changelog)

# Usage
1. Import the crate
   ```toml
   [dependencies]
   rivia-vfs = "0.2.6"
   ```
2. Use the crate
   ```rust
   use rivia_vfs::prelude::*;
   
   fn main() {
       // Simply remove this line to default to the real filesystem.
       vfs::set_memfs().unwrap();
   
       let config = load_config();
       assert_eq!(config, "this is a test");
       println!("VFS test passed");
   }
   
   // Load an example application configuration file using VFS.
   // This allows you to test with a memory backed VFS implementation during testing and with
   // the real filesystem during production.
   fn load_config() -> String {
       let dir = PathBuf::from("/etc/xdg");
       vfs::mkdir_p(&dir).unwrap();
       let filepath = dir.mash("rivia.toml");
       vfs::write_all(&filepath, "this is a test").unwrap();
       assert_eq!(vfs::config_dir("rivia.toml").unwrap().to_str().unwrap(), "/etc/xdg");
   
       if let Some(config_dir) = vfs::config_dir("rivia.toml") {
           let path = config_dir.mash("rivia.toml");
           return vfs::read_all(&path).unwrap();
       }
       "".into()
   }
   ```

### Rustc requirements
This minimum rustc requirement is driven by the enhancements made to [Rust's `std::error::Error`
handling improvements](https://doc.rust-lang.org/std/error/trait.Error.html#method.source)

# Contribute
Pull requests are always welcome. However understand that they will be evaluated purely on whether
or not the change fits with my goals/ideals for the project.

**Project guidelines**:
* ***Chaining*** - ensure Rust's functional chaining style isn't impeded by additions
* ***Brevity*** - keep the naming as concise as possible while not infringing on clarity
* ***Clarity*** - keep the naming as unambiguous as possible while not infringing on brevity
* ***Performance*** - keep convenience functions as performant as possible while calling out significant costs
* ***Speed*** - provide ergonomic functions similar to rapid development languages
* ***Comfort*** - use naming and concepts in similar ways to popular languages

## Dev Environment

### Automatic version
Enable the git hooks to have the version automatically incremented on commits

```bash
cd ~/Projects/rivia-vfs
git config core.hooksPath .githooks
```

## Testing

### Test in container
TBD

# License
This project is licensed under either of:
 * MIT license [LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT
 * Apache License, Version 2.0 [LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0

## Contribution
Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in
this project by you, shall be dual licensed as above, without any additional terms or conditions.

---

# Backlog

# Changelog