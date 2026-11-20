# Use the Francis voice with Home Assistant

Requires a working `en_US-francis-medium.onnx` **and** `en_US-francis-medium.onnx.json` pair that you are authorized to use.

## Home Assistant OS / Piper app

1. In Home Assistant go to **Settings > Apps > Install app**, locate **Piper**, and install it.
2. Start Piper; keep the app backend set to **`piper`** (not `omnivoice`).
3. Use **Open Web UI** on the Piper app page. In the **Piper** section upload both the ONNX and JSON file.
4. Alternatively, on supported installations copy both files to **`/share/piper`**. The base filenames must match.
5. Go to **Settings > Devices & services** and reload the Wyoming integration that connects to Piper. Home Assistant caches the voice list.
6. Open **Settings > Voice assistants**, edit your Assist pipeline and select `en_US-francis-medium` for text-to-speech.
7. Trigger a test response and confirm you hear Francis's synthetic voice.

> If you are running Home Assistant Container instead of the Piper app, use a separately hosted Wyoming Piper server that can access the model pair and connect it through Wyoming. The `/share/piper` path and Web UI procedure are Piper-app-specific.

## Troubleshooting

- **Voice not visible:** Confirm both files were uploaded; reload the Wyoming integration, not just the dashboard.
- **Piper refuses to load voice:** Check JSON validity and confirm the `.onnx` file is present, nonempty, and has the same base name.
- **Voice missing after changing backend:** Return backend to `piper` and reload Wyoming.
- **Playback distorted:** Test locally with the Piper CLI, compare another checkpoint or inspect the dataset.

## Official reference

[Home Assistant Piper app documentation](https://github.com/home-assistant/apps/blob/master/piper/DOCS.md)
