---
name: Transcribe audio with SLNG STT
description: Send an audio file or URL to a SLNG-hosted speech-to-text model and read back the transcript.
api: openapi/slng-stt-slng-openapi.yml
operations: [sttWhisperLargeV3Receive, "slng/deepgram/nova:3-en"]
---

# Transcribe audio with SLNG STT

Convert speech to text over the SLNG gateway (HTTP). Streaming is available over
WebSocket (see `asyncapi/slng-stt-slng-asyncapi.yml`).

## Auth
Send `Authorization: Bearer <SLNG_API_KEY>` (key from https://app.slng.ai/api-keys).

## Steps
1. Pick a model. For 99+ language coverage use `POST /v1/stt/slng/openai/whisper:large-v3`
   (`sttWhisperLargeV3Receive`); for low-latency English use
   `POST /v1/stt/slng/deepgram/nova:3-en`.
2. Provide audio one of two ways:
   - `multipart/form-data` with an `audio` file (Whisper max 25MB), or
   - `application/json` with a public `url` (e.g. `https://docs.slng.ai/audio/hello.wav`).
   Optionally set `language` (ISO-639-1) — omit to auto-detect.
3. (Optional) Route with `X-Region-Override` / `X-World-Part-Override` for latency or data residency.
4. Read the transcript: Whisper returns `{ text, language }`; Deepgram returns
   `results.channels[].alternatives[].transcript` plus `metadata.request_id`.

## Errors
`400` missing/invalid audio, `401` bad key, `413` file too large, `500`/`503`
provider errors. Envelope is `{ "error": ... }` (see `errors/slng-problem-types.yml`).
