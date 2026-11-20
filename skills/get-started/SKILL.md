---
name: get-started
description: Use when the user installs the Interfaze plugin or asks how to set it up. Also use it when the Interfaze tools are missing, or fail with an authorization or limit error. Checks the connection and gets the user to a first result.
---

# Get started with Interfaze

The plugin works in two modes:

- **Keyless:** no account and no credits. Interfaze limits keyless calls for each IP address.
- **With a key:** the server reads an Interfaze API key from the `INTERFAZE_API_KEY` environment variable. Each call uses credits on the user's Interfaze account.

Do not ask the user to paste a key in the chat. Do not print the key.

## 1. Check the tools

Look for the Interfaze tools in your tool list. Hermes shows them as `mcp__interfaze__<tool>`. OpenClaw shows them as `interfaze__<tool>`.

If the tools are not in the list, do these checks:

- Hermes installs portable packages disabled. Tell the user to run `hermes plugins enable interfaze` and start a new session.
- Hermes does not load the tools when the key is wrong. It also does not load them when the key is not set and the user has not set up keyless access. Give the user the two options in step 2.
- The tools load at session start. A new session is necessary after each setup change.

## 2. Set up keyless access or a key

Tell the user to do one of these setups. Then the user restarts the Hermes gateway or the OpenClaw Gateway, and starts a new session. In Hermes, the user runs `hermes gateway restart` in a separate terminal. You cannot restart the gateway from inside the session.

Keyless:

- Hermes: `hermes config set mcp_servers.interfaze.url https://api.interfaze.ai/mcp`, then `hermes config set mcp_servers.interfaze.trust untrusted`.
- OpenClaw: `openclaw mcp set interfaze '{"url":"https://api.interfaze.ai/mcp","transport":"streamable-http"}'`, then restart the Gateway.

With a key:

1. Get a free key from the dashboard at https://interfaze.ai.
2. Set the key:
   - Hermes: `hermes config set INTERFAZE_API_KEY <key>`. If the user set up keyless access before, also run `hermes mcp remove interfaze`.
   - OpenClaw: add `INTERFAZE_API_KEY=<key>` to `~/.openclaw/.env`. Then run `openclaw mcp set interfaze '{"url":"https://api.interfaze.ai/mcp","transport":"streamable-http","headers":{"Authorization":"Bearer ${INTERFAZE_API_KEY}"}}'` and restart the Gateway.

## 3. Read connection errors

- "Unauthorized" or a 401 error: the key is wrong, or the host sent the key placeholder as text. Check the setup in step 2.
- "Too many keyless requests in progress": too many keyless calls run at the same time. Wait a few seconds, then try again.
- "Keyless daily limit ... reached": the IP address used all keyless calls for today. Do not try again today. Tell the user to set up a key.

## 4. Get a first result

Make one small call that shows the plugin works. Use one of these examples:

- `web_search` with the prompt "What is the latest stable version of Python?"
- `ocr` with the prompt "Extract all text" and `files` set to `["https://r2public.jigsawstack.com/interfaze/examples/id.jpg"]`

Show the user the result. Then tell the user which other tools are available. The `interfaze` skill has the full list.
