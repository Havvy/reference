## Procedural Macros

*Procedural macros* are syntax extensions that work by calling a [function] that
transforms [`TokenStream`s]. They are used to implement custom derivers for the
[`derive`] attribute and define [custom attributes]. [`TokenStream`s] are
defined in the [proc_macro crate] that comes with the compiler.

> Note: A tutorial for procedural macros can be found in [the book][procedural
> macro tutorial].

Procedural macros must be defined in a crate with the [crate type] of
`proc-macro`. The two kinds of procedural macros are attribute macros and
custom deriver macros.

### Attribute Macros

*Attribute macros* define new attributes the arbitrarily transform the [item] 
that the attribute is on.

Attribute macros are defined by a [public] function with
the `proc_macro_attribute` attribute that a signature of `(TokenStream,
TokenStream) -> TokenStream`. The first [`TokenStream`] is the attribute's
metaitems, not including the delimiting parenthesis. If the attribute is written
without a metaitem, the attribute [`TokenStream`] is empty. The second
[`TokenStream`] is of the rest of the item including other attributes on the
item. The returned [`TokenStream`] replaces the item. It may contain an
arbitrary number of items. The returned [`TokenStream`] cannot include any
[module] items or [macro] definitions.

```rust
#![crate_type = "proc_macro"]
extern crate proc_macro;

use proc_macro::TokenStream;

#[proc_macro_attribute]
/// This attribute does nothing.
pub fn idempotent(_attr: TokenStream, item: TokenStream) -> TokenStream {
    item
}
```

### Deriver Macros

*Deriver macros* define new derivers for [`derive`] by transforming the item
into trait implementations and other items.

Custom derivers are defined by a [public] function with the
`proc_maco_derive` attribute that takes an input of the type
[`proc_macro::TokenStream`] and returns a
[`proc_macro::TokenStream`].


Procedural macros involve a few different parts of the language and its
standard libraries. First is the `proc_macro` crate, included with Rust,
that defines an interface for building a procedural macro. The
`#[proc_macro_derive(Foo)]` attribute is used to mark the deriving
function. This function must have the type signature:

```rust,ignore
use proc_macro::TokenStream;

#[proc_macro_derive(Hello)]
pub fn hello_world(input: TokenStream) -> TokenStream
```

[`derive`]: attributes.html#derive
[`proc_macro::TokenStream`]: ../proc_macro/struct.TokenStream.html
[attributes]: attributes.html
[custom attributes]: attributes.html
[crate type]: linkage.html
[item]: items.html
[function]: items/functions.html
[macro]: macros.html
[module]: items/modules.html
[proc_macro crate]: ../proc_macro/index.html
[procedural macro tutorial]: ../book/2018-edition/appendix-04-macros.html#procedural-macros-for-custom-derive