# SMART_ASSERT

A lightweight, multi-expression assertion macro for C and C++, with optional debug-only usage.

##  Why use this?

A simple and powerful assertion macro that improves debug visibility with minimal
 code by allowing multiple conditions in a single statement — while keeping production builds fast and clean.


## Features

- Supports **multiple conditions** in one assert call
- Prints exactly **which condition(s) failed**
- Includes **file**, **line**, and **function name**
- Works in **C99** and **C++20** (or newer)
- Fully **header-only**, zero dependencies
- Compatible with both C and C++
- Offers optional debug-only behavior via `DEBUG_SMART_ASSERT(...)`


## 🔧 Usage

Include the header:

```c
#include "smart_assert.h"
```

To enable debug-only assertions, define `DEBUG_MODE` **before** including the header:

```c
#define DEBUG_MODE
#include "smart_assert.h"
```

---

##  Macro Overview

### `SMART_ASSERT(...)`

Evaluates one or more conditions and prints a detailed failure message for each failed condition.  
**Always enabled**, regardless of build configuration.

```c
SMART_ASSERT(x > 0, y == 5, is_valid());
```

### `DEBUG_SMART_ASSERT(...)`

A wrapper macro for `SMART_ASSERT(...)` that is:
- Enabled **only when** `DEBUG_MODE` is defined
- Disabled otherwise — but still evaluates the expressions (side effects are preserved)

Example:

```c
DEBUG_SMART_ASSERT(x++ < 10); // x++ still happens in release
```

---

##  Example

```c
#define DEBUG_MODE
#include "smart_assert.h"

int main(void) {
    int a = 1, b = 2, c = 0;

    // This will print failures for b == 3 and c++ == 1
    DEBUG_SMART_ASSERT(a == 1, b == 3, c++ == 1);

    return 0;
}
```

### Output:
```
main.c:8: main: SMART_ASSERT(): Condition #2: `b == 3` failed.
main.c:8: main: SMART_ASSERT(): Condition #3: `c++ == 1` failed.
```

---

## ⚙️ Behavior When `DEBUG_MODE` Is Not Defined

- `DEBUG_SMART_ASSERT(...)` becomes:
  ```c
  (void)(__VA_ARGS__); // All expressions are still evaluated
  ```
- No assertions, no messages, no aborts
- Useful for maintaining debug logic without performance cost in production

---

## 📦 Integration Steps

1. Copy `smart_assert.h` to your project (e.g. `include/` folder)
2. Optionally define `DEBUG_MODE` in debug builds
3. Use `SMART_ASSERT(...)` for always-on checks
4. Use `DEBUG_SMART_ASSERT(...)` for debug-only assertions

---

## 📋 Requirements

| Feature              | Requirement             |
|----------------------|--------------------------|
| C Standard           | C99 or newer (`_Bool`, `_Static_assert`, variadic macros) |
| C++ Standard         | C++20 or newer (`__VA_OPT__`) |
| Compiler             | GCC, Clang, or compatible |


## 🔒 License

MIT License © 2024 [Georgios Evangelinos](https://github.com/GEvangelinos)

---