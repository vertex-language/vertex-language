<p align="center">
  <img src="docs/assets/banner.png" alt="Vertex" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Spec-2.2-141B3D?style=flat-square&labelColor=4DD9EC" alt="Spec 2.2">
  <img src="https://img.shields.io/badge/Compiler-0.4.0-4DD9EC?style=flat-square&labelColor=141B3D" alt="Compiler 0.4.0">
  <img src="https://img.shields.io/badge/Platforms-7-141B3D?style=flat-square&labelColor=F2D9E3" alt="7 Platforms">
  <img src="https://img.shields.io/badge/License-MIT-141B3D?style=flat-square&labelColor=E8F7F5" alt="MIT License">
</p>

<h3 align="center">A statically-typed, general-purpose programming language with object-oriented, asynchronous, and accelerated (GPU/kernel) programming support.</h3>

<p align="center">
  <a href="https://github.com/vertex-language/vertex">Repository</a> ·
  <a href="https://github.com/vertex-language/vertex#readme">Grammar Summary</a> ·
  <a href="https://github.com/vertex-language/vertex/releases">Releases</a>
</p>

---

### Overview

Vertex is a compiled, statically-typed language targeting seven platforms from a single grammar: `windows`, `linux`, `darwin`, `wasm`, `android`, `js`, and freestanding. A `namespace` block at the top of a file declares its target — memory model, platform, runtime, and backend — and the compiler checks that declaration against the build.

Classes provide object-oriented structure with deterministic destructors. `kernel func` and `graph func` support GPU-accelerated code, lowering to PTX, MSL, or StableHLO depending on backend. Control flow, generics, enums, and tuples follow familiar syntax from C-family and TypeScript-family languages.

```vertex
package math

export func fib(n: int) -> int {
  if n <= 1 {
    return n
  }

  var a = 0
  var b = 1
  var i = 2
  while i <= n {
    let next = a + b
    a = b
    b = next
    i += 1
  }
  return b
}

```

### Platforms

| Native | Host |
| --- | --- |
| `windows` · `linux` · `darwin` · `wasm` | `android` · `js` |
| pointer family (`*const T` / `*mut T`), manual memory, layout control, destructors | runtime-managed memory, foreign object graph access |

### Language features

* Static types with explicit numeric conversions (no implicit widening or narrowing)
* Object-oriented `class` with ARC and deterministic destructors; `struct` for value types
* Generics using square brackets (`[T]`), including const generics
* `kernel func` / `graph func` for accelerated (CUDA, Metal, StableHLO) code
* Optional-based absence (`T?` and `if let`) and return-type failure handling, no exceptions
* Foreign function interface via `declare package` for C, C++, Objective-C, JVM, and JS

### Get started

```sh
GOPROXY=direct go install [github.com/vertex-language/vertex@latest](https://github.com/vertex-language/vertex@latest)
vertex run main.vs

```

Full grammar and language tour: [github.com/vertex-language/vertex](https://github.com/vertex-language/vertex#readme)