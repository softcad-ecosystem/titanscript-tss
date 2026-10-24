# TitanScript STANDARD 2.5 — documentation & examples

[**Română**](README_RO.md) · [SoftCAD language reference](https://softcad.ro/en/tss-documentation)

**Status: documentation release, NOT a standalone runtime or a complete SDK.**

This repository packages the bilingual **TitanScript STANDARD 2.5.0 RC2.1** language reference and two sample `.tss` scripts already available from the SoftCAD public developer center. It does not ship the pure-Python interpreter, a Windows/macOS validated standalone runner, a compiler, the protected Shield/TSX product, or proprietary format codecs.

## Contents

- [`docs/tss-standard-2.5-en.md`](docs/tss-standard-2.5-en.md) — English syntax, modules and limits.
- [`docs/tss-standard-2.5-ro.md`](docs/tss-standard-2.5-ro.md) — referință română.
- [`examples/01_limbaj_standard_25.tss`](examples/01_limbaj_standard_25.tss) — language syntax and arithmetic.
- [`examples/02_baza_tdb3_standard_25.tss`](examples/02_baza_tdb3_standard_25.tss) — TDB3 example; **writes a local database**, run only with a compatible interpreter and on disposable test data.

The docs describe imports and functions verified in the existing STANDARD implementation. **No executable entry point is supplied by this repository.** The current SoftCAD site documents use of the integrated TSS runner in FileCommander 4.4.3; availability and compatibility of other runtimes must be confirmed separately.

## License and limitations

Apache-2.0 for the enclosed documentation/examples with existing attribution in `NOTICE.md`. Third-party dependencies, Shield, commercial engines and unbundled software are not licensed or included here. See `SOURCE_PROVENANCE.json` and [SECURITY.md](SECURITY.md).
