---
description: add a new global hotkey end-to-end in eIsland
---
# Add Global Hotkey Workflow

Use this workflow when adding a new configurable global hotkey in eIsland, including main process registration, IPC bridge, renderer settings UI, and runtime behavior.

1. Define config key and reader in `src/main/config/storeConfig.ts`:
   - Add `DEFAULT_..._HOTKEY` constant.
   - Add `..._HOTKEY_STORE_KEY` constant.
   - Add `read...HotkeyConfig()` helper.
2. Extend hotkey service in `src/main/services/hotkeyService.ts`:
   - Add read callback and trigger callback in `CreateHotkeyServiceOptions`.
   - Add current hotkey cache field.
   - Add `getCurrent...Hotkey()` and `register...Hotkey()`.
   - Include new hotkey in `suspendIslandHotkeys()` and `resumeIslandHotkeys()`.
3. Add IPC handlers in `src/main/ipc/system/hotkey.ts`:
   - Extend options interface with new key/read/get/register fields.
   - Add `...-hotkey:get` and `...-hotkey:set` handlers.
   - Reuse existing duplicate-hotkey guard pattern.
   - Persist to store when registration succeeds.
4. Wire main process integration in `src/main/index.ts`:
   - Import new config constants/readers.
   - Pass new callbacks into `createHotkeyService(...)`.
   - Pass new fields into `registerHotkeyIpcHandlers(...)`.
   - Include new hotkey in reserved list logic (if needed by screenshot/capture conflict checks).
   - Register persisted hotkey on app startup.
5. Expose preload APIs in `src/preload/index.ts`:
   - Add `...HotkeyGet()` and `...HotkeySet()` wrappers.
6. Update preload typings in `src/preload/index.d.ts`:
   - Add method signatures on `window.api`.
7. If the hotkey triggers UI action, connect runtime event channel:
   - Broadcast from main (usually `broadcastSettingChange(...)`).
   - Handle channel in renderer (for example in `DynamicIsland.tsx` via `onSettingsChanged`).
8. If behavior requires state guarding, add store-level control:
   - Add state flag and toggler in `src/renderer/store/types/index.ts` and `src/renderer/store/slices/islandSlice.ts`.
   - Intercept relevant state transitions at the store boundary.
9. Add settings page wiring in `src/renderer/components/states/maxExpand/components/SettingsTab.tsx`:
   - Local state/ref/recording/error for the new hotkey.
   - Load initial value from preload `...HotkeyGet()`.
   - Add keydown handler with accelerator conversion and duplicate checks.
10. Add settings section UI in `src/renderer/components/states/maxExpand/components/setting/components/shortcut/ShortcutSettingsSection.tsx`:
   - Add props for input/ref/handlers.
   - Add card with Edit/Clear interaction matching existing hotkey cards.
11. Add i18n keys in both files:
   - `i18n/zh-CN.json`
   - `i18n/en-US.json`
12. Validate:
   - Run `npm run build` in `eIsland`.
   - Verify hotkey can be set/cleared.
   - Verify duplicate combinations are rejected.
   - Verify hotkey action works and persisted value reloads after restart.

## Output checklist

- [ ] Store config constants and reader added
- [ ] Hotkey service registration lifecycle completed
- [ ] IPC get/set handlers completed
- [ ] Main process wiring and startup registration completed
- [ ] Preload APIs + typings completed
- [ ] Renderer behavior channel wired
- [ ] SettingsTab state/handler wired
- [ ] ShortcutSettingsSection UI card added
- [ ] zh-CN / en-US i18n added
- [ ] Build passes and basic behavior verified
