# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository is a **CLAUDE.md template** — a structured starting point for creating CLAUDE.md files in other projects. It contains a comprehensive template (`CLAUDE_TEMPLATE.md`) with placeholder sections covering architecture, build configuration, development rules, and more, along with supporting doc templates in `docs/`.

## Repository Structure

- `CLAUDE_TEMPLATE.md` — The main template file. Sections use `[bracket placeholders]` and HTML comments to guide users on what to fill in.
- `docs/CLAUDE.md/version-history.md` — Version history template (empty table).
- `docs/CLAUDE.md/future-improvements.md` — Ideas backlog template (checkbox list).
- `docs/CLAUDE.md/testing-checklist.md` — Example QA testing checklist (from an embedded BLE device project).
- `docs/CLAUDE.md/plans/` — Directory for plan, implementation, and audit records (epoch-prefixed directories).

## How This Repo Is Used

Users copy `CLAUDE_TEMPLATE.md` into their own project as `CLAUDE.md` and fill in the bracketed placeholders with project-specific details. The `docs/CLAUDE.md/` templates can be copied alongside it for supplementary documentation.

## Key Design Decisions

- The template is designed for both software and hardware/embedded projects — the Hardware section has a comment noting it should be removed for software-only projects.
- Development Rules in the template are intentionally opinionated (bounds checking, bounded string formatting, throttled output) since they prevent real bug classes found during QA.
- Large sections (testing checklists, changelogs) are kept in separate `docs/` files to keep the main CLAUDE.md scannable.