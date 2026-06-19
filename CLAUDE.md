# CLAUDE.md — kiro-plasma-keybindings

## Project overview

See [README.md](./README.md) for the user-facing description of this repo.

## Current state

Ships Kiro keybindings as **add-only** hidden `.desktop` launchers in `usr/share/applications/` — each carries `NoDisplay=true` + `X-KDE-Shortcuts=`. This registers shortcuts in Plasma's `[services]` namespace without ever touching the built-in `[kwin]`/`[ksmserver]` defaults, so removal is clean and all users (not just `/etc/skel` accounts) get the bindings. Bindings are ported from the ohmychadwm `sxhkd` keymap.

Standard EDU scaffold (`setup.sh`, `up.sh`) plus the required markdown files.

The PKGBUILD lives in [KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD](/home/erik/KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD) and copies `usr/` into the package (`_destname="/usr"`).

### History

Previously shipped a full baked `etc/skel/.config/kglobalshortcutsrc` (snapshot of the whole shortcuts file, new-accounts-only, frozen, non-revertable). Replaced 2026.06.19 with the add-only `.desktop` mechanism above.

## Patterns & decisions

- Bash scripts follow the canonical template from Kiro-HQ — see [Kiro-HQ/CLAUDE.md](/home/erik/Insync/Kiro/Kiro-HQ/CLAUDE.md) and the canonical [up.sh](/home/erik/Insync/Kiro/Kiro-HQ/up.sh) / [setup.sh](/home/erik/Insync/Kiro/Kiro-HQ/setup.sh) / [cleanup.sh](/home/erik/Insync/Kiro/Kiro-HQ/cleanup.sh).
- README + CHANGELOG + IDEAS + TODO are the four other required scaffold files per the ecosystem MD-scaffold rule.

## Next steps

See [TODO.md](./TODO.md) for active work.
