# Page Mock Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- The selected input file is one page under `product/development/mock`.
- No more than one mock draft page file was read for release processing.
- No directory-level, all-pages, or multi-page release was performed by this single-page skill.
- The run stopped after producing at most one release mock page or one blocker file.
- No single-page input source outside `product/development/mock` was used.
- `## 13. 用户补充描述` was located when present, and its content was classified as empty/placeholder or non-empty user modification instructions.
- Every non-empty user supplement instruction was analyzed and applied to all affected release mock sections, or release was blocked with a clear conflict/ambiguity reason.
- Exactly one release mock page file was created or updated under `product/release/mock`, or exactly one blocker file was written.
- The release mock filename preserves the input mock filename relative to `product/development/mock`.
- The document version is `Release`.
- The document remains content-only: it contains no interaction execution, analytics, tracking, API contracts, backend behavior, or implementation code.
- The document contains no `MA-001`, `MA-002`, `MQ-001`, `MQ-002`, or other `MA-\d+` / `MQ-\d+` references.
- The document contains no `Mock 假设`, `Mock 待确认`, `假设`, `待确认`, `待用户确认`, `Release 处理方式`, `用户确认状态`, `用户确认结果`, or `置信度`.
- The document contains no `用户补充描述` section or raw user supplement notes.
- The final release mock page does not include the draft's `Mock 假设与待确认统一清单`.
- All formerly uncertain content has been applied, replaced, or removed according to the draft mock page's release handling.
- Source mapping, section content, element content, form options, mock records, media content, state copy, action-visible-content mapping, and content source summary remain internally consistent.
- User supplement changes are reflected consistently in source mapping, section content, element content, form options, mock records, media content, state copy, action-visible-content mapping, and content source summary when those areas are affected.
- Every retained content row includes `内容来源类型` with `静态` or `动态`.
- Every retained dynamic content row includes `动态来源说明`.
- No state content references a deleted element.
- No media content references a deleted section or element.
- No source mapping row points to removed release mock content without explaining the confirmed removal.
- If release is blocked, the blocker file lists only unresolved blocking items and no release mock page is written.
