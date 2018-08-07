# Unsafety

Unsafe operations are those that can potentially violate the memory-safety
guarantees of Rust's static semantics.

The following language level features cannot be used in the safe subset of
Rust:

- Dereferencing a [raw pointer].
- Reading or writing a [mutable static variable].
- Reading a field of a [`union`], or writing to a field of a union that isn't
  [`Copy`].
- Calling an unsafe function (including an intrinsic or foreign function).
- Implementing an unsafe trait.

[raw pointer]: types.html#pointer-types
[mutable static variable]: items/static-items.html#mutable-statics
[`union`]: items/unions.html
[`Copy`]: special-types-and-traits.html#copy
