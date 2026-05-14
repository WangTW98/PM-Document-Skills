# Gemini Adapter

Use the runner-neutral workflow at `../../SKILL.md`.

When invoking this skill from Gemini CLI or another Markdown-oriented runner:

1. Read `skill.yaml` for the input/output contract.
2. Read `SKILL.md` for the workflow and hard constraints.
3. Read `references/content-filter-rules.md` before filtering the page.
4. Read `references/figma-json-contract.md` and `references/pencil-json-contract.md` before writing output.
5. Process exactly one explicit Markdown file under `product/release/pages`.
6. Resolve output target from the user's language request:
   - `figma` only when explicitly requested
   - `pencil` only when explicitly requested
   - both when explicitly requested or when the target is unspecified
7. Read `product/release/product-sitemap-release.md` and all valid `product/release/layout/product-layout-release*.md` files to resolve one unique release layout family.
8. If the user explicitly provides a `design/<slug>/` spec or `design/<slug>/DESIGN.md`, use it as the primary style source.
9. If no design spec is provided, do not block; infer style from the matched layout and visible content only.
10. Strip all non-visual product logic, including product positioning, API contracts, analytics, backend logic, route mechanics, and invisible business rules.
11. Never process multiple pages in one run, even if the user references a whole module or sitemap batch.
12. Write only the requested target file set under `product/design` using the same relative path and basename as the source file.
13. If the layout match or visual interpretation is materially ambiguous, write a blocker file instead of guessing.
