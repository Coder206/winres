# winres

A simple library to facilitate adding metainformation and icons to windows
executables and dynamic libraries.

[Documentation](https://mxre.github.io/winres)

## Toolkit

Before we begin you need to have the approptiate tools installed.
 - `rc.exe` from the [Windows SDK]
 - `windres.exe` and `ar.exe` from [minGW64]
 
[Windows SDK]: https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk
[minGW64]: http://mingw-w64.org

If you are using Rust with the MSVC ABI you will need the Windows SDK,
for the GNU ABI you'll need minGW64.

Windows SDK can be found in the registry, minGW64 has to be in the path.

## Using winres

First, you will need to add a build script to your crate (`build.rs`)
by adding it to your crate's `Cargo.toml` file:

```toml
[package]
#...
build = "build.rs"

[build-dependencies]
winres = "0.1"
```

Next, you have to write a build script. A short
example is shown below.

```rust
// build.rs

extern crate winres;

fn main() -> io::Result<()> {
  if cfg!(target_os = "windows") {
    let mut res = winres::WindowsResource::new();
    res.set_icon("test.ico");
    res.compile().unwrap();
  }
}
```

Thats, it. The file `test.ico` should be located in the same directory as `build.rs`.
Metainformation, like program version and description are taken from `Cargo.toml`'s `[package]`
section.

## Additional Options

For added convenience, `winres` parses, `Cargo.toml` for a `package.metadata.winres` section:

```toml
[package.metadata.winres]
OriginalFilename = "PROGRAM.EXE"
LeagalCopyright = "Copyright © 2016"
#...
```

This section may contain arbitrary string key-value pairs, to be included
in the version info section of the executable/library file.

Some keys have special meanings, and will be shown in the file properties
of the Windows Explorer:

`FileDescription`, `ProductName`, `ProductVersion`, `OriginalFilename` and `LegalCopyright`

See [MSDN]
for more details on the version info section of executables/libraries.

[MSDN]: https://msdn.microsoft.com/en-us/library/windows/desktop/aa381058.aspx

## About this project

I've written this crate chiefly for my personal projects, and although I've tested it
on my personal computers I have no idea if the behaviour is the same everywhere.

To be brief, I'm very much reliant on your bug reports and feature suggestions
to make this crate better.
