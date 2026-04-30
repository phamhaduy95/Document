# Dereferencing and Type Casting in C#

## 1. Dereferencing and Nulls
**Dereferencing** a variable means accessing one of its members (methods, properties, fields) using the dot (`.`) operator.

If you attempt to dereference a variable that is `null`, the runtime throws a `System.NullReferenceException`.

### Nullable Implementation Details
It is important to note that **Nullable Value Types** and **Nullable Reference Types** are implemented differently in .NET:

- **Nullable Value Types** (`int?`): Implemented using the `System.Nullable<T>` struct. `int?` is physically a different type than `int`.
- **Nullable Reference Types** (`string?`): Implemented using metadata/attributes read by the compiler. Both `string` and `string?` are physically the same `System.String` type at runtime.

---

## 2. Type Casting
Casting is the process of converting a variable from one type to another.

### Implicit Conversions
No special syntax is required because the conversion is guaranteed to succeed without data loss.
- **Smaller to larger types**: e.g., `int` to `long`.
- **Derived to Base classes**: e.g., `Dog` to `Animal`.

```csharp
int i = 10;
long l = i; // Implicit conversion

Dog dog = new Dog();
Animal animal = dog; // Implicit conversion (upcasting)
```

### Explicit Conversions (Casts)
Explicit conversions require a cast expression `(type)`. These are required when information might be lost or the conversion might fail.
- **Larger to smaller types**: e.g., `double` to `int` (truncates decimals).
- **Base to Derived classes**: e.g., `Animal` to `Dog` (requires a runtime check).

```csharp
double d = 9.75;
int i = (int)d; // Explicit cast: i becomes 9 (precision lost)

Animal animal = new Dog();
Dog dog = (Dog)animal; // Explicit cast (downcasting): succeeds if animal is actually a Dog
```
