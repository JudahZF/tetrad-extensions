# Tetrad extensions

The default extension repository of [Tetrad](https://github.com/JudahZF/tetrad), the open,
community-built touch UI for the Neural DSP Quad Cortex. Tetrad units know this repository and
its signing key: open **Settings → Tetrad → Extensions** on the unit and tap **REFRESH**.

| Extension | Version | API | What it does |
| --- | --- | --- | --- |
| MIDI Mapper | 0.3.0 | 3 | Reserves a second MIDI channel and offers its CCs, notes and program changes as triggers on the Mappings page. A note-off releases its note, for Momentary mappings. |
| Neural Amp Modeler A1 | 0.5.0 | 5 | Plays NAM LSTM and WaveNet models (.nam files) as Guitar Amplifier blocks, up to the trainer's lite size, with the NAM plugin's tone controls. |
| Neural Amp Modeler A2 | 0.2.0 | 5 | Plays NAM A2 models (A2-Lite) as Guitar Amplifier blocks, with the NAM plugin's tone controls. |
| DSP Examples | 0.3.0 | 2 | Seven small DSP blocks from the qcdsp examples: gain, two drives, tremolo, slapback, echo and envelope filter. |
| Sweep Synth | 0.1.0 | 2 | An analog-style mono synth, as a guitar block and a bass block, with a filter that sweeps on each note. |
| Custom Footswitch Mode | 0.1.0 | 4 | Adds a Custom footswitch mode: four pages of ten switches, each with an action that you choose on the unit. No code runs. |
| DOOM | 0.1.0 | 7 | The shareware episode of DOOM on the screen, played with the footswitches: the original [qcDOOM](https://github.com/VanIseghemThomas/qcDOOM) binary by Thomas Van Iseghem. A program with full access to the unit. |

A unit offers an extension only when its Tetrad build runs that extension API; an older build
keeps the newest version that it can run.

## What is in this repository

| File | What it is |
| --- | --- |
| `tetrad-extensions.json` | The catalog: each package with its release tag, asset name, size and SHA-256. |
| `tetrad-extensions.sig` | The Ed25519 signature of the exact catalog bytes. |
| `tetrad-extensions.pub` | The public key. Fingerprint: `c702 baa2 2830 5c13 b3e6 e9a1 bda9 277a`. |

The packages (`.tetradext`) are assets of the [releases](https://github.com/JudahZF/tetrad-extensions/releases).
A package with a program (DOOM) has its program files as assets of the same release, such as
`tetrad.doom-0.1.0.qcdoom`; the package pins them by SHA-256.
A unit refuses a package whose size or SHA-256 differs from the signed catalog, and a catalog
that this key did not sign.

## Source

The source of every extension here is in the Tetrad repository, under
[`extensions/`](https://github.com/JudahZF/tetrad/tree/main/extensions). To write your own
extension or publish your own repository, see
[docs/extensions.md](https://github.com/JudahZF/tetrad/blob/main/docs/extensions.md).

## Publish a release

This repository is the only copy of the catalog and the packages. You need a Tetrad checkout
(`T=<tetrad>/target/debug/tetrad-ext`, from `cargo build -p tetrad-extension-cli`) and the
signing key (`$KEY`, mode 0600, never in a repository).

1. In a clone of this repository, download every published package into `packages/` (Git
   ignores it). `tetrad-ext catalog` signs a catalog of every package in that folder.

   ```sh
   <tetrad>/scripts/fetch-extensions.sh .
   ```

2. Build and package the extension from the Tetrad checkout. Increase `version` in its
   `tetrad-extension.json` first: a published `<id>-<version>` never changes. A program also
   writes its files next to the package (DOOM: run `extensions/doom/fetch.sh` first).

   ```sh
   $T build <tetrad>/extensions/midi-mapper
   $T package <tetrad>/extensions/midi-mapper --out packages/tetrad.midi-mapper-0.4.0.tetradext
   ```

3. Sign the catalog with a new tag. Published packages keep their tag.

   ```sh
   $T catalog --repo-dir . --key "$KEY" --tag extensions-2026-11-01
   ```

4. Create the release and upload the new package and its program files, if any:

   ```sh
   gh release create extensions-2026-11-01 --repo JudahZF/tetrad-extensions \
     packages/tetrad.midi-mapper-0.4.0.tetradext
   ```

5. Then commit `tetrad-extensions.json` and `tetrad-extensions.sig` and push. Upload first, so
   that no unit sees a catalog entry whose asset does not exist yet.

Check a package with `$T check FILE.tetradext`, and a whole repository through the unit's
install path with `$T stage --repo-dir . --store /tmp/store`.

## Licence

GPL-3.0-or-later, as Tetrad. See [LICENSE](LICENSE).

Tetrad is an independent, unofficial project. It is not affiliated with, endorsed by or
supported by Neural DSP Technologies. "Neural DSP" and "Quad Cortex" are trademarks of their
owners.
