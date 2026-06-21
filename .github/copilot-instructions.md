# Copilot Collections Instructions

## Repository Scope

This repository stores GitHub Copilot customization artifacts for the `tsh` agent family and related specialized agents, prompts, and skills.

## Graphify-First Code Corpus Search Rule

- Whenever a `tsh`-family agent needs to search, explore, or understand a local code corpus, it should prefer graphify first when graphify is available in the current environment.
- Use graphify first for architecture discovery, ownership tracing, dependency mapping, related-file discovery, cross-module relationships, and broad semantic codebase questions.
- Fall back to standard search or symbol tools only when graphify is unavailable or when a narrower exact lookup is needed after the graphify-first pass.
- Do not skip graphify merely because another search tool could also work; the default posture for code-corpus exploration should be graphify first, then exact tools for confirmation.
