# Page Mock Draft Quality Checklist

Use this checklist before finishing each mock draft.

- Exactly one release page was read.
- Exactly one mock draft file was written under `product/development/mock`.
- The mock filename preserves the release page filename relative to `product/release/pages`.
- The mock document focuses only on display content and mock data.
- The mock document does not include action execution logic, analytics/tracking logic, API definitions, request/response structures, retries, or backend behavior.
- The mock document includes a `来源对照矩阵`.
- Every release page row from `页面布局与内容区块` is mapped or has an explicit unmapped reason.
- Every release page row from `页面元素清单` is mapped or has an explicit unmapped reason.
- Every release page row from `元素状态矩阵` is mapped or has an explicit unmapped reason.
- Every release page row from `交互 Action 与执行效果` is reviewed and mapped to visible content or has an explicit no-visible-content reason.
- Every visible content element from the release page appears in the mock content inventory.
- Every content row labels `内容来源类型` as `静态` or `动态`.
- Every dynamic content row includes `动态来源说明`.
- State-specific text is provided for relevant default/loading/empty/error/disabled/permission/payment/quota/offline states.
- Image/audio/video/file-preview content includes descriptions and alternative text where relevant.
- Inferred content is marked with `MA-*` or `MQ-*`.
- Every `MA-*` and `MQ-*` reference appears in the final unified list.
