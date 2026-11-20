# Use the Francis voice with Piper's command-line interface

## Install Piper

On Windows, Linux, or inside WSL2, use a Python environment:

```bash
python -m pip install piper-tts
```

Download or obtain your authorized voice model pair and place both files in one directory:

```text
en_US-francis-medium.onnx
en_US-francis-medium.onnx.json
```

## Make a test WAV

```bash
python -m piper \
  -m ./en_US-francis-medium.onnx \
  -c ./en_US-francis-medium.onnx.json \
  -f ./francis-test.wav \
  -- "Attention, the back door is open."
```

Open `francis-test.wav` in your preferred audio player. Piper normally supports CPU synthesis without NVIDIA CUDA.

## Open the output directory from WSL2

```bash
explorer.exe "$(wslpath -w "$HOME/francis-tts/output")"
```

## Advanced

- `python -m piper --help` lists the CLI options for your installed version.
- A high-level Piper CLI invocation starts a new model load each time; for a persistent Home Assistant assistant, use the Piper/Wyoming service.
- See [official Piper CLI docs](https://github.com/OHF-Voice/piper1-gpl/blob/main/docs/CLI.md).
