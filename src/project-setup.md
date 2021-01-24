# Project Setup

The directory structure for the project will look like this:

![file-structure](images/file-structure.png)

## File Structure

* `assets/`
  * `hdr/`
    Contains HDR maps used for skyboxes and environment mapping
  * `models/`
    Contains 3D assets, such as \*.gltf and \*.glb files
  * `shaders/`
    Contains all of the shaders used in the project
* `crates/`
  * `obsidian_app/`
    A library that handles the window and application input boilerplate
* `viewer/`
    An application that can render 3D models, developed over the course of this book

## Creating the File Structure

Create the project as a library:

```bash
cargo new --lib obsidian
cd obsidian
```

Create the asset folders:

```bash
mkdir assets
mkdir assets/hdr
mkdir assets/icon
mkdir assets/models
mkdir assets/shaders
```

Edit `Cargo.toml` to make the project a cargo workspace:

```toml
# Leave the [package] section as is

[workspace]
members = ["crates/*", "viewer"]
default-members = ["viewer"]

[dependencies]
obsidian_app = { path = "crates/obsidian_app" }
```

Create the application crate as a library:

```bash
cargo new --lib crates/obsidian_app
```

Create and edit `crates/obsidian_app/src/app.rs` to contain:

```rust,noplaypen
pub struct Application;
```

Update `crates/obsidian_app/src/lib.rs`:

```rust,noplaypen
mod app;
pub use self::app::*;
```

Edit `src/lib.rs` to reference the app library:

```rust,noplaypen
pub mod app {
    pub use obsidian_app::*;
}
```

Create the viewer application:

```bash
cargo new viewer
```

Update the `viewer/cargo.toml` to reference all of obsidian as a single library.

```toml
[dependencies]
obsidian = { path = ".." }
```

Edit the `viewer/main.rs` file to use a type from the app library:

```rust,noplaypen
use obsidian::app::Application;

fn main() {
    let _app = Application {};
    println!("Hello, world!");
}
```

Now, run `cargo run --release` to compile and statically link the program. You should see:

```bash
"Hello, world!"
```
