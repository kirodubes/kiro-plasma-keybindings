<p align="center">
  <img src="kiro.jpg" alt="Kiro" width="220" />
</p>

# kiro-plasma-keybindings

Kiro keybindings for KDE Plasma, shipped the clean, **add-only** way: a set of hidden `.desktop` launchers in `/usr/share/applications/`, each carrying an `X-KDE-Shortcuts=` line. This gives you twm-style launch keys (terminal, browsers, tools) on Plasma without ever touching Plasma's own built-in shortcuts.

The bindings are ported from the ohmychadwm `sxhkd` keymap so the muscle memory carries across desktops.

## Why add-only `.desktop` files

Each file looks like this:

```ini
[Desktop Entry]
Type=Application
Name=Kiro: Terminal
Exec=alacritty
NoDisplay=true                       # hidden from menus / Kickoff / KRunner
X-KDE-Shortcuts=Meta+Return,Ctrl+Alt+Return
```

- **`NoDisplay=true`** keeps the shortcut live but hides the entry from menus — no launcher clutter (verified: `kglobalacceld` still registers the shortcut).
- **System-wide** — installed to `/usr/share/applications/`, so *every* user gets the bindings (not just freshly-created accounts).
- **Clean removal** — these only *add* shortcuts in the `[services]` namespace; Plasma's built-in `[kwin]`/`[ksmserver]` defaults are never modified. Remove the package and every standard Plasma key still works (worst case: an inert orphan line in a user's `~/.config/kglobalshortcutsrc`).
- **Future-proof** — it never freezes a snapshot of `kglobalshortcutsrc`, so new KDE default shortcuts arrive untouched across Plasma upgrades.

> This replaces the earlier approach of shipping a full baked `kglobalshortcutsrc` into `/etc/skel/` (new-accounts-only, frozen, non-revertable).

## Keybindings shipped

| Shortcut(s) | Action |
|---|---|
| `Meta+Return`, `Ctrl+Alt+Return` | Terminal (alacritty) |
| `Meta+F8`, `Meta+Shift+Return` | File manager (dolphin) |
| `Ctrl+Alt+End` | btop |
| `Ctrl+Alt+V` | Vivaldi |
| `Ctrl+Alt+F` | Firefox |
| `Ctrl+Alt+B` | Brave |
| `Ctrl+Alt+C`, `Ctrl+Alt+G` | Chromium |
| `Ctrl+Alt+O` | Opera |
| `Meta+F10` | Spotify |
| `Ctrl+Alt+D` | OBS Studio |
| `Ctrl+Alt+E` | archlinux-tweak-tool |
| `Ctrl+Alt+A`, `Ctrl+Alt+Q` | alacritty-tweak-tool |
| `Ctrl+Alt+S` | fish-tweak-tool |
| `Ctrl+Alt+Z`, `Ctrl+Alt+W` | fastfetch-tweak-tool |
| `Ctrl+Alt+P` | Pamac |
| `Ctrl+Alt+U` | Pavucontrol |
| `Ctrl+Alt+M` | Mintstick (ISO writer) |
| `Ctrl+Alt+I` | Kiro ISO Builder |
| `Ctrl+Alt+Shift+F1`, `Meta+Ctrl+Shift+F1` | update-system |
| `Ctrl+Alt+R` | archlinux-betterlockscreen (lock screen) |
| `Meta+X`, `Ctrl+Alt+K` | archlinux-logout |
| `Ctrl+Alt+L` | archlinux-logout --settings |
| `Meta+Shift+X` | edu-powermenu |
| `Meta+Ctrl+S` | Show keybindings |
| `Meta+Alt+N` | Variety: next wallpaper (`variety -n`) |
| `Meta+Alt+P` | Variety: previous wallpaper (`variety -p`) |
| `Meta+Alt+F` | Variety: favorite wallpaper (`variety -f`) |

## Deliberately handled by Plasma natively (not shipped)

These ohmychadwm bindings are X11-only or already covered by Plasma's own defaults, so they are intentionally **not** included:

- **Volume / brightness / media keys** — handled natively by kmix / PowerDevil.
- **Screenshots** — use Spectacle (`Print`, `Meta+Shift+S`).
- **App launcher** (rofi / dmenu / appfinder) — use KRunner (`Meta`, `Alt+Space`).
- **pywal** and **compositor toggles** (picom / fastcompmgr) — X11-only, not portable to KWin.

> Variety wallpaper *switching* (next/previous/favorite) **is** shipped — see the
> `Meta+Alt+N/P/F` rows above. The static desktop background stays KDE-native.

## Not bound — collide with Plasma built-ins

Because this is strictly add-only, a handful of ohmychadwm apps land on keys Plasma already owns and are **left unbound** (reclaiming them would mean editing KWin's defaults, which breaks clean removal). Assign these by hand in *System Settings → Shortcuts* if you want them:

| App | ohmychadwm key | Plasma built-in using it |
|---|---|---|
| VS Code | `Super+E` / `Super+F2` | Dolphin / Switch to Desktop 2 |
| Inkscape | `Super+F3` | Switch to Desktop 3 |
| GIMP | `Super+F4` | Switch to Desktop 4 |
| Meld | `Super+F5` | Move Mouse to Focus |
| VLC | `Super+F6` | Move Mouse to Center |
| VirtualBox | `Super+F7` | Present Windows (Class) |
| virt-manager | `Super+F9` | Present Windows |

## Installation

### From `nemesis_repo` (recommended)

```ini
[nemesis_repo]
SigLevel = Never
Server = https://erikdubois.github.io/$repo/$arch
```

```bash
sudo pacman -Syu
sudo pacman -S kiro-plasma-keybindings
```

### Manual

```bash
git clone https://github.com/kirodubes/kiro-plasma-keybindings.git
cd kiro-plasma-keybindings
sudo cp -r usr/. /usr/
```

Plasma picks up new `.desktop` shortcuts when the service cache refreshes — run `kbuildsycoca6` or simply log out and back in.

## Websites

Information : https://erikdubois.be

## Social Media

Youtube : https://www.youtube.com/erikdubois

<!-- KIRO-FUNDING-FOOTER:START — managed by Kiro-HQ/cascade-readme-footer.sh -->
## Help fund Kiro

Everything I build here stays free and open — always. If Kiro or any of these
tools have ever saved you time or taught you something, a small monthly
contribution helps keep the work going. Donations target break-even, nothing
more — the core always stays free for everyone.

- GitHub Sponsors: https://github.com/sponsors/erikdubois
- Patreon: https://www.patreon.com/c/kiroproject
- YouTube memberships: https://www.youtube.com/@ErikDubois/join
- Ko-fi: https://ko-fi.com/erikdubois
- PayPal: https://www.paypal.me/erikdubois
<!-- KIRO-FUNDING-FOOTER:END -->

## License

See [LICENSE](./LICENSE).
