# Explorer Visual Tweaks Dark

A Windhawk mod that fixes inconsistent colors and visual states in File
Explorer, primarily in the standard dark theme, and provides coordinated
customization for the affected elements in one place.

## What it does

- **File list selection (ItemsView)** — replaces selection, hover, and
  multi-selection backgrounds with configurable rounded backgrounds and
  corrects the visual artifact where a previously selected item can remain
  highlighted after another item is clicked.
- **Navigation Pane selection** — provides matching configurable backgrounds
  for selected and hovered items in the folder tree.
- **Navigation Pane focus indicator** — draws a configurable vertical focus
  pill next to the focused item.
- **Explorer progress indicators** — redraws the corresponding `PROGRESS`
  theme resources with configurable gradients, borders, and corner rounding.
  The replacement follows these resources wherever Explorer uses them rather
  than relying on window-size or window-tree heuristics.
- **Preview Pane background** — replaces mismatched Preview Pane frame and
  content backgrounds with one configurable color.
- **Plain-text preview** — applies the same background to the text preview and
  automatically chooses a light or dark text color and scrollbar style based
  on the configured background brightness.
- **Per-feature switches** — ItemsView, Navigation Pane, Progress, and Preview
  Pane customization can be enabled or disabled independently.

The selection styling also applies to Explorer-based Open/Save dialogs hosted
by applications added to Windhawk's **Advanced settings → Inclusion list**.

## Screenshots

![Explorer selections and drive progress](https://raw.githubusercontent.com/VitalSkib/files/refs/heads/main/explorer-visual-tweaks-dark-thispc.png)

![Preview Pane and plain-text preview](https://raw.githubusercontent.com/VitalSkib/files/refs/heads/main/explorer-visual-tweaks-dark-text.png)

## Theme independence

Selection and Progress rendering aren't tied to a particular theme. Their
default colors are tuned for the standard dark theme, but they can be changed
to suit another theme.

The Preview Pane correction is intentionally active only in the standard dark
app mode, where Explorer's color mismatch occurs. Its text color and scrollbar
style adapt automatically to the configured Preview Pane background.

## Target processes

- `explorer.exe` — all enabled features are active.
- `prevhost.exe` — hosts the plain-text Preview Pane control for applicable
  file types.
- Applications added through Windhawk's **Inclusion list** — supported for
  Explorer-based Open/Save dialogs.

## Settings

### File list selection (ItemsView)

Setting | Description
--- | ---
`customizeItemsView` | Enables file-list selection styling and ghost-highlight correction

### Navigation Pane

Setting | Description
--- | ---
`customizeNavigationPane` | Enables Navigation Pane selection customization
`showFocusPill` | Shows or hides the vertical focus indicator
`focusPillColor` | Focus indicator color (`RRGGBB`)

### Shared selection appearance

Setting | Description
--- | ---
`cornerRadius` | Selection corner radius (`0–6`)
`showBorder` | Enables the 1 px selection border
`activeFillColor` | Active selection and hover fill color (`RRGGBB`)
`activeBorderColor` | Active selection and hover border color (`RRGGBB`)
`multiFillColor` | Multi-selection fill color (`RRGGBB`)
`multiBorderColor` | Multi-selection border color (`RRGGBB`)

These settings are shared by whichever ItemsView and Navigation Pane blocks
are enabled.

### Progress indicators

Setting | Description
--- | ---
`customizeProgress` | Enables custom Progress rendering
`radius` | Progress corner radius (`0–6`)
`fillLeft`, `fillRight` | Normal progress gradient colors (`RRGGBB`)
`fullLeft`, `fullRight` | Full/warning progress gradient colors (`RRGGBB`)
`background` | Progress background color (`RRGGBB`)
`showProgressBorder` | Enables the progress background border
`backgroundBorder` | Progress border color (`RRGGBB`)

### Preview Pane

Setting | Description
--- | ---
`matchDetailsPaneBg` | Enables Preview Pane and plain-text preview customization
`previewPaneBgColor` | Shared Preview Pane and text-preview background (`RRGGBB`)

ItemsView, Navigation Pane, and Progress settings apply immediately. Preview
Pane changes require restarting File Explorer and any included host
applications.

## Compatibility

If another mod customizes the same Explorer element, disable the overlapping
block in this mod or the corresponding feature in the other mod. A disabled
ItemsView, Navigation Pane, or Progress block passes rendering through without
applying this mod's customization.

## Known limitations

- Preview Pane settings and complete restoration after disabling the Preview
  block require restarting File Explorer and any included host applications.
  The background brush is assigned when the target window class is registered
  and then remains owned by that class according to the documented
  [`WNDCLASSEXW::hbrBackground` contract](https://learn.microsoft.com/en-us/windows/win32/api/winuser/ns-winuser-wndclassexw).
- Third-party preview handlers can draw their own content, background, or
  scrollbar. Those surfaces aren't necessarily affected by Explorer's Preview
  Pane styling.
- The Selection renderer uses a private `DUI70.dll` hook target resolved by an
  exact decorated export name. A future Windows update could change it.
- The current implementation targets 64-bit Windows.

## Installation

### Option 1: Official Windhawk catalog (recommended)

Open Windhawk, search for **Explorer Visual Tweaks Dark**, and click
**Install**.

### Option 2: Manual installation

1. Copy the source code from
   [`explorer-visual-tweaks-dark.wh.cpp`](explorer-visual-tweaks-dark.wh.cpp).
2. Open Windhawk and select **Create a new mod**.
3. Paste the source code and click **Compile Mod**.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for
details.
