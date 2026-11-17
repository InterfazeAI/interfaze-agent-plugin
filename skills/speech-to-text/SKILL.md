---
name: speech-to-text
description: Use when the user wants a transcript of an audio or video file, for example a voice note, call recording, meeting or podcast. Uses the Interfaze speech_to_text tool.
---

# Speech-to-text with Interfaze

Use the `speech_to_text` tool to transcribe speech in audio or video files.

## When not to use this skill

- The input is an image or a document. Use the `ocr` skill.
- The user wants a translation of text that they already have. Use the `translate` tool.

## Steps

1. Get a URL for the file. If the file is local, follow "Send a file" in the `interfaze` skill.
2. Call `speech_to_text`. Put the file URL in `files`.
3. In `prompt`, say what you need, for example:
   - "Transcribe the full recording."
   - "Transcribe the recording and label each speaker."
   - "Transcribe the recording with timestamps."
4. Give the user the transcript. If the user wants a summary, write the summary from the transcript.

## Read the result

The result has `text`, the full transcript, and `chunks`. Each chunk is one word with a `timestamp` (start and end, in seconds) and a `speaker` label, for example `SPEAKER_00`. To show speakers, join the words of each speaker turn into one line.
