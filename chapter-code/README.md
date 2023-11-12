# Chapter source code

This folder contains the source code used in the book.

## Viewing the source code

To view the source code from each chapter, navigate to the corresponding subdirectory in this
directory. Each subdirectory starting with a number corresponds to a chapter of the same number and
name.

Some chapters contain multiple examples, in which case the source code will be subdivided inside
the chapter folder. For example, in case of `images`, there will be two examples:
`chapter-code/05-images/image_clear.rs` and `chapter-code/05-images/mandelbrot.rs`.

## Running the source code

If you want to run the source code and experiment for yourself, run `cargo run --bin <chapter>`
inside this folder. For example:

```bash
cargo run --bin windowing
```
