---
name: get-started
description: Use when the user installs the Interfaze plugin, asks how to set it up, or the Interfaze tools fail with an authorization error. Checks the connection and gets the user to a first result.
---

# Get started with Interfaze

## 1. Check the tools

Look for the Interfaze tools in your tool list. Hermes shows them as `mcp__interfaze__<tool>`. OpenClaw shows them as `interfaze__<tool>`.

If the tools are not in the list, do these checks:

- Hermes installs portable packages disabled. Tell the user to run `hermes plugins enable interfaze` and start a new session.
- The tools load at session start. A new session is necessary after the user sets the key.

## 2. Check the key

The server needs an Interfaze API key in the `INTERFAZE_API_KEY` environment variable. Do not ask the user to paste the key in the chat. Do not print the key.

If a tool returns "Unauthorized" or a 401 error, the key is missing or wrong. Tell the user to do these steps:

1. Get a key from the dashboard at https://interfaze.ai.
2. Set the key:
   - Hermes: `hermes config set INTERFAZE_API_KEY <key>`
   - OpenClaw: add `INTERFAZE_API_KEY` to the environment of the OpenClaw Gateway service, then restart the Gateway.
3. Start a new session.

## 3. Get a first result

Make one small call that shows the plugin works. Use one of these examples:

- `web_search` with the prompt "What is the latest stable version of Python?"
- `ocr` with the prompt "Extract all text" and `files` set to `["https://r2public.jigsawstack.com/interfaze/examples/id.jpg"]`

Show the user the result. Then tell the user which other tools are available. The `interfaze` skill has the full list.
