<p align="center">
  <img src="docs/assets/banner.png" alt="Vertex" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Source-.vs-141B3D?style=flat-square&labelColor=4DD9EC" alt="Source .vs">
  <img src="https://img.shields.io/badge/IR-.vir-4DD9EC?style=flat-square&labelColor=141B3D" alt="IR .vir">
  <img src="https://img.shields.io/badge/Targets-2-141B3D?style=flat-square&labelColor=F2D9E3" alt="2 Targets">
  <img src="https://img.shields.io/badge/License-MIT-141B3D?style=flat-square&labelColor=E8F7F5" alt="MIT License">
</p>

<h3 align="center">A statically typed systems programming language with ARC memory, ownership-aware methods, and compute execution modifiers, compiled straight to native binaries with no external toolchain.</h3>

<p align="center">
  <a href="https://github.com/vertex-language/vsc">Compiler</a> ·
  <a href="https://github.com/vertex-language/vsc/blob/main/docs/vertex_spec.md">Language Specification</a> ·
  <a href="https://github.com/vertex-language/ir">Vertex IR</a> ·
  <a href="https://github.com/vertex-language/vscode-vertex">VS Code</a>
</p>

---

### Overview

Vertex is a statically typed systems language with an ARC memory model, value and reference types, generics, and structured concurrency. It has its own package model, folder imports, receiver methods with explicit ownership, lowercase primitive types, relaxed argument labels, and compute execution modifiers. Vertex source lives in `.vs` files, and a module's public surface is described by a `.vertexinterface`.

The compiler, `vsc`, owns every stage in-process: scanning, type checking, ownership verification and definite initialization, lowering to Vertex IR (`.vir`), object emission, and linking. It produces Mach-O and PE/COFF executables directly — no system C compiler, assembler, or linker required.

```vertex
package main

struct Vec2 {
    var x: float32
    var y: float32
}

func (v: borrowing Vec2) dot(_ other: Vec2) -> float32 {
    return v.x * other.x + v.y * other.y
}

func (v: inout Vec2) scale(by factor: float32) {
    v.x *= factor
    v.y *= factor
}

func fib(_ n: int32) -> int32 {
    if n <= 1 { return n }
    var a: int32 = 0
    var b: int32 = 1
    for _ in 2...n {
        let next = a + b
        a = b
        b = next
    }
    return b
}

func main() -> int32 {
    var p = Vec2(x: 3, y: 4)
    p.scale(by: 2)
    return int32(p.dot(p)) / 10 + fib(10)   // exits with 65
}
```

### Platforms

| Target | Architecture | Format | System link |
| --- | --- | --- | --- |
| `aarch64-macos` | ARM64 | Mach-O | macOS SDK `libSystem.tbd` |
| `x86_64-windows` | x86-64 | PE/COFF | MSVC CRT (`libcmt`, `libucrt`) |
| `-freestanding` | either | either | none — no SDK, no C runtime |

### Language features

* Stable module interfaces — `.vertexinterface` files carry a module's public surface with bodies stripped
* Lowercase primitive spellings (`bool`, `int32`, `uint8`, `float32`, `double`, `string`, `char`, `never`, `any`) as built-in types
* Receiver methods with explicit ownership: `func (v: borrowing T)`, `inout`, `consuming`
* `package` declarations and folder imports: `import "std/fmt"`, `import geom "./geometry"`, grouped `import ( … )`
* Argument labels optional at call sites when the call is unambiguous
* `struct` value types, `class` reference types with ARC and `deinit`, `enum`, `protocol`, generics, `async`/`await`, typed `throws`
* `kernel` and `graph` execution modifiers — parsed and checked today, refused at lowering until a compute backend lands
* One target-independent IR, [Vertex IR](https://github.com/vertex-language/ir), shared with the `vcc` C and `vcx` C++ compilers

### Get started

`vsc` builds against its sibling repositories, so clone them side by side:

```sh
for r in vsc ir amd64 arm64 i386 asm macho pe elf vcc vcx; do
  git clone https://github.com/vertex-language/$r
done
cd vsc/cmd && go build -o bin/vsc ./vsc && export PATH="$PWD/bin:$PATH"

vsc run main.vs
vsc build -o app main.vs
vsc build --emit vir -o main.vir main.vs
```

Full specification: [vsc/docs/vertex_spec.md](https://github.com/vertex-language/vsc/blob/main/docs/vertex_spec.md) · Compiler guide: [github.com/vertex-language/vsc](https://github.com/vertex-language/vsc#readme)
