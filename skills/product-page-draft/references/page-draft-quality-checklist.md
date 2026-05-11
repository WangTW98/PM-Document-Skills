# Page Draft Quality Checklist

Use this checklist before finishing each page draft.

- Exactly one page MD file was created or updated.
- The target page came from one row in `Sitemap 页面生成总表`.
- The output path exactly matches the target row's `页面级MD文件`.
- The H1 exactly matches the `页面名称` from the sitemap row.
- The document includes Mermaid mind map or flowchart.
- The document includes tables for layout sections, element inventory, element states, actions, data models, APIs, edge cases, and media/resources.
- Every important page element has an element ID and specific name.
- Every important element has state definitions with triggers, style, available actions, disabled actions, feedback, and recovery.
- Every user action lists preconditions, validation, effects, success handling, and failure handling.
- API contracts include method, path, request structure, response structure, and behavior based on returned fields.
- Boundary states and error handling cover empty, loading, network error, permission, validation, quota, payment, backend failure, and device capability where relevant.
- Inferred content is marked with `PA-*` or `PQ-*`.
- Every `PA-*` and `PQ-*` reference appears in the final unified list.
- No unrelated sitemap page is generated in the same run.
