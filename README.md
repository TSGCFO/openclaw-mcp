# openclaw-mcp

OpenClaw documentation, batch-scraped from https://docs.openclaw.ai using Firecrawl, organized as Markdown files. The corpus is intended as the source for a Skill Seekers–generated Claude/Agent skill packaged from the OpenClaw docs.

## Layout

- `docs/` — 541 markdown pages, mirroring the docs.openclaw.ai URL structure (English at the top level; localized variants under `de/`, `es/`, `fr/`, `it/`, `ko/`, `zh-CN/`, etc.).
- `MANIFEST.json` — array of `{ url, path, title }` for every scraped page.
- `urls.txt` — flat list of scraped URLs.

## Source

- Firecrawl batch scrape job: `019deea9-0355-7618-a39b-155d57dac299`
- Scraped 541 pages (2,705 Firecrawl credits)

## Skill generation

This corpus is fed into [Skill Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) to produce an `openclaw` skill (SKILL.md + categorized references), packaged as a `.zip` ready for upload to Claude / Perplexity Computer.
