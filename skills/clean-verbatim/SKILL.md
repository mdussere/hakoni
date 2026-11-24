---
name: clean-verbatim
description: Clean up a voice-recorded verbatim (usually French) that the user has dropped into a markdown file. Translate to English, structure it, add a title, summary, and detailed summary, and flag likely mistranscriptions. Work in-place in the file the user provided. Use when the user asks to "clean", "process", or "structure" a verbatim / transcription / monologue, or refers to this skill.
---

# clean-verbatim

The user voice-records a monologue (most often in **French**) and drops the raw transcript into a markdown file. They want it turned into a clean, structured English document — **edited in place in the same file**.

## What the output looks like

The file ends up with this structure:

1. **Title** — `# <Title>` synthesised from the content.
2. **Summary** — **very short**: 2–4 sentences, the headline only. Not a recap, not a paragraph-of-everything. If you find yourself writing 10 lines, you've drifted into the detailed summary.
3. **Detailed summary** — bolded sub-headings (problem, current strategy, proposal, difficulties, tool shape, plan, …) with short bullets. Headings reflect *the content*, not a fixed template. This is where the longer recap lives.
4. **Verbatim** — the cleaned, translated, section-headed transcript, preceded by a short blockquote header noting:
    - that it is translated from spoken French (or whatever source language),
    - the convention used for ambiguous French (kept in parentheses),
    - speaker, date, duration, and original recording filename if present in the raw text.
    Then numbered section headers (`### § 1. <name>`, `### § 2. <name>`, …) that the agent adds for readability.

## Rules for the cleaned verbatim

- **Translate to English.** Do not keep a parallel French version.
- **Preserve French in parentheses *only* where the original wording is genuinely ambiguous, a likely mistranscription, or a term-of-art that loses meaning when translated.** Format: `English (*"raw French"* — short note on what is likely meant)`.
- **Flag likely mistranscriptions inline** the same way. Common cases to watch for:
    - English technical terms spoken inside French and butchered by the transcriber (e.g. *"le dit pageant"* for "DeepAgent", *"faire gaming"* for "Safer Gaming").
    - Project-specific names, product names, internal jargon.
    - Words that don't fit the sentence semantically — those are usually transcription errors, not the speaker's intent.
- **Ask the user interactively to disambiguate suspected mistranscriptions** before finalising the file. Use `AskUserQuestion` with one question per suspect (batch them in a single call when there are several), giving the raw French and your best guesses as options. **Always include the line number from the source file in each question** (e.g. "Line 36: 'Les clusters, c'est des outils de data science dans iKiller…'") so the user can jump to the context in their editor without re-reading the whole file. Apply the user's choices into the verbatim — keep the parenthetical note only when the disambiguation itself is worth recording. Do not silently pick a guess; do not pepper the file with `(?)` markers and call it done.
- **Add `### § N. <section name>` headers** to break the monologue into readable sections. These are an editorial addition by the cleaner; say so in the header blockquote.
- **Do not invent content.** No summarising-by-paraphrase inside the verbatim, no smoothing-away of hesitations that carry meaning, no adding examples the speaker did not give. Light cleanup of filler words ("euh", "donc", repetitions) is fine; substantive rewording is not.
- **Keep the speaker's voice.** First person, conversational register, the speaker's own analogies. The verbatim is a *cleaned transcript*, not a polished essay.

## Rules for the summaries

- **Summary** is written *by the cleaner*, in the cleaner's voice (third person, descriptive). **Really short — 2–4 sentences, the headline only.** Detail belongs in the detailed summary, not here.
- **Detailed summary** is also third-person, structured by content (problem → current state → proposal → open questions → plan), not by a fixed template. Short bullets, bolded sub-headings.
- Both summaries should be **faithful to the verbatim** and not introduce claims the speaker did not make.

## Working in place

- **Edit the file the user provided.** Do not create a new file, do not rename, do not move. If the file already has French raw text, replace it; the cleaned structure *is* the file.
- If the user pasted the raw transcript at the top of an otherwise-empty file, the final file should contain only the four sections above (title, summary, detailed summary, verbatim).
- Preserve any existing surrounding content the user has clearly put there on purpose (e.g. front-matter, a link block at the bottom). If unsure, ask before deleting.

## When to ask

Default to acting — the user prefers in-place edits without ceremony. Only ask when:
- The source language is unclear (e.g. mixed French/English and you can't tell which is the base).
- The raw text is so fragmented that you cannot reliably reconstruct sections without inventing.
- The file contains material that does *not* look like a verbatim and you might overwrite it.
