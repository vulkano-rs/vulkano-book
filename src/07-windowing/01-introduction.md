# Windowing introduction

Up until now, we have only created applications that perform one quick action and then exit. What
we are going to do next is create a window in order to draw graphics on it, and keep our
application running forever until the window is closed.

Strictly speaking, creating a window and handling events is **not** covered by vulkano. Vulkano,
however, is capable of rendering to window(s).

> **Note**: You can find the [full source code of this chapter
> here](https://github.com/vulkano-rs/vulkano-book/blob/main/chapter-code/07-windowing/main.rs).

## Creating a window

In order to create a window, we will use the `winit` crate.

Add to your `Cargo.toml` dependencies:

```toml
winit = "0.30.0"
```

We encourage you to browse [the documentation of `winit`](https://docs.rs/winit/0.30.0/winit/).

Starting from version 0.30.0 winit switched from using an event-loop based system to an 
event-handler system. This means we have to create a struct that implements the 
`ApplicationHandler` trait and pass it into `EventLoop::run_app`.

We use two structs `App` and `RenderContext` to organize the objects necessary for rendering. `App` 
implements the `ApplicationHandler` trait and contains the window-independent objects. 
Objects that depend on a window are stored in the `RenderContext` struct.

We start with this minimal setup and add objects as we go:

```rust
struct App {
    rcx: Option<RenderContext>
}

struct RenderContext {
    window: Arc<Window>
}

impl App {
    pub fn new(_event_loop: &EventLoop<()>) -> Self {
        // initialize window independent objects here
        Self { rcx: None }
    }
}

impl ApplicationHandler for App {
    fn resumed(&mut self, event_loop: &winit::event_loop::ActiveEventLoop) {
        let window = Arc::new(
            event_loop
                .create_window(Window::default_attributes())
                .unwrap(),
        );
        // initialize window dependent objects here
        self.rcx = Some(RenderContext {
            window
        });
    }

    fn window_event(
        &mut self,
        event_loop: &winit::event_loop::ActiveEventLoop,
        _window_id: winit::window::WindowId,
        event: WindowEvent,
    ) {
        match event {
            WindowEvent::CloseRequested => {
                event_loop.exit();
            }
            _ => (),
        }
    }
}
```

We need to wrap the Window in an Arc, so that both you and vulkano can keep a reference to it.

To run the app, pass it into `EventLoop::run_app` like this:

```rust
fn main() -> Result<(), impl Error> {
    use winit::event_loop::EventLoop;

    let event_loop = EventLoop::new().unwrap();
    let mut app = App::new(&event_loop);

    event_loop.run_app(&mut app)
}
```

Running this program should give you a closeable window. 

## Create Vulkan instance

The next step is to create a Vulkan instance in the constructor function `App::new` and store the 
reference in the `App` struct.

Because the objects that come with creating a window are not part of Vulkan itself, the first thing
that you will need to do is to enable all non-core extensions required to draw a window.
`Surface::required_extensions` automatically provides them for us, so the only thing left is to
pass them on to the instance creation:

```rust
use vulkano::instance::{Instance, InstanceCreateFlags, InstanceCreateInfo};
use vulkano::swapchain::Surface;

let library = VulkanLibrary::new().expect("no local Vulkan library/DLL");
let required_extensions = Surface::required_extensions(&event_loop).unwrap();
let instance = Instance::new(
    library,
    InstanceCreateInfo {
        flags: InstanceCreateFlags::ENUMERATE_PORTABILITY,
        enabled_extensions: required_extensions,
        ..Default::default()
    },
)
.expect("failed to create instance");
```

Next we need to create a *surface*, which is a cross-platform abstraction over the actual window
object, that vulkano can use for rendering:

```rust
let surface = Surface::from_window(self.instance.clone(), window.clone()).unwrap();
```

Also store the surface reference in the `App´ struct.

<!-- todo: is this correct? -->

<!-- > **Note**: Since there is nothing to stop it, the window will try to update as quickly as it can,
> likely using all the power it can get from one of your cores.
> We will change that, however, in the incoming chapters. -->

Right now, all we're doing is creating a window and keeping our program alive for as long as the
window isn't closed. The next section will show how to initialize what is called a *swapchain* on
the window's surface.

Next: [Swapchain creation](02-swapchain-creation.html)
