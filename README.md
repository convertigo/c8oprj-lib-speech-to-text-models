# Models of lib_SpeechToText_ui_ngx

Speech recognition models for [lib_SpeechToText_ui_ngx](https://github.com/convertigo/c8oprj-lib-speech-to-text):
OpenAI Whisper, ONNX exports by onnx-community, multilingual (99 languages). One Convertigo project per model, so that a
server deploys only the models it needs. The projects hold files only, no code: deploy their CARs on the Convertigo
server that serves your applications, next to `lib_SpeechToText_ui_ngx`. Browsers download the model from there once
and keep it in their cache; nothing is sent to an external service.

| Project | Model | Source and revision | Size | Used by `model: 'auto'` on |
| --- | --- | --- | --- | --- |
| `lib_SpeechToText_model_whisperBase` | Whisper base | [onnx-community/whisper-base](https://huggingface.co/onnx-community/whisper-base) `27a07b4b52b3` | 98 MB | phones, tablets and computers without WebGPU (or when Small is not deployed) |
| `lib_SpeechToText_model_whisperSmall` | Whisper small | [onnx-community/whisper-small](https://huggingface.co/onnx-community/whisper-small) `36050c46d777` | 302 MB | computers with WebGPU |

## Deployment

Take the CARs from the GitHub releases of this repository: the model files are not in git, so the projects cannot be
imported from a git URL.

## Build

The model files are listed in each project's `models.json` with their pinned revision and SHA-256. The `fetchModels`
Gradle task downloads them from Hugging Face into `DisplayObjects/models` and checks them; `car` runs it first.

```bash
./gradlew car                                       # both CARs, in <project>/build
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

## Licence

OpenAI releases the Whisper weights under the Apache-2.0 licence on Hugging Face (MIT for its original GitHub
release). This repository is under the Apache-2.0 licence too (see `LICENSE`).
