# Reflective PE COFF DLL loader

A loader is a program that loads some executable code (e.g. in ELF, PE COFF, or Mach-O formats) into memory so that it can be executed.

A reflective loader is such a loader that loads the executable code from a memory buffer, rather than from a file on disk.

```rust
use reflective_pe_dll_loader::{PeDll, Symbol};

let bytes: &[u8] = include_bytes!("../test-dlls/hello_world_lib.dll");
let pe_dll = PeDll::new(&bytes).unwrap();

let add: Symbol<extern "C" fn(i32, i32) -> i32> = {
    let symbol = pe_dll.get_symbol_by_name("add").unwrap();
    unsafe { symbol.assume_type() }
};

assert_eq!(add(1, 2), 3);
```

## Credits

It is largely based on the code from <https://www.joachim-bauch.de/tutorials/loading-a-dll-from-memory/>.