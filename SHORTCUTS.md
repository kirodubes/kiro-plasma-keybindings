# Kiro Plasma — Keyboard Shortcuts

Complete reference for the Kiro keybindings on KDE Plasma. `Super` and `Meta` are
the same key (the Windows/⌘ key); KDE labels it `Meta`.

There are **two categories**, by mechanism:

| Category | Mechanism | Touches KDE defaults? | Reverts cleanly? | Reaches |
|---|---|---|---|---|
| **A. Launchers** | add-only `.desktop` in `/usr/share/applications` (`X-KDE-Shortcuts`) | No | Yes | all users |
| **B. Overrides** | `apply-override-shortcuts.sh` writes `~/.config/kglobalshortcutsrc` | Yes (rebinds KWin) | No (opt-in) | per user, on run |

---

## A. Launchers — shipped by the `kiro-plasma-keybindings` package

Hidden `.desktop` files (`NoDisplay=true`) that only **add** shortcuts — they never
touch a Plasma built-in, so removing the package leaves every standard key working.
Installed system-wide; active after `kbuildsycoca6` or next login (registers ~10 s later).

| Shortcut(s) | Action | Command |
|---|---|---|
| `Meta+Return`, `Ctrl+Alt+Return` | Terminal | `alacritty` |
| `Meta+F8`, `Meta+Shift+Return` | File manager | `thunar` |
| `Ctrl+Alt+End` | System monitor | `alacritty -e btop` |
| `Ctrl+Alt+V` | Vivaldi | `vivaldi-stable` |
| `Ctrl+Alt+F` | Firefox | `firefox` |
| `Ctrl+Alt+B` | Brave | `brave --password-store=basic` |
| `Ctrl+Alt+C`, `Ctrl+Alt+G` | Chromium | `chromium -no-default-browser-check` |
| `Ctrl+Alt+O` | Opera | `opera` |
| `Ctrl+Alt+S` | Spotify | `spotify` |
| `Ctrl+Alt+D` | OBS Studio | `obs` |
| `Ctrl+Alt+E` | Tweak Tool | `archlinux-tweak-tool` |
| `Ctrl+Alt+A`, `Ctrl+Alt+Q` | Alacritty Tweak Tool | `alacritty-tweak-tool` |
| `Ctrl+Alt+P` | Pamac | `pamac-manager` |
| `Ctrl+Alt+U` | Pavucontrol | `pavucontrol` |
| `Ctrl+Alt+M` | Mintstick (ISO writer) | `mintstick -m iso` |
| `Ctrl+Alt+I` | Kiro ISO Builder | `kiro-iso-builder` |
| `Ctrl+Alt+Shift+F1`, `Meta+Ctrl+Shift+F1` | Update system | `update-system` |
| `Meta+X`, `Ctrl+Alt+K`, `Ctrl+Alt+L` | Logout | `archlinux-logout` |
| `Meta+Shift+X` | Power menu | `edu-powermenu` |
| `Meta+Ctrl+S` | Show keybindings | `kiro-keybindings` |

---

## B. Overrides — applied by `apply-override-shortcuts.sh`

These **must** override a built-in KWin action, so they can't be add-only. They live
in [`kiro-plasma/apply-override-shortcuts.sh`](https://github.com/erikdubois/kiro-plasma)
(local: `~/DATA/kiro-plasma/apply-override-shortcuts.sh`). Run it once per user to opt in:

```bash
~/DATA/kiro-plasma/apply-override-shortcuts.sh
```

Each F-key app is bound **only if it's installed** (otherwise the Plasma default is left
intact — no dead keys).

| Shortcut | Action | Replaces Plasma default |
|---|---|---|
| `Alt+F4`, `Super+Shift+Q` | Close Window | (adds `Super+Shift+Q`; `Alt+F4` kept) |
| `Super+F2` | VS Code | Switch to Desktop 2 |
| `Super+F3` | Inkscape | Switch to Desktop 3 |
| `Super+F4` | GIMP | Switch to Desktop 4 |
| `Super+F5` | Meld | Move Mouse to Focus |
| `Super+F6` | VLC | Move Mouse to Center |
| `Super+F7` | VirtualBox | Present Windows (Window class) |
| `Super+F9` | virt-manager | Present Windows (Current desktop) |

> Because these rebind built-ins, they don't revert on their own — resetting those
> actions in *System Settings → Shortcuts* (or re-running stock KDE) undoes them.

---

## Handled by Plasma natively (not shipped by either)

These ohmychadwm bindings are already covered by Plasma's own defaults, so Kiro doesn't
duplicate them:

- **Volume / brightness / media keys** — kmix / PowerDevil
- **Screenshots** — Spectacle (`Print`, `Meta+Shift+S`)
- **App launcher / run command** — KRunner (`Meta`, `Alt+Space`)
- **Window management** (workspaces, tiling, focus, move/resize) — KWin defaults
- **Wallpaper / compositor** — KDE wallpaper / KWin
