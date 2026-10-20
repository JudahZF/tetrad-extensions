# Tetrad extensions

The default extension repository of [Tetrad](https://github.com/JudahZF/tetrad), the open,
community-built touch UI for the Neural DSP Quad Cortex. Tetrad units know this repository and
its signing key: open **Settings → System → Extensions** on the unit and tap **REFRESH**.

| Extension | Version | What it does |
| --- | --- | --- |
| MIDI Mapper | 0.2.0 | Reserves a second MIDI channel and offers its CCs, notes and program changes as triggers on the Mappings page. |

## What is in this repository

| File | What it is |
| --- | --- |
| `tetrad-extensions.json` | The catalog: each package with its release tag, asset name, size and SHA-256. |
| `tetrad-extensions.sig` | The Ed25519 signature of the exact catalog bytes. |
| `tetrad-extensions.pub` | The public key. Fingerprint: `c702 baa2 2830 5c13 b3e6 e9a1 bda9 277a`. |

The packages (`.tetradext`) are assets of the [releases](https://github.com/JudahZF/tetrad-extensions/releases).
A unit refuses a package whose size or SHA-256 differs from the signed catalog, and a catalog
that this key did not sign.

## Source

The source of every extension here is in the Tetrad repository, under
[`extensions/`](https://github.com/JudahZF/tetrad/tree/main/extensions). To write your own
extension or publish your own repository, see
[docs/extensions.md](https://github.com/JudahZF/tetrad/blob/main/docs/extensions.md).

## Licence

GPL-3.0-or-later, as Tetrad. See [LICENSE](LICENSE).

Tetrad is an independent, unofficial project. It is not affiliated with, endorsed by or
supported by Neural DSP Technologies. "Neural DSP" and "Quad Cortex" are trademarks of their
owners.
