# Creating the Application

To make our application boilerplate reusable, we created the `obsidian_app` library. Now, we can start filling this out.

## Configuring the Application

Add an application configuration struct:

```rust,noplaypen
pub struct AppConfig {
    pub width: u32,
    pub height: u32,
    pub title: String,
}

impl Default for AppConfig {
    fn default() -> Self {
        Self {
            width: 800,
            height: 600,
            title: "Obsidian Application".to_string(),
        }
    }
}
```

For error handling, we'll be using the [anyhow](https://github.com/dtolnay/anyhow) crate.

To setup a window, we'll be using the [winit](https://github.com/rust-windowing/winit) crate. 

We will need to list these libraries as dependencies in `crates/obsidian_app/Cargo.toml` under the `[dependencies]` section:

```toml
[dependencies]
anyhow = "1.0.38"
winit = "0.24.0
```

Using the `AppConfig`, we can create a `Window` and `EventLoop<()>` in `crates/obsidian_app/src/app.rs`:

```rust,noplaypen
use anyhow::Result;
use winit::{dpi::PhysicalSize, event_loop::EventLoop, window::Window, window::WindowBuilder};

pub fn create_window(config: &AppConfig) -> Result<(EventLoop<()>, Window)> {
    let event_loop = EventLoop::new();

    let window = WindowBuilder::new()
        .with_title(config.title.to_string())
        .with_inner_size(PhysicalSize::new(config.width, config.height))
        .build(&event_loop)?;

    Ok((event_loop, window))
}
```
