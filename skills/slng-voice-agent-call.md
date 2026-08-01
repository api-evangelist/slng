---
name: Build and dispatch a SLNG voice agent
description: Create a voice agent, dispatch an outbound phone call, and poll the call result.
api: openapi/slng-agents-openapi.yml
operations: [createAgent, dispatchCall, getCall, listAgentCalls, createWebSession]
---

# Build and dispatch a SLNG voice agent

Create an LLM-driven voice agent that takes/makes phone calls or runs in the browser.
Base URL: `https://api.agents.slng.ai`.

## Auth
`Authorization: Bearer <SLNG_API_KEY>`.

## Steps
1. Create the agent: `POST /v1/agents` (`createAgent`) with `name`, `system_prompt`,
   `greeting`, `language`, `region`, and a `models` block (`stt`, `llm`, `tts`,
   `tts_voice`). Optional: `template_defaults`, `idle_nudges`, and `tools`.
2. Dispatch an outbound call: `POST /v1/agents/{agent_id}/calls` (`dispatchCall`),
   passing the destination and any template variables.
3. Poll the call: `GET /v1/agents/{agent_id}/calls/{call_id}` (`getCall`), or list
   with `GET /v1/agents/{agent_id}/calls` (`listAgentCalls`).
4. For an in-browser (non-telephony) session instead of a call, use
   `POST /v1/agents/{agent_id}/web-sessions` (`createWebSession`).

## Notes
Template variables use `{{name}}` placeholders resolved from `template_defaults`
and per-call inputs. Errors use `{ "error": ... }` with `401`/`403` for auth.
