# Console Log Example

This example demonstrates three different approaches to using `console.log` from Rust in WebAssembly.

## 1. Bare Bones Approach

The basic approach using direct `wasm-bindgen` bindings:

```rust
#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console)]
    fn log(s: &str);
    
    #[wasm_bindgen(js_namespace = console, js_name = log)]
    fn log_u32(a: u32);
    
    #[wasm_bindgen(js_namespace = console, js_name = log)]
    fn log_many(a: &str, b: &str);
}

// Usage
log("Hello from Rust!");
log_u32(42);
log_many("Logging", "many values!");
```

## 2. Macro Approach

A more Rust-like approach using a macro similar to `println!`. Note that this approach still requires the extern binding:

```rust
#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console)]
    fn log(s: &str);
}

macro_rules! console_log {
    ($($t:tt)*) => (log(&format_args!($($t)*).to_string()))
}

// Usage
console_log!("Hello {}!", "world");
console_log!("1 + 3 = {}", 1 + 3);
```

## 3. web-sys Approach

The most convenient approach using the `web-sys` crate:

```rust
use web_sys::console;

// Usage
console::log_1(&"Hello using web-sys".into());
let js: JsValue = 4.into();
console::log_2(&"Logging arbitrary values looks like".into(), &js);
```

## Comparison

- **Bare Bones**: Offers fine-grained control and explicit bindings. Good for understanding how the binding works.
- **Macro**: Provides familiar Rust-style formatting. Useful for printf-style debugging. Requires underlying extern binding.
- **web-sys**: Most convenient and maintained approach. Recommended for production use.
