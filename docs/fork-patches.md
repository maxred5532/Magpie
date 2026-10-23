# Fork patches

Local changes on top of `upstream` (SAOG0721/Magpie, branch `experimental`).
Keep this list current so upstream merges can be checked against it.

## Windowed toolbar: window drag handle

v0.6.9 made the toolbar background drag the toolbar, so the scaling window could no longer be moved from the toolbar. This patch adds a dedicated move button (first button, windowed mode only). Pressing it posts `WM_NCLBUTTONDOWN`/`HTCAPTION` to the scaling window.

Touched places (all small, additive):

- `src/Magpie.Core/OverlayDrawer.cpp`: `if (windowed) { ... }` block just before the Pin button in `_DrawToolbar`.
- `src/Magpie.Core/OverlayHelper.h`: `SegoeIcons::Move` and its `ICON_RANGES` entry.
- `src/Magpie.Core/ImGuiFontsCacheManager.cpp`: `FONTS_CACHE_VERSION` bumped by one. After a merge, make sure it is still higher than upstream's value.
- `src/Magpie/Resources.language-en-US.resw`, `Resources.language-zh-Hans.resw`: `Overlay_Toolbar_MoveWindow`.

If upstream later fixes window dragging itself, drop this patch.
