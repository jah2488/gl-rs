_NOTE: this loader does not currently work with the latest version of Rust but will soon. This is because we are waiting for external function pointer calling to be implemented. This should land soon though, so stay tuned!_

# gl-rs

An OpenGL function pointer loader for the Rust Programming Language.

## Usage

### Pointer-style loader

You can import the pointer style loader and give it a nice name like so:

~~~rust
extern mod gl (name = "gl_ptr");
use gl;
use gl::types::*;
~~~

You can load each OpenGL symbol `load_with`function. You must supply a loader function from your context library, for example, `glfwGetProcAddress` or `SDL_GL_GetProcAddress`.

This is an example of what it would look like using [glfw-rs](https://github.com/bjz/glfw-rs):

~~~rust
unsafe { gl::load_with(glfw::get_proc_address) };
~~~

If the symbol could not be loaded, it is assigned a failing function. Calling a function that has not been loaded will result in a failure like: `fail!("gl::Viewport was not loaded")`, which aviods a segfault and provides useful information as to the cause of the error. This feature does not cause any run time overhead.

This is how you access an OpenGL enum:

~~~rust
gl::RED_BITS
~~~

And this is how you call a function:

~~~rust
gl::Viewport(0, 0, 600, 480);
~~~

Functions that take pointers are marked as `unsafe`.

Each function pointer has a boolean value associated with allowing you to check if a function has been loaded at run time:

~~~rust
if gl::Viewport::is_loaded() {
    //...
}
~~~

### Struct-style loader

_Todo_