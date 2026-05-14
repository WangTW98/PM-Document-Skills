# Gemini Adapter

Use the runner-neutral workflow at `../../SKILL.md`, but this skill requires a Figma Remote MCP-compatible environment.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the Figma creation workflow.
3. Read `references/figma-target-resolution-checklist.md` before any Figma write.
4. Read `references/design2figma-quality-checklist.md` after creating the Figma design.
5. Require one explicit design specification created by `visual-design-spec`, such as `design/<slug>/DESIGN.md` or `design/<slug>/`.
6. If the user did not explicitly specify the design specification, stop immediately and ask for it.
7. Require one Figma link from the user. If missing, stop immediately and ask for it.
8. Read `product/release/product-sitemap-release.md`, all release layout files under `product/release/layout`, and the selected release pages under `product/release/pages`.
9. If the user did not specify the application form, infer it from `product/release/product-sitemap-release.md` and the release layout metadata. If still ambiguous, stop and ask.
10. For each selected page, resolve one unique matched release layout family from `product-sitemap-release.md` plus all release layout md files. If no unique match exists, stop and ask or mark the page blocked.
11. Parse the provided Figma link and inspect the target Figma file/page before writing.
12. Create or reuse all release layout components first.
13. Before writing each page, run a visibility integrity gate: every section must have a parent container, explicit layout mode, size behavior, and overflow policy.
14. Resolve every required icon before icon creation. Do not silently skip icons. If an icon cannot be resolved, create a visible `ICON-MISSING-*` placeholder and treat the page as incomplete.
15. Then create only the user-selected release pages in Figma, and for each page use only its matched layout family. Do not mix layout families within one page.
16. After writing each page, run a repair loop until there is no unresolved overlap, clipping, truncation, hidden CTA, broken z-order, or missing icon.
17. Name every created page frame as `<sitemap 页面级MD文件 basename>-<页面ID>-<页面标题>`.
18. Do not use `product/release/design/*.md` as the primary source.
19. Do not process the full sitemap in this skill; use `product-all-pages-design2figma` for that.
