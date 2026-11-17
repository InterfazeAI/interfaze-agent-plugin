---
name: interfaze
description: Use when a task needs an Interfaze tool. The tools do web search, web scraping, OCR, speech-to-text, object and GUI detection, translation, classification and forecasting. Tells you which tool to call and how to send a local file.
---

# Interfaze tools

The `interfaze` MCP server gives you 11 tools. Hermes shows them as `mcp__interfaze__<tool>`. OpenClaw shows them as `interfaze__<tool>`. This skill uses the bare tool name.

## Pick the tool

Use the task-specific tool when the request maps to one task. Use `ask_interfaze` only for work with more than one step.

| Request | Tool | Files |
|---|---|---|
| Find current information on the web | `web_search` | No |
| Get data from a known web page | `scraper` | No. Put the URL in `prompt`. |
| Read text from an image, PDF or Word document | `ocr` | Yes |
| Transcribe audio or video | `speech_to_text` | Yes |
| Find objects in an image or PDF, with bounding boxes | `object_detection` | Yes |
| Find buttons, inputs and links in a screenshot | `gui_detection` | Yes |
| Translate text | `translate` | No |
| Sort text or images into labels that you give | `classification` | Yes |
| Predict future values from a dated series | `forecast` | No |
| A task with more than one step, or a task that does not fit one tool | `ask_interfaze` | Yes |

Every task tool takes a `prompt`. Write the prompt as a full instruction. Include the URL, the fields that you want and the output language.

## Send a file

The `files` argument takes up to 10 `https` URLs. Do not put base64 data or file text in a tool argument.

If the file already has a public URL, put the URL in `files`.

If the file is local, do these steps:

1. Call `get_upload_url` with the file name. Keep the extension, for example `receipt.jpg`.
2. Upload the raw bytes with an HTTP PUT. For example: `curl -T ./receipt.jpg '<upload_url>'`.
3. Call the task tool. Put the returned `file_url` in `files`.

`get_upload_url` is not a read-only tool. Hermes asks the user for approval before each call. The upload URL expires after 15 minutes. The file URL expires after 1 hour. Upload the file and call the task tool at once.

If you cannot run a shell command, you cannot do step 2. Ask the user for a public URL of the file.

## Read the result

Each task tool returns a JSON object with `name` and `result`. When the task made more than one call, `results` lists each call.

If `result` contains `error`, the task failed. Tell the user the error text. Do not present the error as data.

## Costs

Each call uses credits on the user's Interfaze account. Do not call a tool again with the same input when you already have the result.
