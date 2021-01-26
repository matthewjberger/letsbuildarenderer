# Vulkan Backend

Now that the `Render` trait exists, we will need to create a backend that implements it.

## Render Backend Feature Flags

To allow compiling a specific backend, we will use [feature flags](https://doc.rust-lang.org/cargo/reference/features.html). For the purpose of this book, we will only be implementing the `Vulkan` backend so it will be a default feature.

```toml
# crates/obsidian_render/Cargo.toml
[features]
default = ["vulkan"]
vulkan = [] 
```

## Vulkan Render Backend

Create a new module named `vulkan.rs`

```bash
touch crates/obsidian_render/src/vulkan.rs
```

Update the `crates/obsidian_render/src/lib.rs` to list the new module:

```rust,noplaypen
#[cfg(feature = "vulkan")]
mod vulkan;
```

And declare the `VulkanRender` as a plain struct that implements the `Render` trait:

```rust,noplaypen
// crates/obsidian_render/src/vulkan.rs
use crate::Render;
use anyhow::Result;
use raw_window_handle::HasRawWindowHandle;
use log::info;

pub(crate) struct VulkanRenderer;

impl Render for VulkanRenderer {
    fn render(
        &mut self,
        _dimensions: &[u32; 2],
    ) -> Result<()> {
        Ok(())
    }
}

impl VulkanRenderer {
    pub fn new(
        _window_handle: &impl HasRawWindowHandle,
        _dimensions: &[u32; 2],
    ) -> Result<Self> {
        info!("Created Vulkan render backend");
        Ok(Self{})
    }
} 
```

## Instantiating Graphics Backends

We can now write an associated method for the `Render` trait to provide a [trait object](https://doc.rust-lang.org/book/ch17-02-trait-objects.html) (some type implementing the `Render` trait) by specifying the desired backend.

```rust,noplaypen
// creates/obsidian_render/src/render.rs
#[cfg(feature = "vulkan")]
use crate::vulkan::VulkanRenderer;

impl dyn Render {
    pub fn create_backend(
        backend: &Backend,
        window_handle: &impl HasRawWindowHandle,
        dimensions: &[u32; 2],
    ) -> Result<impl Render> {
        match backend {
            Backend::Vulkan => VulkanRenderer::new(window_handle, dimensions),
        }
    }
}
```

