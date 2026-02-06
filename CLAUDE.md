# CLAUDE.md

This file provides guidance to Claude Code when working with the yosh-website repository.

## Project Overview

This is the landing page / marketing website for **Yosh**, an LLM-enabled shell. It's a single `website/index.html` file — all CSS and JS are inline. No build system, no dependencies, no frameworks.

The companion project is at `../yosh/` — read its `CLAUDE.md` and plan files for deep context on what yosh does and how it works.

## How to Preview

Open `website/index.html` directly in a browser. That's it.

## Page Sections (in order)

1. **Header** — "Yosh" title + tagline
2. **Terminal demo** — Animated terminal showing the yo workflow (the centerpiece)
3. **About** — One-paragraph description
4. **Features** — 6-card grid (Natural Language, Session Memory, Terminal Aware, Multi-Step Workflows, Memory-Safe, It's Just Bash)
5. **Download** — Button with placeholder URL (`https://example.com/yosh-latest.tar.gz` — search for `example.com` to replace)
6. **Footer**

## Design Decisions

- **Page theme**: Low-contrast cyan. Dark teal background (`#0c191d`), muted cyan text. Sans-serif font.
- **Terminal theme**: Black background, standard shell colors:
  - `pizlo@traitor` — green (`#4ec940`)
  - `~` — blue (`#5c78ff`)
  - LLM responses — cyan (`#5ed7d7`) + italic
  - Everything else — white
- **Terminal chrome**: GNOME Terminal style (not macOS). Dark gray titlebar with centered title, minimize/maximize/close on the right.
- **Yosh is Linux-only** (x86_64), so the terminal should never look like a Mac.

## Terminal Animation

The animation is **data-driven** — the sequence and content are defined at the top of the `<script>` block and the engine that plays them is below. To change what the demo shows:

### Key variables (top of script)

| Variable | Purpose |
|----------|---------|
| `TYPING_SPEED` | ms per character for typing animation |
| `TYPING_JITTER` | random variation in typing speed |
| `LOOP_DELAY` | ms pause at end before the animation restarts |
| `EXPLAIN_RESPONSE` | Array of lines for the chat response |
| `SEQUENCE` | Array of animation steps (the main thing to edit) |
| `PROMPT` | HTML for the shell prompt (`pizlo@traitor:~$`) |

### Animation step types

| Action | Properties | What it does |
|--------|-----------|--------------|
| `prompt` | — | Appends the PS1 prompt |
| `type` | `text` | Types text character-by-character |
| `pause` | `ms` | Waits |
| `enter` | — | Newline |
| `thinking` | `ms` | Shows pulsing "Thinking..." then clears it |
| `llm-line` | `text` | Single cyan italic line (for command explanations) |
| `llm-block` | `lines: []` | Multi-line cyan italic block (for chat responses) |
| `prompt-prefill` | `text` | Prompt with a pre-filled command |
| `output` | `text` | Plain white output line |

### Text formatting in LLM responses

The `formatLlm()` function processes text in `llm-line` and `llm-block` steps. Currently it just escapes HTML. The CSS classes `.llm b` and `.llm .code` are defined for bold and inline-code styling if you want to add `**bold**` and `` `code` `` parsing back to `formatLlm()`.

## Reference: story.txt

The `story.txt` file contains the storyboard that describes the animation flow in plain text. If you change the animation, consider updating `story.txt` to match.
