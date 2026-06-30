# Changelog

## 2026.06.30

### Add tweak-tool + logout-settings + lockscreen launchers; move Spotify to Meta+F10 (sync with ohmychadwm)
- **What Changed:** Added add-only `.desktop` launchers for `fish-tweak-tool` on
  `Ctrl+Alt+S`, `fastfetch-tweak-tool` on `Ctrl+Alt+Z` (+ the azerty/qwerty swap
  partner `Ctrl+Alt+W`), and `archlinux-betterlockscreen` on `Ctrl+Alt+R`, matching
  the ohmychadwm `sxhkdrc` source. Spotify previously
  squatted on `Ctrl+Alt+S` in this repo (drifted from source); moved it to
  `Meta+F10` (`super + F10`) where the source actually binds it, freeing the slot.
  Also split `Ctrl+Alt+L` out of the plain Logout launcher into its own
  `archlinux-logout --settings` launcher — the source binds `ctrl + alt + l` to
  `--settings`, not plain logout. The Logout launcher now carries only `Meta+X`
  and `Ctrl+Alt+K`.
- **Why:** ohmychadwm is the canonical keymap source. `fish-tweak-tool` and
  `fastfetch-tweak-tool` were the missing tweak-tool bindings here, the repo's
  Spotify shortcut had diverged from the source (causing the collision), and
  `Ctrl+Alt+L` was firing plain logout instead of the settings dialog.
- **Technical Details:** same hidden-launcher pattern (`NoDisplay=true` +
  `X-KDE-Shortcuts=`), `Icon=utilities-terminal` (fish-tweak-tool is terminal-based,
  consistent with the alacritty-tweak-tool launcher). PKGBUILD needs a `pkgrel` bump
  for the rebuild (`usr/` is copied wholesale).
- **Files Modified:**
  - `usr/share/applications/kiro-kb-fish-tweak-tool.desktop` (new)
  - `usr/share/applications/kiro-kb-fastfetch-tweak-tool.desktop` (new)
  - `usr/share/applications/kiro-kb-logout-settings.desktop` (new)
  - `usr/share/applications/kiro-kb-betterlockscreen.desktop` (new)
  - `usr/share/applications/kiro-kb-spotify.desktop` (`Ctrl+Alt+S` → `Meta+F10`)
  - `usr/share/applications/kiro-kb-logout.desktop` (drop `Ctrl+Alt+L`)
  - `README.md`, `SHORTCUTS.md` (keybinding tables)
  - `../KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD` (`pkgrel` 107 → 108)

### Regenerate the cheatsheet `keybindings.txt` from the real shortcut sources (28 bindings, was 1)
- **What Changed:** Rebuilt `etc/skel/.config/keybindings.txt` so the kiro-keybindings
  cheatsheet app shows all of Plasma's Kiro shortcuts. It previously listed a single
  binding (`alt + F4` Close window) because it was generated only from
  `kglobalshortcutsrc` — but every Kiro launch shortcut on Plasma actually lives in the
  `usr/share/applications/kiro-kb-*.desktop` files' `X-KDE-Shortcuts=` lines, which the
  generator never read. The file now carries all 28 bindings (27 launcher shortcuts +
  the KWin close-window action), sorted into the canonical Applications / Window
  Management / System & Session sections.
- **Why:** the cheatsheet was effectively empty on Plasma — opening it told the user
  almost nothing. Pairing this with the app-side path fix (kiro-keybindings) makes the
  searchable cheatsheet fully usable on the Plasma edition.
- **Technical Details:** ported each `kiro-kb-*.desktop`'s **primary** `X-KDE-Shortcuts`
  combo + its `Name=Kiro: <action>` text, normalized `Meta`→`super`, lowercased modifiers,
  and merged in the `kglobalshortcutsrc` `[kwin]` close-window line. Header `Source:` line
  updated to name both inputs. The `/kiro-keybindings-all` generator command was corrected
  to read both sources for Plasma going forward.
- **Files Modified:**
  - `etc/skel/.config/keybindings.txt` (regenerated: 1 → 28 bindings)
  - rebuild auto-bumps `pkgrel` via `build-data.sh` — no manual PKGBUILD edit

## 2026.06.21

### Add Variety wallpaper-switching shortcuts (next / previous / favorite)
- **What Changed:** Ported the ohmychadwm Variety bindings to Plasma as three new
  add-only `.desktop` launchers: `Meta+Alt+N` → `variety -n` (next), `Meta+Alt+P`
  → `variety -p` (previous), `Meta+Alt+F` → `variety -f` (favorite).
- **Why these key combos (not the sxhkdrc's plain `Alt+N/P/F` + `Alt+Arrows`):**
  plain `Alt+letter` and `Alt+Left/Right/Up/Down` would, as *global* Plasma
  shortcuts, shadow browser/Dolphin Back-Forward and app menu mnemonics. The
  `Meta+Alt+…` namespace is unused by KWin defaults, so the package keeps its
  conflict-free, clean-removal promise.
- **Scope:** only the three flags requested (`-n`/`-p`/`-f`). Trash (`-t`),
  `--toggle-pause`, `--resume`, and `--selector` were left out by choice.
- **Technical Details:** same hidden-launcher pattern as the rest of the package
  (`NoDisplay=true` + `X-KDE-Shortcuts=`), `Icon=variety`. No PKGBUILD logic change
  — `usr/` is copied wholesale; `pkgrel` bumped for the rebuild. Moved Variety out
  of the README/SHORTCUTS "handled natively / not shipped" sections.
- **Files Modified:**
  - `usr/share/applications/kiro-kb-variety-next.desktop` (new)
  - `usr/share/applications/kiro-kb-variety-previous.desktop` (new)
  - `usr/share/applications/kiro-kb-variety-favorite.desktop` (new)
  - `README.md`, `SHORTCUTS.md`
  - `../KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD` (`pkgrel` bump)

## 2026.06.20

### Switch file-manager keybinding from thunar to dolphin
- The `Meta+F8`, `Meta+Shift+Return` file-manager binding now launches
  `dolphin` instead of `thunar` — the native Plasma file manager fits this
  Plasma-targeted package.
- **Files Modified:** `usr/share/applications/kiro-kb-filemanager.desktop`,
  `README.md`, `SHORTCUTS.md`.

## 2026.06.19

### Add Super+Shift+Q to close the active window (KWin skel seed)
- Ported the ohmychadwm "kill focused window" binding to Plasma: `Super+Shift+Q`
  now also triggers KWin's **Close Window** action (the default `Alt+F4` is kept).
- **Why a different mechanism than the rest of the package:** closing a window is
  a built-in KWin action, not an app launch, so it can't ride the `/usr` add-only
  `.desktop` + `X-KDE-Shortcuts` mechanism (that can only run an `Exec=`). It has
  to live in the user's `kglobalshortcutsrc`.
- **How:** ships a **minimal** `etc/skel/.config/kglobalshortcutsrc` with only the
  `[kwin] Window Close` line — `Alt+F4\tMeta+Shift+Q,Alt+F4,Close Window` (real
  tab between the two active shortcuts). Not the old full frozen snapshot — every
  other shortcut still falls back to KDE defaults. New Kiro accounts pick it up at
  first login (no `kglobalaccel` reload needed). Trade-off accepted: new-accounts
  only, so existing upgraders don't get it retroactively.
- PKGBUILD now also packages `etc/` (previously `usr/`-only).

### Files Modified
- `etc/skel/.config/kglobalshortcutsrc` (new — minimal `[kwin]` seed)
- `../KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD` (package `etc/`)

### What Changed
- Switched the whole package from the baked `kglobalshortcutsrc` approach to **add-only `.desktop` launchers**. The old mechanism shipped a full snapshot of `kglobalshortcutsrc` into `/etc/skel/` (reached new accounts only, froze KDE defaults, never reverted on removal). The new mechanism ships hidden `.desktop` files in `/usr/share/applications/` that register shortcuts in Plasma's `[services]` namespace — system-wide for all users, never touching KDE's built-in `[kwin]`/`[ksmserver]` defaults, and cleanly removable.
- Ported the ohmychadwm `sxhkd` keymap to 20 `.desktop` launchers (terminal, browsers, tools, Kiro utilities). Verified on a Plasma 6 / KWin 6.7 Wayland VM that `NoDisplay=true` hides the menu entry while `kglobalacceld` still registers the shortcut.
- Dropped X11-only / natively-handled categories (volume, brightness, media keys, screenshots, wallpaper, compositor toggles, rofi/dmenu launchers) — these are covered by Plasma's own defaults / KRunner / Spectacle.
- Documented the 6 apps left unbound because their ohmychadwm keys collide with Plasma built-ins (VS Code, Inkscape, GIMP, VLC, VirtualBox, virt-manager on the `Super+F#` row).
- **Bugfix (same day):** first build of the package shipped to the wrong path and nothing registered. Two causes, both fixed:
  - PKGBUILD copied to `/usr/usr/share/applications/` (doubled `usr`) — the license step pre-creates `${pkgdir}/usr`, so `cp -r src/usr ${pkgdir}/usr` nested. Switched to `cp -a src/usr/. ${pkgdir}/usr/`.
  - Renamed all 20 launchers `kiro-*.desktop` → `kiro-kb-*.desktop` to avoid filename collisions with real launchers already in `/usr/share/applications` (`kiro-iso-builder.desktop`, `kiro-keybindings.desktop`).
  - Verified on the VM that registration is **async** (~10s after `kbuildsycoca6`/login) and that the `X-KDE-Shortcuts` default becomes the active key (confirmed non-empty key code via the kglobalaccel D-Bus interface).

### Technical Details
- Each launcher: `Type=Application` + `NoDisplay=true` + `X-KDE-Shortcuts=` (comma-separated for multiple keys; `\t`-style alternates not used). Shortcuts that collided with KWin/KDE defaults were skipped, not rebound, to preserve clean removal.
- PKGBUILD updated in the sibling repo: `_destname` changed from `/etc` to `/usr` so the package ships `usr/share/applications/` instead of `etc/skel/`. `readme.install` message updated accordingly.

### Files Modified
- usr/share/applications/kiro-kb-*.desktop (20 new launchers, `kiro-kb-` prefixed)
- etc/skel/.config/kglobalshortcutsrc, kglobalshortcutsrc-or (removed)
- README.md (rewritten for the add-only mechanism)
- CLAUDE.md (current state + history)
- ../KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD (_destname /etc → /usr)
- ../KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/readme.install (install message)

## 2026.05.21

### What Changed
- Initial markdown scaffold added per the ecosystem MD-scaffold rule ([HQ/CLAUDE.md](/home/erik/Insync/Kiro/Kiro-HQ/CLAUDE.md#required-markdown-scaffold-every-repo)).
- Stubs created for `CHANGELOG.md`, `CLAUDE.md`, `IDEAS.md`, `TODO.md` (whichever were missing).
- README rewritten with real install/usage content (replaced earlier one-line stub) where applicable.

### Files Modified
- CHANGELOG.md (created)
- CLAUDE.md (created where missing)
- IDEAS.md (created where missing)
- TODO.md (created where missing)
- README.md (rewritten where it was a stub)
