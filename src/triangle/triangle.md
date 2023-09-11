# Triangle
I know that this chapter name is misleading. We are not coding the "hello world" triangle. We still need to add more complex shaders in to get there. But this chapter is still one step closer to the goal.

## Loading
Some of you may know that we didn't add any [`Opengl`](https://www.khronos.org/opengl/wiki/) loading libraries. Well, we are going to do that now. [Click Here](https://gen.glad.sh/generated/tmpgwzdd52kglad/glad-gl/src/gl.rs) to get the [`OpenGL`](https://www.khronos.org/opengl/wiki/) file with right settings. Put it into your `src` and link it to the `main.rs`. Once you are down with that, create a [`OpenGL`](https://www.khronos.org/opengl/wiki/) context and get the functions.

```rust
// Window Create Function
    let _ = SDL_GL_CreateContext(window); // Doesn't need a name for now

    gl::load(|s| {
        let proc = CString::new(s).unwrap(); 
        SDL_GL_GetProcAddress(proc.as_ptr()) // Gets the pointer to functions
    })
```

## Clear

Now that all of the functions are loading we can get into [`OpenGL`](https://www.khronos.org/opengl/wiki/). We can test if the function work by covering the screen with a color. Using the  `gl::ClearColor`, which takes 4 parameters. They corresponds to each of the RGBA. We also need to use `gl::Clear` to choose the buffer to clear. In this case, the `gl::COLOR_BUFFER_BIT`. To refresh the window and to get the window to display the picture we need use `SDL_SwapWindow`.
```rust
// Loop
        gl::ClearColor(0.6, 0.7, 0.7, 1.0);
        gl::Clear(gl::COLOR_BUFFER_BIT);

        SDL_GL_SwapWindow(window);
```

## Triangle

[`OpenGL`](https://www.khronos.org/opengl/wiki/) renders objects on the screen by taking in data called vertices. 3 Vertices make a triangle. Something simple enough that a gpu can render it with no wasted time or processing power. That data goes through the vertex shader. The vertex shader is a shader that is given the vertex data and outputs the final vertex position. It runs for every vertex. New graphic programers may ask why does the vertex shader finalze the position when we give the position we want to it in the first place. We will talk about in a different chapter. Once the vertex shader is done the fragment shader takes hold. It runs for every single pixel that the triangle covers and chooses the color for the pixel. Like so:

    ==============================================================
    |               Vertex Shader  Frgament Shader     Screen    |
    |                ===========     ===========     ----------- |
    |                |   *     |     |   #     |     |   █     | |
    | Vertex Data -> |         | --> |  ####   | --> |  ████   | |
    |                | *     * |     | ####### |     | ███████ | |
    |                ===========     ===========     ----------- |
    ==============================================================

Lets start from left to right. To make vertex data we need some vertices. Each vertices needs three coordinates. X Y Z. Make a variable that holds an array of 9 f32.
```rust
    let vertices = [
         0.0,  0.5, 0.0,
         0.5, -0.5, 0.0,
        -0.5, -0.5, 0.0
    ];
```
Now, this may seem very weird. You may think that would only see part of the triangle, while the rest would be off screen either on the bottom left of top left. The thing is. [`OpenGL`](https://www.khronos.org/opengl/wiki/) coordinates system is different than most. 0, 0 isn't in a corner. Its in the middle of the window, regardless of the the windows size or shape. 1, 1 is top right and -1, -1 is bottom left. 

### VAO and VBO

To give this data to the gpu. We need to make a `Vertex Array Object`(or `vbo`) and a `Vertex Buffer Object`(or `vao`). The `vbo` is buffer that allows us to send large amounts of vertex data to the gpu. The `vao` is a object that contains the vertex data for gpu to process. 