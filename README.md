# Models of lib_SpeechToText_ui_ngx

Speech recognition models for [lib_SpeechToText_ui_ngx](https://github.com/convertigo/c8oprj-lib-speech-to-text):
OpenAI Whisper, ONNX exports by onnx-community, multilingual (99 languages), and a Vosk (Kaldi) model for French.
One Convertigo project per model, so that a
server deploys only the models it needs. The projects hold files only, no code: deploy their CARs on the Convertigo
server that serves your applications, next to `lib_SpeechToText_ui_ngx`. Browsers download the model from there once
and keep it in their cache; nothing is sent to an external service.

| Project | Model | Source and revision | Size | Used by `model: 'auto'` on |
| --- | --- | --- | --- | --- |
| `lib_SpeechToText_model_whisperBase` | Whisper base | [onnx-community/whisper-base](https://huggingface.co/onnx-community/whisper-base) `27a07b4b52b3` | 98 MB | phones, tablets and computers without WebGPU (or when Small is not deployed) |
| `lib_SpeechToText_model_whisperSmall` | Whisper small | [onnx-community/whisper-small](https://huggingface.co/onnx-community/whisper-small) `36050c46d777` | 302 MB | computers with WebGPU |
| `lib_SpeechToText_model_whisperTiny` | Whisper tiny | [onnx-community/whisper-tiny](https://huggingface.co/onnx-community/whisper-tiny) `ff4177021cc4` | 34 MB | never: apps that set `model: 'tiny'` (fastest Whisper, less accurate) |
| `lib_SpeechToText_model_voskSmallFr` | Vosk small French | [vosk-model-small-fr-0.22](https://alphacephei.com/vosk/models) (zip SHA-256 `cabf6180e177`) | 42 MB | never: apps that set `model: 'vosk'` (streaming, French) |

Whisper tiny, base and small: release v1.1.0 holds the four CARs (base and small unchanged since v1.0.0).

## Deployment

Take the CARs from the GitHub releases of this repository: the model files are not in git, so the projects cannot be
imported from a git URL.

## Build

The model files are listed in each project's `models.json` with their pinned revision and SHA-256. The `fetchModels`
Gradle task downloads them from Hugging Face into `DisplayObjects/models` and checks them; `car` runs it first. For
Vosk, `models.json` lists the zip of Alpha Cephei with its SHA-256: the task downloads it, checks it, and repacks it as
the `.tar.gz` that vosk-browser loads (`DisplayObjects/models/vosk/vosk-model-small-fr-0.22.tar.gz`).

```bash
./gradlew car                                       # every CAR, in <project>/build
./gradlew :lib_SpeechToText_model_whisperBase:car   # one model
./gradlew fetchModels                               # only download and check the files
```

## Files

`lib_SpeechToText_model_whisperBase/DisplayObjects/models/onnx-community/whisper-base/`:

- `config.json` (0.0 MB)
- `generation_config.json` (0.0 MB)
- `onnx/decoder_model_merged_quantized.onnx` (53.7 MB)
- `onnx/encoder_model_q4.onnx` (18.8 MB)
- `onnx/encoder_model_quantized.onnx` (23.2 MB)
- `preprocessor_config.json` (0.0 MB)
- `tokenizer.json` (2.5 MB)
- `tokenizer_config.json` (0.3 MB)

`lib_SpeechToText_model_whisperSmall/DisplayObjects/models/onnx-community/whisper-small/`:

- `config.json` (0.0 MB)
- `generation_config.json` (0.0 MB)
- `onnx/decoder_model_merged_q4.onnx` (233.1 MB)
- `onnx/encoder_model_q4.onnx` (66.2 MB)
- `preprocessor_config.json` (0.0 MB)
- `tokenizer.json` (2.5 MB)
- `tokenizer_config.json` (0.3 MB)

`lib_SpeechToText_model_whisperTiny/DisplayObjects/models/onnx-community/whisper-tiny/`:

- `config.json`, `generation_config.json`, `preprocessor_config.json`, `tokenizer.json` (2.5 MB), `tokenizer_config.json` (0.3 MB)
- `onnx/decoder_model_merged_quantized.onnx` (30.7 MB)
- `onnx/encoder_model_q4.onnx` (9.0 MB)
- `onnx/encoder_model_quantized.onnx` (10.1 MB)

`lib_SpeechToText_model_voskSmallFr/DisplayObjects/models/vosk/`:

- `vosk-model-small-fr-0.22.tar.gz` (42.2 MB)

## Licence

OpenAI releases the Whisper weights under the Apache-2.0 licence on Hugging Face (MIT for its original GitHub
release). Alpha Cephei releases vosk-model-small-fr-0.22 under the Apache-2.0 licence (the other small French model,
pguyot, is CC-BY-NC-SA and is not used). This repository is under the Apache-2.0 licence too (see `LICENSE`).
