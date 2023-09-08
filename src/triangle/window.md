# Getting a Window Up
Window mangment is quite hard to do by hand. That is why there are libraries like [`SDL2`](https://www.libsdl.org) to help can do the cross plateform stuff for us. Let's get to Opengl already, so lets hurry.

We need to initialize `SDL2` and [`OpenGL`], but we also need to catch any errors that happen.
```rust
// Makes it more like C and easier to follow other tutorials
use sdl2_sys::*;
    
fn main() {unsafe {
    // initializes sdl returns 0 on sucess and anything else is failure
    if SDL_Init(SDL_INIT_EVERYTHING) != 0 {
        panic!("Error: Something idk"); // You can handle this
    }

    if 
        SDL_GL_SetAttribute(SDL_GLattr::SDL_GL_CONTEXT_MAJOR_VERSION, 3) != 0 || // sets the major version to 3
        SDL_GL_SetAttribute(SDL_GLattr::SDL_GL_CONTEXT_MINOR_VERSION, 3) != 0 || // sets the major version to 3
        SDL_GL_SerAttribute(SDL_GLattr::SDL_GL_CONTEXT_PROFILE_MASK, SDL_GLprofile::SDL_GL_CONTEXT_PROFILE_CORE as i32) != 0 // makes the profile core and only use Opengl 3.3
    {
        panic!("error");
    }
}
```

Now, that we have set up [`SDL2`](https://www.libsdl.org).Time to create an [`OpenGL`](https://www.khronos.org/opengl/wiki/) window.
```rust
// Main Function
    let title = CString::new("OpenGL Window").unwrap(); // We need to pass the title as a *const char for C
    let window = SDL_CreateWindow(title.as_ptr(), 500, 500, 500, 500, SDL_WindowFlags::SDL_WINDOW_OPENGL); // You can add more flags if you want by doing this 
    /*
        flag1 | flag2
    */
```