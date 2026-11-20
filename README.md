# Francis (Left 4 Dead) Piper TTS

An **English (US) text-to-speech voice based on Francis from Left 4 Dead**, designed for [Piper TTS](https://github.com/OHF-Voice/piper1-gpl) and local voice responses in [Home Assistant](https://www.home-assistant.io/).

## Overview

| Property | Value |
| --- | --- |
| Voice | Francis (Left 4 Dead) |
| Language | English (United States), `en_US` |
| Engine | Piper TTS / ONNX Runtime |
| Architecture | VITS, single speaker |
| Quality label | `medium` |
| Sample rate | 22,050 Hz |
| ONNX file size | ~60 MB |
| Training method | Fine-tuned from the Piper Lessac medium checkpoint |
| Hardware used | RTX 4070 Ti (12 GB), Windows 11 + Ubuntu WSL2 |

The prepared training dataset contains **1,284 audio recordings with matching transcripts**. Training was configured for up to 2,000 epochs using a pretrained Piper checkpoint. The selected release checkpoint and final training details can be documented in [MODEL_CARD.md](MODEL_CARD.md).

## Download

Download **both** model files from [Releases](../../releases):

```text
en_US-francis-medium.onnx
en_US-francis-medium.onnx.json
```

Keep the files together with exactly these names. The `.onnx` file contains the voice; the `.onnx.json` file contains its configuration.

## Quick test (Windows / Linux / WSL2)

Install Piper in a Python environment:

```bash
python -m pip install piper-tts
```

In the directory containing the two model files, generate a test WAV:

```bash
python -m piper \
  -m ./en_US-francis-medium.onnx \
  -c ./en_US-francis-medium.onnx.json \
  -f ./test.wav \
  -- "I hate stairs. And I hate it when someone leaves the kitchen lights on!"
```

Play `test.wav` using your preferred audio player. No NVIDIA GPU is required for ordinary CPU inference.

For detailed instructions see [CLI usage](docs/CLI.md) and [troubleshooting](docs/TROUBLESHOOTING.md).

## Use with Home Assistant

1. Install and start the **Piper** app in Home Assistant (or use an existing Piper service connected through Wyoming).
2. Upload `en_US-francis-medium.onnx` **and** `en_US-francis-medium.onnx.json` using Piper's web interface. On supported Home Assistant OS setups, copying them to `/share/piper` is also supported.
3. **Reload the Wyoming integration** for Piper so Home Assistant refreshes its voice list.
4. Under **Settings > Voice assistants**, select the voice for the relevant Assist pipeline.
5. Test a spoken Home Assistant response.

See [the complete Home Assistant guide](docs/HOME_ASSISTANT.md).

## How it was created

- Source used: Francis voice recordings from [Left 4 Dead 2 — The Sounds Resource](https://sounds.spriters-resource.com/pc_computer/left4dead2/asset/412954/).
- The original collection contained individually recorded Francis dialogue lines in WAV format.
- Duplicate recordings were removed, and audio was filtered and converted to 22,050 Hz mono WAV.
- Speech transcripts were automatically generated using [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper).
- The prepared dataset contained **1,284 WAV recordings and 1,284 matching transcript entries**.
- Training used the [OHF-Voice Piper](https://github.com/OHF-Voice/piper1-gpl) VITS fine-tuning workflow and the Lessac medium base checkpoint.
- Training was accelerated using an NVIDIA RTX 4070 Ti with 12 GB VRAM.
- The trained checkpoint can be exported to ONNX, with the matching JSON configuration generated during training.

More details: [Model card](MODEL_CARD.md) and [Training notes](docs/TRAINING.md).

## Limitations

This is a character-style experimental voice designed primarily for English speech synthesis.

It may pronounce unusual names, foreign-language words, numbers, or long sentences incorrectly.

The model's performance depends on the quality and diversity of the training recordings. Since Francis's original dialogue includes shouting, short phrases, and expressive speech, some generated sentences may sound more aggressive or less natural than standard TTS voices.

A successful ONNX export does not guarantee that every generated phrase will sound natural.

## Repository contents

- [MODEL_CARD.md](MODEL_CARD.md) - technical details, model architecture and known limitations
- [docs/HOME_ASSISTANT.md](docs/HOME_ASSISTANT.md) - Piper / Wyoming installation
- [docs/CLI.md](docs/CLI.md) - local generation and verification
- [docs/DATASET.md](docs/DATASET.md) - dataset preparation and transcription process
- [docs/TRAINING.md](docs/TRAINING.md) - training configuration and reproducibility notes
- [docs/EXPORT.md](docs/EXPORT.md) - exporting checkpoints to ONNX
- [docs/PUBLISHING.md](docs/PUBLISHING.md) - GitHub release workflow
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) - common problems and solutions
- [scripts/verify_voice.py](scripts/verify_voice.py) - validate model/config files and print checksums
- [scripts/prepare_release.py](scripts/prepare_release.py) - stage local model files for a Release

## Credits

Thanks to the [Piper TTS project](https://github.com/OHF-Voice/piper1-gpl), [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper), The Sounds Resource community, and [Home Assistant](https://www.home-assistant.io/) for the tools and resources used to create this voice model.
