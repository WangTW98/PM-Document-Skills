# Gemini Adapter

Use the runner-neutral workflow at `../../SKILL.md`.

When invoking this orchestration skill from Gemini CLI or another Markdown-oriented runner:

1. Read `skill.yaml` for the orchestration contract.
2. Read `SKILL.md` for the one-page-per-round workflow.
3. Resolve the upstream source in this order:
   - `product/release/product-overview-release.md`
   - `product/release/product-sitemap-release.md`
4. Parse the sitemap page generation table and create or update `product/design/_md-design-status.md`.
5. Select exactly one unfinished page.
6. Read `../../product-md-design/skill.yaml` and `../../product-md-design/SKILL.md`.
7. Apply `product-md-design` rules to that one page only.
8. Respect the user-requested output target:
   - `figma` only when explicitly requested
   - `pencil` only when explicitly requested
   - both when unspecified or explicitly requested
9. Mark the selected page `done` or `blocked` in the status file.
10. Stop after one page. Continue with the next page only on the next invocation.
