# Getting Started
This part of the tutorial will go over getting the proper libraries and toolchain for the rest of tutorial. 

```admonish note
We will not go how to install rust or learning rust. If you want to know that we would suggest you read the [Rust Book](https://doc.rust-lang.org/book/).
```

## SDL
If you know about [`learnopengl`](learnopengl.com), then you would assume that we are going to use the window mangment library [`glfw`](glfw.org), but in this tutorial we will be using [`SDL2`](https://www.libsdl.org). They are differnt libraries meant for different things. The reason this tutorial will be using SDL2 is because SDL2 allows for many different rendering systems and plateforms(and because Valve uses it too). So lets get to installing:

First, we need to make a our project. Make it:
```bash
cargo new example
```

Now, we need to choose the right library. You can make your own wrapper, but in this case we are going to use the `sdl2-sys` crate.
Open up your `cargo.toml` and add `sdl2-sys` like this:
```toml
[dependency.sdl2-sys]
version = "*" # you can choose what ever version you want
features = ["bundled", "static-link"] # this makes it so that you don't have to use sdl2.dll file with the exe
```

```admonish warning
If you are on windows you will need to change the toolchain to msvc
```

```admonish note
If you want your program to have `#![no_std]`, then you need to add `libc` to your project.
```

Now that we are done with installing everything we can move on.