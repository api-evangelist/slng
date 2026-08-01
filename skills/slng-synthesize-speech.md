---
name: Synthesize speech with SLNG TTS
description: Turn text into audio with a SLNG-hosted text-to-speech model, with optional pronunciation dictionaries.
api: openapi/slng-tts-slng-openapi.yml
operations: ["rime/arcana:3-en", "slng/deepgram/aura:2-en", pronunciation-dictionaries-create]
---

# Synthesize speech with SLNG TTS

Generate audio from text over the SLNG gateway (HTTP or streaming WebSocket).

## Auth
`Authorization: Bearer <SLNG_API_KEY>`.

## Steps
1. Choose a voice model, e.g. `POST /v1/tts/slng/rime/arcana:3-en`
   (`rime/arcana:3-en`) for expressive low-latency English, or
   `POST /v1/tts/slng/deepgram/aura:2-en` (`slng/deepgram/aura:2-en`).
2. POST the text body (with a `speaker`/voice where the model requires one).
   Discover available voices and models via `GET /v1/catalog/models`
   (`catalog-models-list`).
3. (Optional) For brand names and domain terms, create a reusable dictionary with
   `POST /v1/tts/pronunciation-dictionaries` (`pronunciation-dictionaries-create`)
   and reference it on synthesis.
4. Receive the audio stream/file in the response.

## Errors
`400` validation (e.g. invalid speaker id / encoding), `401` bad key, `500`/`503`
provider errors. Provider errors carry `{ error, status, details }`.
