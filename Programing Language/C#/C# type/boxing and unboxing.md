# Boxing and Unboxing

**Boxing** is the process of converting a **value type** (e.g., `int`, `struct`) to the type `object` (or any interface type implemented by the value type). This process stores the value on the managed heap.

**Unboxing** extracts the value type from the object. Boxing is implicit, while unboxing is explicit.

## Example

In the following example, the integer variable `i` is **boxed** and assigned to object `o`.

```csharp
int i = 123;

// The following line boxes i implicitly.
object o = i;
```

The object `o` can then be **unboxed** and assigned back to an integer variable:

```csharp
o = 123;
int j = (int)o; // Unboxing is explicit
```

## Performance Impact
Boxing and unboxing both have a negative impact on performance:
- **Boxing**: Requires a new heap allocation and copying the value.
- **Unboxing**: Requires a type check and copying the value from the heap.

Both operations are computationally expensive and should be avoided in performance-critical code (e.g., using **Generics** instead of `object` collections).
