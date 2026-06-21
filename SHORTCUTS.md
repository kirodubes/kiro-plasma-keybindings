# Kiro Plasma — Launcher Shortcuts

Keyboard shortcuts shipped by the **`kiro-plasma-keybindings`** package. `Super` and
`Meta` are the same key (the Windows/⌘ key); KDE labels it `Meta`.

These are **add-only** launchers: hidden `.desktop` files (`NoDisplay=true`) in
`/usr/share/applications` carrying `X-KDE-Shortcuts`. They only *add* shortcuts and
never touch a Plasma built-in, so removing the package leaves every standard key
working. Installed system-wide; active after `kbuildsycoca6` or next login (registers
~10 s later).

| Shortcut(s) | Action | Command |
|---|---|---|
| `Meta+Return`, `Ctrl+Alt+Return` | Terminal | `alacritty` |
| `Meta+F8`, `Meta+Shift+Return` | File manager | `dolphin` |
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
| `Meta+Alt+N` | Variety: next wallpaper | `variety -n` |
| `Meta+Alt+P` | Variety: previous wallpaper | `variety -p` |
| `Meta+Alt+F` | Variety: favorite wallpaper | `variety -f` |

## Handled by Plasma natively (not shipped)

Already covered by Plasma's own defaults, so Kiro doesn't duplicate them:

- **Volume / brightness / media keys** — kmix / PowerDevil
- **Screenshots** — Spectacle (`Print`, `Meta+Shift+S`)
- **App launcher / run command** — KRunner (`Meta`, `Alt+Space`)
- **Window management** (workspaces, tiling, focus, move/resize) — KWin defaults
- **Compositor** — KWin

Variety wallpaper *switching* (next/previous/favorite) **is** now shipped above
(`Meta+Alt+N/P/F`); the static desktop wallpaper itself stays KDE-native.

## Shortcuts that override Plasma built-ins

A few ohmychadwm keys (close window, and the `Super+F#` app launches for VS Code,
Inkscape, GIMP, Meld, VLC, VirtualBox, virt-manager) **can't** be add-only because
they rebind a built-in KWin action.

**This package does not handle those** — it is strictly add-only and never runs a
script or modifies KWin defaults. Applying the overrides is a separate, manual,
per-user step in the **`kiro-plasma`** repo — see `apply-override-shortcuts.sh` and
its `SHORTCUTS.md` there.
