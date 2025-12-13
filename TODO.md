# TODO.md

This file serves as both a TODO list and a changelog.  

---

## Date Range
**2025-11-25 - 2025-12-05**

---

## TODO

- [/] Implement the Vox runtime (VRT)
- [/] Implement variables and constants
- [/] Implement FFI
- [/] Implement Dynamic stubbing
- [o] Implement function definitions
  - [/] Code in functions
  - [/] Returning static constants
  - [ ] Returning non-static constants and variables
- [/] Implement function declarations
- [o] Implement if statements
  - Go to **Implement binary expressions**
- [o] Implement pointer dereferences
  - [/] Basic pointers
  - [ ] Pointer to a struct
  - [ ] Pointer to a function
  - [ ] Maybe pointer (`prptr`)
- [o] Implement preprocessors
  - [/] Inbuilt `#ifdef`
  - [ ] Custom `#ifdef` conditions
  - [ ] Macros
- [/] Implement type aliases
- [o] Implement binary expressions
  - [/] Static conditions
  - [/] Dynamic (bool only) conditions
  - [ ] Dynamic, non-bool, general conditions
- [o] Useful errors
  - [/] Line number
  - [ ] Filename
  - [/] Line
- [/] Constant folding
- [o] Structs
  - [/] Primitive attributes
  - [ ] Non-primitive attributes

---

## Changelog / Progress Key
- `/` → Done  
- `o` → Partially done  
- Blank → Todo
