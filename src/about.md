# Safe Rust wrapper around the Vulkan API

Vulkano is a Rust wrapper around [the Vulkan graphics API](https://www.khronos.org/vulkan/).
It follows the Rust philosophy, which is that as long as you don't use unsafe code you shouldn't
be able to trigger any undefined behavior. In the case of Vulkan, this means that non-unsafe code
should always conform to valid API usage.

What does vulkano do?

- Provides a low-levelish API around Vulkan. It doesn't hide what it does but provides some
  comfort types.
- Plans to prevent all invalid API usages, even the most obscure ones. The purpose of Vulkano
  is not to simply let you draw a teapot, but to cover all possible usages of Vulkan and detect all
  the possible problems in order to write robust programs. Invalid API usage is prevented thanks to
  both compile-time checks and runtime checks.
- Can handle synchronization on the GPU side for you (unless you choose to do that yourself), as this
  aspect of Vulkan is both annoying to handle and error-prone. Dependencies between submissions are
  automatically detected, and semaphores are managed automatically. The behavior of the library can
  be customized thanks to unsafe trait implementations.
- Tries to be convenient to use. Nobody is going to use a library that requires you to browse
  the documentation for hours for every single operation.

<br/>

[![Build Status](https://github.com/vulkano-rs/vulkano/workflows/Rust/badge.svg)](https://github.com/vulkano-rs/vulkano/actions?query=workflow%3ARust)
[![Discord](https://img.shields.io/discord/937149253296476201?label=discord)](https://discord.gg/xjHzXQp8hw)
[![vulkano github](https://img.shields.io/badge/vulkano-github-lightgrey.svg)](https://github.com/vulkano-rs/vulkano)
[![vulkano-book github](https://img.shields.io/badge/vulkano--book-github-lightgrey.svg)](https://github.com/vulkano-rs/vulkano-book)
<br/>
[![vulkano crates.io](https://img.shields.io/crates/v/vulkano?label=vulkano)](https://crates.io/crates/vulkano)
[![vulkano-shaders crates.io](https://img.shields.io/crates/v/vulkano-shaders?label=shaders)](https://crates.io/crates/vulkano-shaders)
[![vulkano-util crates.io](https://img.shields.io/crates/v/vulkano-util?label=util)](https://crates.io/crates/vulkano-util)
[![vulkano-win crates.io](https://img.shields.io/crates/v/vulkano-win?label=win)](https://crates.io/crates/vulkano-win)
<br/>
[![vulkano docs](https://img.shields.io/docsrs/vulkano?label=vulkano%20docs)](https://docs.rs/vulkano)
[![vulkano-shaders docs](https://img.shields.io/docsrs/vulkano-shaders?label=shaders%20docs)](https://docs.rs/vulkano-shaders)
[![vulkano-util docs](https://img.shields.io/docsrs/vulkano-util?label=util%20docs)](https://docs.rs/vulkano-util)
[![vulkano-win docs](https://img.shields.io/docsrs/vulkano-win?label=win%20docs)](https://docs.rs/vulkano-win)
