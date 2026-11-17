---
name: web-search
description: Use when the user needs current information from the web, for example news, prices, release versions, facts that change, or sources for a claim. Uses the Interfaze web_search tool.
---

# Web search with Interfaze

Use the `web_search` tool to find current information on the web.

## When not to use this skill

- The user gives a URL and wants data from that page. Use the `web-scraping` skill.
- The question needs several searches and then a comparison or a report. Use `ask_interfaze` with the full task in `prompt`.

## Steps

1. Call `web_search`. Put the query or question in `prompt`. Be specific: include names, dates and places.
2. Read the results. Answer the user's question from the results.
3. Give the source URL for each fact that you use.

## Read the result

The result is a list. Each item has `title`, `url`, `description`, `snippets` and `content`. `content` is the full text of the page, so the result can be several hundred KB. Read `title`, `url` and `snippets` first. Read `content` only when the snippets do not answer the question.

## Tips

- If the results do not answer the question, change the query once. Do not repeat the same query.
- Say when the results disagree with each other.
