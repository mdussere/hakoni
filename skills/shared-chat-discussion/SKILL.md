---
name: shared-chat-discussion
description: Capture a shared ChatGPT discussion (a chatgpt.com/share/... link) as raw Markdown in a workline's docs/ folder, with Playwright. Use when the user provides such a link as input material.
---

# Shared ChatGPT discussion

Use when the user provides a `chatgpt.com/share/...` link as input material.

1. **Capture with the skill's scraper**: it is the only accepted source. No HTTP extraction, search snippets or conversation rebuilt by hand:
   `uv run python skills/shared-chat-discussion/scrape_shared_chat.py --url <link> --output worklines/<workline>/docs/<NNNN_NN>_<name>.md`
2. **The raw export goes into the docs/ folder of the relevant workline.** The debug JSON (`--debug-json`) and logs stay outside the repository unless a review needs them.
3. **Prerequisites:** the `playwright` Python package (`uv sync`) and Chrome at `/usr/bin/google-chrome`. The browser opens visibly. If something is missing, report the exact error and ask before installing anything. Check for the `Exported <N> messages` line in the output.
4. **Links must stay clickable.** The scraper converts the assistant's answers to Markdown (`[text](url)` links, headings, lists, bold, code, tables); the user's messages stay plain text. Check that the export has links when the discussion cites sources.
5. **Check the raw export.** A remaining "Show more" means truncated text. The scraper must expand those controls before reading `innerText`: fix the scraper and rerun rather than changing source.
6. **Public repository:** read the export before committing it. A discussion that mentions confidential matters (employers, colleagues, internal projects or tools) is not committed as is: ask the user, and keep it outside the repository or commit an anonymised version.
7. **Cleaning, if the user asks:** `clean-verbatim`, in a separate document that links to the raw export. Keep the user's decisions, distinguish them from the assistant's proposals, and flag unverified technical claims.
8. **Effects on plans:** with the `workline` skill, read each affected plan, keep earlier evidence, and reopen only what the new direction invalidates. Update `docs/LOGBOOK.md`. Changing a plan does not authorise implementing it.
