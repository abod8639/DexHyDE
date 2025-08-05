# tree-sitter-tablegen

This crate provides an LLVM TableGen grammar for the [tree-sitter][]
parsing library. To use this crate, add it to the `[dependencies]` section of
your `Cargo.toml` file. (Note that you will probably also need to depend on the
[`tree-sitter`][tree-sitter crate] crate to use the parsed result in any useful
way.)

```toml
[dependencies]
tree-sitter = "~0.20.3"
tree-sitter-tablegen = "0.0.1"
```

Typically, you will use the [language][language func] function to add this
grammar to a tree-sitter [Parser][], and then use the parser to parse some code:

```rust
let code = r#"
multiclass ABSTRACT_SIMD_I<dag oops_r, dag iops_r, dag oops_s, dag iops_s,
                           list<dag> pattern_r, string asmstr_r,
                           string asmstr_s, bits<32> simdop,
                           Predicate simd_level> {
  defm "" : I<oops_r, iops_r, oops_s, iops_s, pattern_r, asmstr_r, asmstr_s,
              !if(!ge(simdop, 0x100),
                  !or(0xfd0000, !and(0xffff, simdop)),
                  !or(0xfd00, !and(0xff, simdop)))>,
            Requires<[simd_level]>;
}
"#;
let mut parser = Parser::new();
parser.set_language(tree_sitter_tablegen::language()).expect("Error loading TableGen grammar");
let parsed = parser.parse(code, None);
```

If you have any questions, please reach out to us in the [tree-sitter
discussions] page.

[language func]: https://docs.rs/tree-sitter-tablegen/*/tree_sitter_tablegen/fn.language.html
[parser]: https://docs.rs/tree-sitter/*/tree_sitter/struct.Parser.html
[tree-sitter]: https://tree-sitter.github.io/
[tree-sitter crate]: https://crates.io/crates/tree-sitter
[tree-sitter discussions]: https://github.com/tree-sitter/tree-sitter/discussions
