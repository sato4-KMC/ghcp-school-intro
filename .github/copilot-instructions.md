# Project Guidelines

## Scope
- This repository is a small static website.
- Prefer simple HTML, CSS, and vanilla JavaScript over framework-based solutions.
- Keep changes minimal and focused on the requested task.
- The repository is managed in Japanese, so use Japanese by default for user-facing explanations unless asked otherwise.

## Code Style
- Follow the existing file and folder structure under `css/` and `js/`.
- Use clear naming for classes, ids, functions, and files.
- Avoid adding new dependencies unless they are clearly necessary.
- Do not perform unrelated refactors while handling a small change request.

## Implementation Conventions
- Prioritize readability over abstraction.
- Keep DOM logic straightforward and avoid over-engineered utility layers.
- Reuse existing styles and patterns before introducing new ones.
- Preserve responsive behavior when editing layout or styling.
- Treat [plan-gmail.md](../plan-gmail.md) as a planning document for intended structure and features, not as proof that those files already exist.
- Before implementing against planned files under `css/` or `js/`, verify whether they are actually present in the workspace.

## Build and Test
- There is no assumed build step.
- If verification is needed, prefer lightweight checks that fit a static site workflow.
- Do not assume Astro or other framework tooling is active just because the dev container mentions them; verify the actual project files first.

## Collaboration Rules
- Explain proposed changes in concise Japanese unless the user asks otherwise.
- When requirements are ambiguous, clarify the goal before making broad structural changes.
- If a request can be solved with a small edit, prefer editing the existing file over creating new files.
- When project documentation is sparse, inspect the current workspace state before relying on scaffold or container metadata.