# Small Wasm Example

This example demonstrates how to create small WebAssembly binaries using `wasm-bindgen`.

## Key Points

- `wasm-bindgen` follows a "pay-only-for-what-you-use" philosophy
- The example implements a simple addition function:
  ```rust
  #[wasm_bindgen]
  pub fn add(a: u32, b: u32) -> u32 {
      a + b
  }
  ```

## Size Optimization Tips

1. **Build in Release Mode**
   ```bash
   wasm-pack build --release
   ```

2. **Enable LTO in Cargo.toml**
   ```toml
   [profile.release]
   lto = true
   ```

3. **Use wasm-opt (optional)**
   ```bash
   wasm-opt -Os pkg/add_bg.wasm -o pkg/add.wasm
   ```

## Note
The actual size of the generated Wasm binary may vary depending on the version of tools and build configurations used.

## TODO
- Investigate why size optimization is not as effective as documented (current: ~1.3KB vs documented: 710B -> 172B)
- Research additional optimization techniques for Wasm binary size reduction
