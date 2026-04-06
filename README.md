# 🇸🇪 Piper Swedish TTS — High-Quality Voice Training

Swedish Piper voice work for natural, offline text-to-speech.

## Voices

### Axel v5
- **Voice**: male Swedish voice
- **Status**: exported from the final v5 checkpoint
- **Checkpoint**: `epoch=4999-step=7998000.ckpt`
- **Files**: `models/axel-v5/`
- **Includes**:
  - `axel-v5.onnx`
  - `axel-v5.onnx.json`
  - `axel-v5-test-2026-04-06.wav`
  - `test-text-2026-04-06.txt`

### Alma
- **Voice**: female Swedish voice
- **Important**: Alma and Axel are separate voices and must not be mixed.

## Why?

The goal is better Swedish TTS for accessibility tools and open software, with warmer prosody and more natural pacing than the stock NST voice.

## Usage

```bash
echo "Hej! Det här är Axel version fem." | piper \
  --model models/axel-v5/axel-v5.onnx \
  --output_file axel-test.wav
```

## Training background

- Base work builds on KBLab's Swedish Piper checkpoint and the NST Swedish dataset.
- Exported ONNX is intended for practical use in Piper-compatible apps and services.

## Credits

- KBLab
- NST / Norwegian Language Bank
- Piper
- espeak-ng

## License

Training code: MIT  
Model artifacts in this repo: see upstream dataset/model terms.
