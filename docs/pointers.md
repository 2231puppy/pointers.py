# Using Pointers

## Dereferencing

Ok, we know how to create pointers, but they aren't very useful if we only have pointer objects.

Pointer.py has a few different ways to dereference a pointer object. The simplest one is the `Pointer.dereference()` method:

```py
from pointers import Pointer, decay

@decay
def test(ptr: Pointer[str])
    print(ptr.dereference()) # prints "abc"

test("abc")
```

Unfortunately, calling dereference takes a _minimum_ of 15 characters, which can get messy very quickly.

The recommended way to dereference a pointer object is to use the `~` operator, which looks a lot nicer:

```py
@decay
def test(ptr: Pointer[str])
    print(~ptr) # much shorter and easier to type
```

Finally, you can use the `*` operator, like in low level languages:

```py
@decay
def test(ptr: Pointer[str])
    print(*ptr) # works the same as above
```

### Why should I use `~` over `*`?

The `~` operator is a **unary operator**, opposed to `*` which is for iterators.

This can cause some issues. For example:

```py
@decay
def test(ptr: Pointer[str])
    deref = ~ptr # valid, runs correctly
    deref = *ptr # invalid syntax error!

test("abc")
```

The `*` can also cause issues with iterability of the dereferenced object.

For example, if we have a `list` that we are trying to pass into a function, using the `*` operator steals the iterability:

```py
@decay
def test(ptr: Pointer[list])
    some_func(*ptr) # this only dereferences it, doesn't splat it
    some_func(**ptr) # this is treated as a kwarg splat, raises a typeerror

test([1, 2, 3])
```

## Assignment

If we would like to switch where a pointer is pointing to, we can use the `assign` method:

```py
from pointers import to_ptr

a = to_ptr("a")
b = to_ptr("b")

a.assign(b)
print(~a) # prints "b", since a is now pointing to the address of b
```

Note that this **does not** change the original value, it only changes what the pointer is looking at.

If you would like to change the original value, please see [movement](movement.md).

The `>>` operator can be used instead of calling `assign` manually as well:

```py
from pointers import to_ptr

a = to_ptr("a")
b = to_ptr("b")

a >>= b # does the same thing as above
```

In fact, we don't even need to create a second pointer when using `>>`.

```py
from pointers import to_ptr

a = to_ptr("a")
a >>= "b" # works just fine
```
