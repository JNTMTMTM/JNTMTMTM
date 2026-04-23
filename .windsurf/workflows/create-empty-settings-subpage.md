---
description: create an empty paged sub-interface in settings
---
# Create Empty Settings Subpage Workflow

Use this workflow when adding a new empty child page (placeholder) inside an existing paged settings section, while keeping architecture and UI style consistent with existing modules.

1. Locate the target settings section component and identify existing page state, page keys, and dot navigation component usage.
2. Add a new page key (for example `feedback`) to the page key type/union and page list constant, keeping naming aligned with existing page keys.
3. Add translation labels for the new page in both `i18n/zh-CN.json` and `i18n/en-US.json` (do not rely only on `defaultValue`).
4. In the section component, add or update `pageLabels` mapping so the new page label is available for dot tooltips and title subtitle.
5. Add an empty render function for the new page (example: `renderFeedbackPage`) returning a placeholder panel container:
   - `<div className="settings-xxx-page-panel" />`
6. Mount the new page in the main content switch block, preserving existing page rendering logic and conditional order.
7. Ensure dot navigation includes the new page key and can switch to it (click and wheel behavior if this module already supports wheel switching in dot area).
8. If title uses section subtitle style, make subtitle follow current page label (e.g. `- {pageLabels[currentPage]}`).
9. Add/update CSS only if required:
   - Reuse existing paged layout classes first.
   - Add only minimal new classes for this section to keep style consistent.
10. Validate integration:
    - No type errors for page keys.
    - New page appears in dot navigation.
    - Switching between pages is smooth and content area does not break.
    - zh-CN/en-US labels both render correctly.

## Output checklist

- [ ] New page key/type wired
- [ ] Dot navigation includes new page
- [ ] Empty page panel mounted
- [ ] Both i18n files updated
- [ ] Subtitle (if used) reflects current page
- [ ] Style consistency preserved
