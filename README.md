# Explorer Visual Tweaks Dark

A set of visual tweaks for File Explorer's dark theme.

## What it does

- **Selection highlights** — replaces the built-in selection/hover/multi-select
  backgrounds in the file list and navigation pane with custom rounded
  backgrounds (configurable radius, fill and border colors), by hooking
  `DrawThemeBackground`/`DrawThemeBackgroundEx` in `uxtheme.dll`.
- **Navigation pane focus indicator** — a small colored pill next to the
  selected item in the folder tree, drawn the same way.
- **Disk usage progress bars** — redraws the drive-space bars in the
  navigation pane with a custom gradient fill and rounded corners, also via
  the `uxtheme.dll` hooks above.
- **Preview Pane background fix** — Explorer normally paints the Preview
  Pane frame with a fixed background color that doesn't match the file
  list's dark background. This mod replaces it with a configurable color
  (`191919` by default) by hooking `DirectUI::Element::PaintBackground`
  inside `DUI70.dll` and patching the DirectUI `Value` color field it
  paints from, directly, for the duration of the call. `DUI70.dll` isn't
  always loaded yet when the mod starts, so if it isn't, the hook is
  installed as soon as it loads, via `LdrRegisterDllNotification`.
- **Preview Pane text view (dark)** — the Preview Pane's plain-text view
  (a `RICHEDIT50W` control, used for `.txt` and similar files, hosted in
  `explorer.exe` and `prevhost.exe`) is themed separately, using the same
  `previewPaneBgColor` so the frame and the text view always match. Text
  color adjusts automatically for contrast against whatever background
  color is set, so it stays readable even if you set a light background.

The selection-highlight styling also applies inside Open/Save dialogs
(`IFileDialog`) hosted in other applications (added via Advanced →
Inclusion List), since those reuse Explorer's own shell view internally.
The Preview Pane fix currently stays scoped to `explorer.exe` — see the
comment above the `g_hostIsExplorer` check in the mod for why.

## Theme independence

Although the mod is named "Dark", nothing in it is hardwired to a dark
theme — every hook just paints whatever color is configured. The "Dark"
in the name refers only to the defaults: they're picked for dark theme.
To use it with any other theme, set your own colors for selection,
progress bars, and Preview Pane in Settings.

## Target processes

- `explorer.exe` — the main target; all features are active here.
- `prevhost.exe` — the COM surrogate that hosts the Preview Pane's text
  view for certain file types; only the Preview Pane text fix applies
  here.

Can also be added to other processes (browsers, Notepad, Photoshop, etc.)
via Advanced Settings → Inclusion List, to style their native Open/Save
file dialogs. See the mod's own comments (`IsShellHwnd`, `IsOwnedByCurrentProcess`)
for how it stays scoped to the right windows in that case.

## Settings

| Setting              | Description                                              |
| -------------------- | -------------------------------------------------------- |
| `cornerRadius`       | Corner radius for selection/hover highlights             |
| `borderWidth`        | Border width for selection/hover highlights              |
| `pill` (color)       | Navigation pane focus indicator color                    |
| `detailsPaneBgMatch` | Master on/off switch for Preview Pane background styling |
| `previewPaneBgColor` | Preview Pane background color (RRGGBB)                   |
| `progressRadius`     | Corner radius for disk usage progress bars               |

## Known limitations

- Preview Pane background fix only applies to `explorer.exe` and
  `prevhost.exe`; not yet verified in other Inclusion List processes.
- Requires `DUI70.dll` internals resolved by decorated C++ export names,
  which are undocumented and could change in a future Windows build.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
