# wedrive-voices

Offline TTS voice packs for WeDrive, a personal car-navigation app for one vehicle.
Each pack is a self-contained directory the app downloads once and runs fully offline
(onnxruntime on the head unit). `manifest.json` at the top of this repo is the index the
app reads: pack ids, release-asset URLs, sizes and sha256 checksums. Each archive carries
its own `manifest.json` with per-file sha256, plus `NOTICE` and `LICENSE`.

## Packs

| pack | language | voices | engine | license |
|---|---|---|---|---|
| `ru-silero-v5cis` | Russian | ru_ekaterina (F), ru_eduard (M) | Silero v5 base-cis, ONNX | MIT |
| `en-kokoro-82m` | English | af_heart (F), am_fenrir (M), am_michael (M alt) | Kokoro-82M v1.0, ONNX fp32 | Apache-2.0 |

## Licenses

- **Silero v5 base-cis** model weights: MIT (Silero Team). Stress/accentor data derived from
  silero-stress, MIT.
- **Kokoro-82M** model weights: Apache-2.0 ([hexgrad/Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M)).
  ONNX export from [thewh1teagle/kokoro-onnx](https://github.com/thewh1teagle/kokoro-onnx) (MIT project).
- **English lexicon** in `en-kokoro-82m`: extracted from [misaki](https://github.com/hexgrad/misaki)
  0.9.4 `us_gold` + `us_silver` dictionaries, Apache-2.0.
- **Not shipped anywhere in this repo:** espeak-ng (GPL-3), misaki's out-of-dictionary fallback.
  The English pack replaces it with the lexicon plus `overrides.json`; see the pack's `NOTICE`.

Every pack keeps the upstream license text in its `LICENSE` file and names its sources in `NOTICE`.

## Layout of a pack

```
<packId>/
  manifest.json     pack id, language, voices, per-file sha256
  *.onnx            model(s)
  frontend.json     text-frontend data (token vocabulary, normalization rules)
  overrides.json    per-word pronunciation overrides
  NOTICE, LICENSE
```

The English pack additionally ships `lexicon.tsv.gz` (G2P dictionary), `voices.npz`
(style vectors for the three shipped voices, trimmed losslessly from upstream) and
`G2P_MAPPING.md` (the exact model tensor contract with a worked, verified example).

Assets live in the rolling release tagged `packs`; checksums in `manifest.json` are
the contract — verify after download.
