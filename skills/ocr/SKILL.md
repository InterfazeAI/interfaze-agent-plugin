---
name: ocr
description: Use when the user wants text or fields from an image, screenshot, scan, photo, PDF or Word document, for example a receipt, invoice or form. Uses the Interfaze ocr tool. Also use it when the user says "read this" or "what does this say".
---

# OCR with Interfaze

Use the `ocr` tool to read text from images, PDFs and Word documents (.docx).

## When not to use this skill

- The input is already text, JSON or CSV. Read it directly.
- The input is audio. Use the `speech-to-text` skill.
- The user wants object locations, not text. Use the `object-detection` skill.
- The user wants data from a live web page. Use the `web-scraping` skill.

## Steps

1. Get a URL for the file. If the file is local, follow "Send a file" in the `interfaze` skill.
2. Call `ocr`. Put the file URL in `files`.
3. In `prompt`, say what to extract:
   - All text: "Extract all text in reading order."
   - Fields: "Extract the merchant name, date, line items and total."
4. Give the user the extracted text or fields. Keep the field names that the user asked for.

## Tips

- Send one document per call when the user wants separate results for each document.
- For a long PDF, say which pages or sections you need.
