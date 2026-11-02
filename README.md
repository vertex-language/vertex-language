<p align="center">
  <img src="docs/assets/banner.png" alt="Vertex — a general-purpose programming language" width="100%">
</p>

Vertex is a general-purpose programming language. It is statically typed, reads
plainly, and compiles to native code with a single tool that needs nothing else
installed beside it.

[Compiler](https://github.com/vertex-language/vsc) ·
[Specification](https://github.com/vertex-language/vsc/blob/main/docs/vertex_spec.md) ·
[Editor support](https://github.com/vertex-language/vscode-vertex)

<img src="docs/assets/snippets/snippet-01.png" alt="A Vertex program: a Point struct, a borrowing Length method, an inout Scale method, and a call printing 10.0" width="100%">

## Getting started

The compiler, `vsc`, is written in Go 1.25 and builds from a set of sibling
checkouts:

```
mkdir vertex && cd vertex
for repo in vsc vcx ir asm air amd64 arm64 i386 amdgpu ptx elf macho pe; do
    git clone https://github.com/vertex-language/$repo
done
cd vsc/cmd && go build -o ~/bin/vsc ./vsc
```

A program can be a single file:

<img src="docs/assets/snippets/snippet-02.png" alt="Hello world: let name = &quot;world&quot;; print(&quot;Hello, \(name)!&quot;)" width="100%">

```
vsc run hello.vs
vsc build -o hello hello.vs
```

## The language

### Values

Types are written in lowercase, and collections are built in.

<img src="docs/assets/snippets/snippet-03.png" alt="Values: bool, int, float64, string, an array and a dictionary" width="100%">

Structs are values, classes are references, and enums carry data and are matched
exhaustively.

<img src="docs/assets/snippets/snippet-04.png" alt="An enum Shape with circle and rect cases, matched in a switch" width="100%">

### Methods

A method can be written beside a type or apart from it. Written apart, it says
how it holds its receiver: `borrowing` to read, `inout` to change, `consuming`
to take.

<img src="docs/assets/snippets/snippet-05.png" alt="Receiver methods: borrowing Length and inout Scale on Point" width="100%">

### Calls

A parameter is named once. A call may pass its arguments by name or in order.

<img src="docs/assets/snippets/snippet-06.png" alt="A function Area(width:height:) called by name and in order" width="100%">

### Protocols and generics

<img src="docs/assets/snippets/snippet-07.png" alt="A protocol Shape, a conforming Square, and a generic Largest function" width="100%">

### Errors

<img src="docs/assets/snippets/snippet-08.png" alt="Errors: a ParseError enum, a throwing ParseInt, and do/catch" width="100%">

### Concurrency

Tasks run with `async` and `await`, side by side with `async let`, and actors
keep their state to themselves.

<img src="docs/assets/snippets/snippet-09.png" alt="Concurrency: async let for two fetches, and an actor Counter" width="100%">

## Packages

A package is a folder, and a module is a folder with a `vs.mod`.

```
hello/
├── vs.mod
├── geometry/
│   └── geometry.vs
└── cmd/
    └── hello/
        └── main.vs
```

```
module example.com/hello

vertex 0.9
```

<img src="docs/assets/snippets/snippet-10.png" alt="A main package importing example.com/hello/geometry and printing a distance" width="100%">

```
vsc run ./cmd/hello
```

The standard library is a set of packages, each in its own repository under
[vertex-language](https://github.com/vertex-language), imported by name:

<img src="docs/assets/snippets/snippet-11.png" alt="Importing standard packages: import &quot;fs&quot; and import &quot;net/http&quot;" width="100%">

## Repositories

| | |
| :--- | :--- |
| [`vsc`](https://github.com/vertex-language/vsc) | The compiler |
| [`ir`](https://github.com/vertex-language/ir) | The intermediate representation and its lowerings |
| [`vcx`](https://github.com/vertex-language/vcx) | The C++ compiler for a package's native code |
| [`vscode-vertex`](https://github.com/vertex-language/vscode-vertex) | Editor support |

## License

[MIT](https://github.com/vertex-language/vsc/blob/main/LICENSE)
