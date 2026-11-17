# Interfaze plugin for AI agents

This plugin connects an AI agent to the hosted [Interfaze](https://interfaze.ai) MCP server. It also adds skills that tell the agent which Interfaze tool to use. It is an [Agent Plugins](https://agent-plugins.org) 1.0.0 package. The package has a manifest, an MCP connection and Markdown skills. It has no local code and no runtime dependencies.

It works with [Hermes Agent](https://hermes-agent.nousresearch.com) and [OpenClaw](https://openclaw.ai).

## What's included

- `mcp.json`: the hosted Interfaze MCP server at `https://api.interfaze.ai/mcp`, as the `interfaze` server.
- `skills/get-started`: checks the connection and gets the user to a first result.
- `skills/interfaze`: picks the correct tool for each request and explains how to send a local file.
- Workflow skills: `ocr`, `speech-to-text`, `web-search`, `web-scraping` and `object-detection`.

## Tools

| Tool | What it does |
|---|---|
| `ask_interfaze` | Does a task with more than one step. It uses the other tools as necessary. |
| `web_search` | Searches the web for current information. |
| `scraper` | Gets structured data from a web page. |
| `ocr` | Reads text from images, PDFs and Word documents. |
| `speech_to_text` | Transcribes audio and video. |
| `object_detection` | Finds objects in an image or PDF, with bounding boxes. |
| `gui_detection` | Finds buttons, inputs and links in a screenshot. |
| `translate` | Translates text. |
| `classification` | Sorts text or images into labels that you give. |
| `forecast` | Predicts future values from a dated series. |
| `get_upload_url` | Gives a URL to upload a local file to. |

## Get an API key

Get a key from the dashboard at [interfaze.ai](https://interfaze.ai). Keep the key out of chats, URLs and this repository.

## Hermes Agent

Requires Hermes Agent 0.22.2 or later.

1. Install the plugin:

   ```bash
   hermes plugins install interfaze
   ```

   To install from this repository instead of the catalog:

   ```bash
   hermes plugins install InterfazeAI/interfaze-agent-plugin --no-enable
   ```

2. Hermes installs portable packages disabled. Enable the plugin in the profile that you chat from:

   ```bash
   hermes plugins enable interfaze
   ```

3. Save the key to the same profile:

   ```bash
   hermes config set INTERFAZE_API_KEY <your-key>
   ```

4. Start a new session.

Hermes names the tools `mcp__interfaze__<tool>`, for example `mcp__interfaze__ocr`. The skills load as `interfaze:<skill>`.

### Approvals

The plugin marks the `interfaze` server as `trust: untrusted`. Hermes runs the read-only tools without a prompt. `get_upload_url` is not read-only, so Hermes asks for approval before each call. Approve it once, for the session, or always.

## OpenClaw

1. Install the plugin from GitHub:

   ```bash
   openclaw plugins install git:github.com/InterfazeAI/interfaze-agent-plugin@main
   ```

   A ClawHub listing is not available yet. The ClawHub CLI (v0.26.0) does not publish packages in the Agent Plugins format.

2. Add `INTERFAZE_API_KEY` to the environment of the OpenClaw Gateway service. OpenClaw reads `${INTERFAZE_API_KEY}` in `mcp.json` from that environment. A variable that you set only in an interactive shell does not reach a Gateway that runs as a service.
3. Restart the Gateway and start a new session.
4. Check the install:

   ```bash
   openclaw plugins inspect interfaze
   ```

OpenClaw names the tools `interfaze__<tool>`, for example `interfaze__ocr`.

## Local files

The tools take files as `https` URLs. For a local file, the agent does these steps:

1. It calls `get_upload_url` with the file name.
2. It uploads the file with an HTTP PUT, for example `curl -T ./receipt.jpg '<upload_url>'`.
3. It puts the returned `file_url` in the `files` argument of the task tool.

The agent needs a shell tool for step 2. Without a shell tool, give the agent a public URL of the file.

## Example prompts

- "Search the web for this week's announcements about MCP and summarize them with links."
- "Get the name and monthly price of each plan from https://example.com/pricing."
- "Extract the merchant, date, line items and total from ./receipt.jpg."
- "Transcribe ./standup.mp3 and list the action items."
- "Find the login button in ./screenshot.png and give its bounding box."

## Disclosure

- **Network:** Every tool call goes to the Interfaze MCP server at `api.interfaze.ai`. Each call uses the user's `INTERFAZE_API_KEY`.
- **Data sent:** The plugin sends prompts, search queries, URLs, text, and the images, documents and audio that the agent attaches. Local files go to Interfaze storage through `get_upload_url`.
- **Data kept:** Interfaze stores the request payload of each call and logs call metadata, unless zero data retention is on for the request. Results also go into the agent's context, so they reach the user's model provider.
- **Billing:** Each call uses credits on the user's Interfaze account.
- **Tool list:** The server supplies the tool list at runtime. A pinned commit of this repository does not include the tool list.
- **Local behavior:** The package has no local code, no telemetry, no background process and no self-update. It stores no credentials. The host reads the key from its own configuration.

## CI

The `Validate` workflow runs `hermes plugins validate . --install-deps` on Hermes v0.22.2 and v0.23.2. It also checks `plugin.json` and `mcp.json` against the Agent Plugins 1.0.0 schemas.

## License

MIT
