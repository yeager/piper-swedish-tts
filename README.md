# 🇸🇪 Piper Swedish TTS — Enhanced Voice Training

Train a high-quality Swedish text-to-speech voice for [Piper](https://github.com/rhasspy/piper), optimized for children's educational apps.

## Background

The existing Swedish Piper voice (`sv_SE-nst-medium`) was trained by [KBLab](https://kb-labb.github.io/posts/2023-05-24-swedish-text-to-speech/) (National Library of Sweden) on the [NST Swedish TTS dataset](https://huggingface.co/datasets/KTH/nst). It's decent but has issues:

- Somewhat robotic prosody
- Odd pronunciation of certain words (espeak-ng phoneme conversion errors)
- Not optimized for slow, clear speech (needed for children with autism/language disorders)

## What We're Doing

**Continuing training** from KBLab's checkpoint (epoch 4041, 1.75M steps) with:

1. **Same NST dataset** (5,300 recordings, single male speaker, CC0)
2. **More training steps** — targeting 500k-1M additional steps
3. **Tuned hyperparameters** — lower learning rate for refinement
4. **Quality focus** — evaluating on child-friendly test sentences

The goal is a voice suitable for [autismapps](https://github.com/yeager/autismapps) — a PWA with 42 educational apps for children with autism, ADHD, and verbal dyspraxia.

## Quick Start

1. Open the Colab notebook: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yeager/piper-swedish-tts/blob/main/train_swedish_tts.ipynb)
2. Enable GPU runtime (Runtime → Change runtime type → T4)
3. Run all cells
4. Training auto-saves checkpoints to Google Drive
5. Export to ONNX when satisfied

## Architecture

```
NST Dataset (5300 wav + text, CC0)
        ↓
espeak-ng phonemizer (sv)
        ↓
Piper/VITS training (PyTorch Lightning)
        ↓ resume from
KBLab checkpoint (epoch 4041)
        ↓ +500k-1M steps
Improved checkpoint
        ↓
ONNX export (Piper format)
        ↓
autismapps (Piper WASM in browser)
```

## Files

| File | Description |
|------|-------------|
| `train_swedish_tts.ipynb` | Google Colab notebook — the main training pipeline |
| `README.md` | This file |

## Data Sources

| Source | License | Description |
|--------|---------|-------------|
| [NST Swedish TTS](https://huggingface.co/datasets/KTH/nst) | CC0 1.0 | 5,300 recordings, single speaker |
| [KBLab checkpoint](https://huggingface.co/KBLab/piper-tts-nst-swedish) | — | Pre-trained VITS weights (epoch 4041) |
| [Piper](https://github.com/rhasspy/piper) | MIT | TTS engine and training framework |

## Expected Results

| Metric | Current (KBLab) | Target |
|--------|-----------------|--------|
| Naturalness | 5/10 | 7-8/10 |
| Swedish prosody | 4/10 | 7/10 |
| Clear pronunciation | 6/10 | 8/10 |
| Slow speech quality | 3/10 | 8/10 |

Limitations:
- Swedish tone accents (accent 1 vs 2) remain challenging for all TTS
- Single male speaker — no female voice from this dataset
- espeak-ng phoneme errors affect some words

## Training Tips

- **Colab T4**: ~1000 steps/hour, batch size 16
- **Colab A100** (Pro): ~3000 steps/hour, batch size 32
- If Colab disconnects, re-run step 6 — it resumes from Drive
- Lower learning rate (5e-5) for fine-tuning, higher (1e-4) for continued training
- Monitor loss — if it plateaus, try reducing LR

## Credits

- **[KBLab](https://kb-labb.github.io/)** — pretrained Swedish checkpoint
- **[NST / Språkbanken](https://www.nb.no/sprakbanken/)** — speech dataset
- **[Piper / rhasspy](https://github.com/rhasspy/piper)** — TTS engine
- **[autismapps](https://github.com/yeager/autismapps)** — the app this voice is for

## License

Training code: MIT  
NST dataset: CC0 1.0  
Exported model weights: Same as upstream (check KBLab/Piper licensing)
