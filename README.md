Binding to Solace C library solclient-7.21.0.5
generated by bindgen.

Unfortunately I'm not allowed to distribute Solace C libs.
You must instead download them from https://solace.com/downloads/.
When downloading from that URL select filter Solace APIs
and download API for C.

I tested only API for Intel Mac.

Extract downloaded archive and copy Solace libraries
to directory `lib`.
To link them with your program add build script `build.rs`
with the following content:

```rust
use std::env;
use std::path::Path;

fn main() {
    let target = env::var("TARGET").unwrap();
    let manifest_dir = env::var("CARGO_MANIFEST_DIR").unwrap();
    let lib_dir = Path::new(&manifest_dir).join("lib").to_str().unwrap().to_string();

    if target == "x86_64-apple-darwin" {
        println!("cargo:rustc-link-lib=framework={}", "kerberos");

        println!("cargo:rustc-link-search=native={}", lib_dir);
        println!("cargo:rustc-link-lib=dylib={}", "crypto");
        println!("cargo:rustc-link-lib=dylib={}", "ssl");
        println!("cargo:rustc-link-lib=dylib={}", "solclient");
        println!("cargo:rustc-link-lib=dylib={}", "solclientssl");
    } else {
        panic!("Unknown target {}", target)
    }
}
```

If you're not using Intel Mac you will need to modify
the script, and it may not work at all :-/
