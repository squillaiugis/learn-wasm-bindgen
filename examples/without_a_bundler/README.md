# wasm-bindgen Without a Bundler

## Overview
This example demonstrates how to run Rust WebAssembly code directly in a browser without using bundlers like Webpack or other JavaScript bundling tools.

## Setup

### Required Dependencies
Add the following to your `Cargo.toml`:
```toml
[dependencies.web-sys]
version = "0.3.4"
features = [
  'Document',
  'Element',
  'HtmlElement',
  'Node',
  'Window',
]
```

## Implementation Methods

### `--target web` Method (Recommended)
1. **Rust Code**:
   - Implement initialization using the `#[wasm_bindgen(start)]` function
   - Use `web_sys` for DOM manipulation

2. **Loading in HTML**:
   ```javascript
   import init, { add } from './pkg/without_a_bundler.js';
   
   async function run() {
     await init();  // Initialize the Wasm module
     const result = add(1, 2);  // Use exported functions
   }
   ```

3. **Build and Run**:
   ```sh
   wasm-pack build --target web
   python3 -m http.server 8080  # or miniserve . --index "index.html" -p 8080
   ```

### `--target no-modules` Method (Legacy)
1. **Limitations**:
   - Does not support local JS snippets
   - Does not generate ES modules
   - Does not support `--split-linked-modules` outside a document (e.g., in workers)

2. **Loading in HTML**:
   ```html
   <script src='pkg/without_a_bundler_no_modules.js'></script>
   <script>
     const { add } = wasm_bindgen;
     
     async function run() {
       await wasm_bindgen();  // Initialize
       const result = add(1, 2);
     }
   </script>
   ```

## Important Notes
- Due to CORS restrictions, you cannot open HTML files directly in a browser
- Always use a local web server for testing
- The `web-sys` crate is required for DOM manipulation from Rust/WebAssembly

## TODO
- Fix WebAssembly.Table.grow error that occurs in Chrome 
- Check if changing edition from 2024 to 2021 in Cargo.toml resolves the issue
- Verify wasm-bindgen version compatibility with current setup
- Test in different browsers to isolate if this is a Chrome-specific issue
