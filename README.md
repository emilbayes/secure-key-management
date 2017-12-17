# `secure-key-managment`

> A collection of modules for securely working with cryptographic keys and secrets

## Goals

It is important that you understand the goals and short-comings of this module,
so you can effectively incorporate it into your security model.

* This is not a replacement for a Hardware-Secure Module (HSM), which provides
  a physical trust boundary around your keys.
* This module is intended to provide secure defaults for storing keys on disk,
  retrieval into memory and management while kept in memory. It does so by:
  * Setting highly restricted access permissions for files on disk (`0400`).
  * It uses [Secure Buffers]() extensively. Keys are read directly from disk
    into secure memory, appropriate memory protection is applied to avoid
    accidental or malicious access, keys are never swapped to disk nor will they
    appear in core dumps.
  * Misuse is considered a fatal error and will crash the program. Fatal errors
    include:
    - Reading an unexpected number of key bytes
    - Changing bytes of a key
    - Accessing a key after it has been destroyed

## Usage

WIP - A unified API will eventually surface, composed of the following modules:

* [`secure-create-key`](https://github.com/emilbayes/secure-create-key)
* [`secure-destroy-key`](https://github.com/emilbayes/secure-destroy-key)
* [`secure-read-key`](https://github.com/emilbayes/secure-read-key)

## Secure Buffers

The modules in this suite use the Secure Buffers from
[`sodium-native`](https://github.com/sodium-friends/sodium-native) extensively.
Secure Buffers are a wrapper around `libsodium` secure memory, but with the same
interface as normal Node.js `Buffer`s. Secure Buffers take more space than
normal buffers, but with the benefit that overflows and underflows are detected
and that data is destroyed when the memory is released. Secure memory is also
marked as not being swappable, meaning the OS will not write it to disk when
swapping pages in and out of main memory, which could lead to accidental
exposure. Secure memory is also masked in case of a core dump. Secure memory can
also have memory protection applied, so you can control `noaccess`, `readonly`
and `readwrite` state of the memory, crashing the process if these protections
are broken.

## Install

```sh
npm install secure-key-managment
```

## License

[ISC](LICENSE)
