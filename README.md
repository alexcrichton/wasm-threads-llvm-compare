# Comparing threaded implementations with different LLVM builds

This repository contains a compiled version of the [wasm-bindgen
raytrace-parallel
example](https://rustwasm.github.io/docs/wasm-bindgen/examples/raytrace.html)
which is compiled with LLVM 8 and LLVM 9. The main crux of this repository is
that LLVM 8's build completes in about 300ms per frame (on my computer) but the
LLVM 9 build takes ~2000ms.

* [LLVM 8 online](https://alexcrichton.github.io/wasm-threads-llvm-compare/llvm-8/)
* [LLVM 9 online](https://alexcrichton.github.io/wasm-threads-llvm-compare/llvm-9/)

The problem seems to be that LLVM 9 is emitting `memory.copy` instructions while
LLVM 8 did not do so. with LLVM 8 there is a Rust-written `memcpy` function that
was compiled to wasm, and with LLVM 9 this function isn't linked in because it
isn't needed.

Profiling historically showed that with the LLVM 9 version 80+% of the time is
spent in copying memory in one form or another.

