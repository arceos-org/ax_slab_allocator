# ax_slab_allocator

Slab allocator for `no_std` systems. It uses multiple slabs with blocks of different sizes (64, 128, 256, 512, 1024, 2048, 4096 bytes) and a [buddy_system_allocator] for blocks larger than 4096 bytes.

**Requires nightly Rust** due to `#![feature(allocator_api)]`.

## Usage

Add to `Cargo.toml`:

```toml
[dependencies]
ax_slab_allocator = "0.3"
```

Use a nightly toolchain, e.g. `rustup run nightly cargo build`, or add `rust-toolchain.toml` with `channel = "nightly"`.

## Example

```rust,ignore
use ax_slab_allocator::Heap;
use core::alloc::Layout;

// Safety: heap region must be valid and exclusively owned.
let heap_start = 0x1000_0000_usize;
let heap_size = 0x10_0000; // 1 MiB, must be >= 0x8000 and multiple of 0x8000
let mut heap = unsafe { Heap::new(heap_start, heap_size) };

let layout = Layout::from_size_align(64, 8).unwrap();
let ptr = heap.allocate(layout).unwrap();
unsafe { heap.deallocate(ptr, layout) };
```

## License

MIT
