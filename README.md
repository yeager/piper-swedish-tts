# 🇸🇪 Piper Swedish TTS — High-Quality Voice Training

Training a better Swedish text-to-speech voice for [Piper](https://github.com/rhasspy/piper), targeting children's accessibility apps.

## Why?

The existing Piper Swedish voice (`sv_SE-nst-medium`) is functional but sounds robotic. We're finetuning [KBLab's pretrained checkpoint](https://huggingface.co/KBLab/piper-tts-nst-swedish) to produce a warmer, more natural-sounding Swedish voice — specifically for use in [autismapps](https://github.com/yeager/autismapps), a PWA suite of 42 AAC/autism apps for children.

## Approach

1. **Base model**: KBLab/piper-tts-nst-swedish (epoch 4041, 1.75M training steps)
2. **Dataset**: [NST Swedish TTS](https://huggingface.co/datasets/jimregan/nst_swedish_tts) — 5300 recordings from a single Swedish speaker (CC0 Public Domain)
3. **Architecture**: VITS (via Piper's training pipeline)
4. **Training**: Continue training from KBLab's checkpoint with tuned hyperparameters
5. **Export**: ONNX model for Piper WASM (runs in-browser, fully offline)
6. **Platform**: Google Colab free tier (T4 GPU)

## What KBLab Did

[KBLab](https://kb-labb.github.io/posts/2023-05-24-swedish-text-to-speech/) (National Library of Sweden) trained the original Swedish Piper voice on the NST dataset and published both the Piper model and their training checkpoint. Their work made this possible — we're building on it.

### Known Issues with Current Voice
- Robotic/flat prosody
- espeak-ng phonemization errors for some Swedish words
- No tone accent distinction (accent 1 vs accent 2)
- Monotone — lacks the natural melody of Swedish speech

## What We're Doing

- **Extended training** (target: 2-3M total steps) for better convergence
- **Hyperparameter tuning** (learning rate, batch size)
- **Quality evaluation** focused on children's app use cases (simple sentences, clear enunciation)
- **ONNX export** optimized for browser deployment (Piper WASM)

## Quick Start

1. Open [`train_swedish_tts.ipynb`](train_swedish_tts.ipynb) in Google Colab
2. Runtime → Change runtime type → **T4 GPU**
3. Run all cells
4. Training takes ~8-12h per session on T4
5. Model saves to Google Drive (survives disconnects)
6. Export ONNX when satisfied with quality

## Using the Trained Model

The exported `.onnx` file is a drop-in replacement for Piper's `sv_SE-nst-medium.onnx`:

```bash
echo "Hej! Jag hjälper dig med dina övningar." | piper \
  --model sv_SE-nst-medium.onnx \
  --output_file output.wav
```

For browser use (autismapps), place the `.onnx` file in `static/voices/` and update the voice configuration.

## Credits

- **[KBLab](https://kb-labb.github.io/)** — pretrained checkpoint and original training
- **[NST / Norwegian Language Bank](https://www.nb.no/sprakbanken/)** — Swedish speech dataset (CC0)
- **[Piper](https://github.com/rhasspy/piper)** — TTS engine and training framework
- **[espeak-ng](https://github.com/espeak-ng/espeak-ng)** — phonemization

## License

Training code: MIT  
NST dataset: CC0 1.0 (Public Domain)  
Piper: MIT  
Trained model weights: CC0 1.0
