# Chapter 1 Support Code

## 64 bit integer types

Chapter 1 - Preliminaries uses the following functions from `utils.py`:

```python
# Adds two numbers and returns the result as a signed 64-bit integer.
def add64(x, y) -> int:
    return to_signed(x + y)


# Subtracts two numbers and returns the result as a signed 64-bit integer.
def sub64(x, y) -> int:
    return to_signed(x - y)


# Negates a number and returns the result as a signed 64-bit integer.
def neg64(x) -> int:
    return to_signed(-x)


# Reads an integer from the user and returns it as a 64-bit signed number.
def input_int() -> int:
    # This can fail on bad user input
    x = int(input())
    x = min(max_int64, max(min_int64, x))
    return x
```

These functions depend on:

```python
min_int64 = -(1 << 63)
max_int64 = (1 << 63) - 1
mask_64 = (1 << 64) - 1
offset_64 = 1 << 63


def to_unsigned(x):
    """Converts a signed 64-bit integer to an unsigned 64-bit integer."""
    return x & mask_64


def to_signed(x):
    """Converts an unsigned 64-bit integer to a signed 64-bit integer."""
    return ((x + offset_64) & mask_64) - offset_64
```

Since Python's `int` is in fact a Big Integer and as such has infinite precision and no signedness,
it poorly imitates the behavior of a statically typed integer (`i64` and `u64` in this case).
These functions are meant to compel Python `int` to act like the aforementioned integer types
in an x86_64 environment.  
