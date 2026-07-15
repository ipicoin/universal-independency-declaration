# Universal Independency Declaration

An incubating, multi-language research workspace for expressing and testing
portable independence checks.

> **Status: concept skeleton.** The JavaScript, Python, Go, and Rust entry
> points are placeholders. This repository does not yet define a stable API,
> protocol, library, or normative declaration.

## Intended direction

The repository may become a small, implementation-neutral toolkit for checking
whether a system's independence claims are observable and reproducible. Any
normative definition must first be specified through an
[IPI Improvement Proposal](https://github.com/ipicoin/.github/tree/main/ipi).
The current foundation is
[IPI-0001: Verifiable Independence](https://github.com/ipicoin/.github/blob/main/ipi/IPI-0001.md).

Useful contributions at this stage include:

- defining one narrow, testable use case;
- designing a shared fixture format across supported languages;
- adding equivalent tests for each implementation; and
- documenting what a result proves and, equally importantly, what it does not.

## Development

Language manifests are present for JavaScript, Python, Go, and Rust, but there
is no supported release or unified build yet. Do not publish packages from this
repository until the API, tests, versioning policy, and release ownership are
recorded.

## License

Licensed under [Apache License 2.0](LICENSE). Project names and marks are
governed separately by the IPI
[trademark policy](https://github.com/ipicoin/.github/blob/main/TRADEMARKS.md).
