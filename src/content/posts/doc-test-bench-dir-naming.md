---
title: Inconsistent Naming Standards of Programming Langauges for Tests, Documentations, and Benchmarks
pubDatetime: 2026-11-02
modDatetime: 2026-11-02
tags: ["comment"]
description: ""
---

I am always unsure whether I should use the singular/plural form for their directory names.
Actually, the standards diverges. See the following table researched by AI:

**Singular vs. plural - directory names: written standards vs. repo reality** *(checked 2026-10-28; a cell is a link = defined in the linked written standard · italic = observed repo layout, no written standard · - = none defined)*

| Toolchains / Projects | Tests | Documentation | Benchmarks |
|---|---|---|---|
| [Maven](https://maven.apache.org/) | [`src/test/`](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) | [`src/site/`](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) | - |
| [Cargo](https://doc.rust-lang.org/cargo/) | [`tests/`](https://doc.rust-lang.org/cargo/guide/project-layout.html) | - | [`benches/`](https://doc.rust-lang.org/cargo/guide/project-layout.html) |
| [golang-standards/project-layout](https://github.com/golang-standards/project-layout) | [`test/`](https://github.com/golang-standards/project-layout) | [`docs/`](https://github.com/golang-standards/project-layout) | - |
| [golang/go](https://github.com/golang/go) | *`test/`* | *`doc/`* | - |
| [Linux kernel](https://www.kernel.org/) | [`tools/testing/selftests/`](https://docs.kernel.org/dev-tools/kselftest.html) | *`Documentation/`* | *`tools/perf/`* |
| [CPython](https://github.com/python/cpython) | *`Lib/test/`* | [`Doc/`](https://devguide.python.org/getting-started/setup-building/#directory-structure) | *`Tools/*bench/`* |
| [Node.js](https://github.com/nodejs/node) | [`test/`](https://github.com/nodejs/node/blob/HEAD/test/README.md) | *`doc/`* | [`benchmark/`](https://github.com/nodejs/node/blob/HEAD/benchmark/README.md) |
| [rust-lang/rust](https://github.com/rust-lang/rust) | [`tests/`](https://rustc-dev-guide.rust-lang.org/tests/intro.html) | *`src/doc/`* | - |
| [LLVM](https://llvm.org/) | [`llvm/test/`](https://llvm.org/docs/TestingGuide.html) | *`llvm/docs/`* | *`llvm/benchmarks/`* |
| [Django](https://github.com/django/django) | [`tests/`](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/unit-tests/) | *`docs/`* | - |

In my view, `test, doc, bench` and `tests, docs, benches` are both fine, since at least they are better than the inconsistent standards.
