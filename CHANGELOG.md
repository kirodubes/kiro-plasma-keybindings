# Changelog

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
