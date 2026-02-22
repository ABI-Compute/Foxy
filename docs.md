# Foxy Language Documentation

Vox is a statically compiled systems programming language targeting LLVM IR. It is designed for low-level development with support for direct memory manipulation, C-style structs, and seamless interoperability with C libraries.

## Compiler Usage

The Foxy compiler is invoked via the `Foxy.py` script. It translates `.vpy` source files into LLVM IR, compiles them to object files using `llc`, and links them into executables or libraries.

### Command Line Interface

```bash
python Foxy.py [options] <source_file.vpy>
```

### Options

| Option | Description |
| :--- | :--- |
| `-o <file>` | Specify output filename (default: `main`). |
| `-arch <arch>` | Target architecture (default: `64`). |
| `-os <os>` | Target OS (e.g., `win`, `linux`). |
| `-mtriple <triple>` | Specify LLVM target triple (e.g., `x86_64-w64-mingw32`). |
| `--noconsole` | (Windows) Link as a GUI application (subsystem windows). |
| `--debug` | Enable debug symbols (`-g` passed to linker). |
| `--asm` | Output assembly code (`.asm` or `.s`) instead of binary. |
| `--llvm-ir` | Output LLVM IR (`.ll`) instead of binary. |
| `--obj` | Output object file (`.o` or `.obj`) only. |
| `--lib` | Output static library. |
| `--dyn` | Output dynamic library. |
| `--kernel` | Output freestanding kernel binary (nostdlib). |
| `--bin32` / `--bin64` | Output raw binary (useful for bootloaders). |
| `-OBJ <file>` | Link an external object file. |
| `-LIB <lib>` | Link an external library. |
| `-a <path>` | Add a search path to `vox_path.json` for global imports. |

## Language Reference

### Data Types

Foxy supports standard primitive types and composite types.

| Type | Description | LLVM Equivalent |
| :--- | :--- | :--- |
| `int`, `uint`, `int32`, `uint32` | 32-bit integer | `i32` |
| `int64`, `uint64` | 64-bit integer | `i64` |
| `char`, `uint8` | 8-bit integer/character | `i8` |
| `int16`, `uint16`, `wchar` | 16-bit integer | `i16` |
| `float` | 32-bit floating point | `double` |
| `float64` | 64-bit floating point | `double` |
| `bool` | Boolean | `i1` |
| `void` | Void type | `void` |

#### Pointers and Buffers
*   `ptr[T]`: Pointer to type `T`.
*   `buff[T; N]`: Fixed-size buffer (array) of `N` elements of type `T`.
*   `buff[T]`: Unspecified size buffer (decays to pointer).
*   `fnptr[Ret; Arg1, Arg2]`: Function pointer.

#### Type Aliasing
Use `using` to create type aliases.
```Foxy
using Handle = int64
```

### Variables and Constants

#### Constants
Constants are evaluated at compile-time or initialized as globals.
```Foxy
const PI: float = 3.14
const MSG: ptr[char] = addr "Hello World"
```

#### Variables
Global variables are defined with `var`.
```Foxy
var counter: int = 0
var buffer: buff[char; 256] = "Initial string"
```

### Structs

Structs define memory layout for composite data. Fields are separated by commas.

```Foxy
struct Point:
    x: int,
    y: int
endstruct
```

**Initialization and Usage:**
```Foxy
# Named initialization
const p: Point = Point { x: 10, y: 20 }

# Field access
var x_val: int = p.x
```

### Functions

Functions are defined with `fn` and end with `endfn`.

```Foxy
fn int add(a: int, b: int):
    return a + b
endfn
```

### Control Flow

#### Conditionals
```Foxy
if x == 10:
    # code
elif x > 10:
    # code
else:
    # code
endif
```

#### Loops and Jumps
Foxy currently uses labels and goto for loops.
```Foxy
lb loop_start
    if i >= 10:
        goto loop_end
    endif
    # body
    goto loop_start
lb loop_end
```

### Memory Operations

*   **Address Of**: `addr variableName`
*   **Dereference/Write**: `*(Address): Type = Value`
*   **Pointer Arithmetic**: `*(Base + Offset)`

```Foxy
# Write 0 to address 0x1234
*(0x1234): int = 0

# Dereference pointer variable
var ptrVal: ptr[int] = ...
*ptrVal: int = 10
```

### Interoperability (FFI)

Foxy can link against C libraries and import functions.

#### Linking Libraries
```Foxy
lib dyn <kernel32>   # Link dynamic library
lib static <mylib>   # Link static library
```

#### Importing Functions
External functions must be declared with `dyn_import`. Note the trailing dot.
```Foxy
dyn_import fn int MessageBoxA(hwnd: int64, text: ptr[char], caption: ptr[char], type: uint).
```

### Preprocessor

Foxy supports conditional compilation based on the target environment (`WIN32`, `LINUX`, `X86_64`, etc.).

```Foxy
%if WIN32
    import "windows_defs.vpy"
%elif LINUX
    import "linux_defs.vpy"
%endif
```

### Inline Assembly

```Foxy
ASM:
    mov eax, 1
    ret
asmend
```

