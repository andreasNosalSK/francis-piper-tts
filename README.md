# Francis (Left 4 Dead) - Piper TTS voice (unofficial)

**An unofficial, experimental English-language Francis-inspired voice project** for [Piper TTS](https://github.com/OHF-Voice/piper1-gpl), [Home Assistant](https://www.home-assistant.io/), and other local text-to-speech applications.

## Project status

| Item | Status |
| --- | --- |
| Training audio and transcriptions | **1,284 WAV files and 1,284 metadata rows** generated locally (verified by the project owner) |
| Audio/transcript quality audit | **Owner to confirm**; automatic speech recognition may contain mistakes |
| Model fine-tuning | **Owner to confirm exact run/epoch/checkpoint** |
| ONNX export and inference test | **Owner to confirm** |
| Public model download | **Not included in this documentation template**; requires a rights review |

This repository package contains documentation and lightweight verification utilities. It **does not** contain trained weights, the game's audio, or a demo recording. Update the status above before publishing a model release.

## At a glance

| Property | Target / known configuration |
| --- | --- |
| Model name | `en_US-francis-medium` |
| Voice inspiration | Francis from *Left 4 Dead* / *Left 4 Dead 2* |
| Language | English (US), `en_US` |
| Framework | [Piper TTS](https://github.com/OHF-Voice/piper1-gpl) |
| Architecture | VITS, single speaker |
| Inference format | ONNX and Piper JSON config |
| Sample rate | 22,050 Hz (`medium`) |
| Training method | Fine-tuning from Piper Lessac medium checkpoint |
| Training hardware | NVIDIA RTX 4070 Ti (12 GB), Windows 11, Ubuntu on WSL2, 128 GB system RAM |
| Dataset preparation | Pre-split game speech; Faster-Whisper `small.en` used for draft transcripts |
| Training corpus size | 1,284 aligned filename/transcript records; not independently audited |

## What you need to use the voice

After a valid release is available, download **both** files and keep their names exactly matched:

```text
en_US-francis-medium.onnx
en_US-francis-medium.onnx.json
```

The ONNX file is the synthesizer; the JSON file contains its language, phonemes, speaker and audio settings. A JSON file alone cannot synthesize speech.

## Quick local test

Install Piper in a Python virtual environment:

```bash
python -m pip install piper-tts
```

From the folder containing both voice files:

```bash
python -m piper \
  -m ./en_US-francis-medium.onnx \
  -c ./en_US-francis-medium.onnx.json \
  -f ./francis-test.wav \
  -- "Somebody turn the lights on. I can't see a thing."
```

Play `francis-test.wav` to assess clarity, pronunciation and similarity. See [CLI guide](docs/CLI.md) and [Troubleshooting](docs/TROUBLESHOOTING.md).

## Home Assistant integration

Home Assistant's Piper app supports custom voices through the web UI or `/share/piper`:

1. Install and start the **Piper** app in Home Assistant; select the `piper` backend.
2. Upload `en_US-francis-medium.onnx` **and** `en_US-francis-medium.onnx.json` through **Open Web UI**, or copy both to `/share/piper`.
3. Under **Settings > Devices & services**, reload the Wyoming integration used by Piper; Home Assistant caches the voice list.
4. Under **Settings > Voice assistants**, choose the new voice for the Assist pipeline.
5. Test a response on your speaker.

See [Home Assistant installation](docs/HOME_ASSISTANT.md). The user interface can differ if you run Piper in a container instead of Home Assistant OS.

## How this project was prepared

The creator worked with pre-split Francis voice-line files; in the initial scan there were 2,730 paths, including 1,365 byte-identical duplicates. A compatibility issue in the audio decoder was repaired and the final processed dataset contained **1,284 WAV files and 1,284 `metadata.csv` lines**. Faster-Whisper generated draft transcripts; manual transcript correction has **not** been verified.

## Documentation

- [Model card](MODEL_CARD.md): architecture, dataset, evaluation and known limitations
- [Rights & licensing](RIGHTS_AND_LICENSING.md): what must be verified before public redistribution
- [Home Assistant setup](docs/HOME_ASSISTANT.md): installation and Wyoming troubleshooting
- [Local CLI](docs/CLI.md): generate a WAV on Windows / Linux / WSL2
- [Dataset notes](docs/DATASET.md): transcripts, cleaning, and corpus provenance
- [Training workflow](docs/TRAINING.md): fresh fine-tune and checkpoint resume
- [ONNX export](docs/EXPORT.md): select a checkpoint, export, check, test
- [Publishing guide](docs/PUBLISHING.md): release assets and checksums
- [Troubleshooting](docs/TROUBLESHOOTING.md)
- [Contributing](CONTRIBUTING.md) and [Changelog](CHANGELOG.md)

## Validate a local model pair

When the two files exist on your machine, run:

```bash
python scripts/verify_voice.py \
  /path/to/en_US-francis-medium.onnx \
  /path/to/en_US-francis-medium.onnx.json
```

The utility checks both files, parses the JSON and prints SHA-256 hashes without needing Piper. It does not assess synthesized voice quality.

## Attribution and usage

Piper is developed by the [Open Home Foundation](https://github.com/OHF-Voice/piper1-gpl). The character Francis and underlying media are associated with Valve's *Left 4 Dead* franchise. Credits and links do **not** grant rights to distribute original clips or derivative model weights.

This is a research and fan documentation template. Avoid portraying synthetic audio as an authentic performance by the original voice actor.
