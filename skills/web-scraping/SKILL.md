---
name: web-scraping
description: Use when the user gives a web page URL and wants data from it, for example prices, a table or a list of links. Uses the Interfaze scraper tool.
---

# Web scraping with Interfaze

Use the `scraper` tool to get structured data from a web page.

## When not to use this skill

- The user does not have a URL and wants information from the web. Use the `web-search` skill.
- The file is a PDF or an image. Use the `ocr` skill.

## Steps

1. Call `scraper`. In `prompt`, put the page URL and the data that you want. For example: "Get the name and monthly price of each plan from https://example.com/pricing."
2. Name each field that you want. The result is more consistent when you name the fields.
3. Give the user the data. Use a table when the result is a list of items with the same fields.

## Tips

- Use one call for each page.
- If the page needs a login, the tool cannot read it. Tell the user.
