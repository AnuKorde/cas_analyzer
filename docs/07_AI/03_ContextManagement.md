# AI Context Management

**Document Version:** 0.1

**Status:** Draft

**Last Updated:** 2026-08-29

## 1. Purpose

AI coding assistants (like Gemini, Copilot, or Cursor) operate best when given the right amount of context. Too little, and they hallucinate incorrect architectures. Too much, and they lose focus.

## 2. The Golden Context File

The file `docs/project_context.md` is specifically designed as the primary entry point for AI assistants. It is concise, rule-heavy, and summarizes the entire project.

**Rule:** When opening a new chat or session with an AI assistant for a major task, always instruct it to read `docs/project_context.md` first.

## 3. Context Scoping

When working on specific features, provide only the relevant documentation to the AI:

- **Working on UI?** Feed the AI `project_context.md` + `docs/04_UI/00_UIArchitecture.md`.
- **Working on Parsers?** Feed the AI `project_context.md` + `docs/03_Parser/*.md`.
- **Working on Database?** Feed the AI `project_context.md` + `docs/02_Database/*.md`.

Avoid dumping the entire `docs/` folder into the context window unless necessary for high-level architectural refactoring.

