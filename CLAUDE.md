# CLAUDE.md — kiro-plasma-keybindings

## Project overview

See [README.md](./README.md) for the user-facing description of this repo.

## Current state

Ships Kiro keybindings mostly as **add-only** hidden `.desktop` launchers in `usr/share/applications/` — each carries `NoDisplay=true` + `X-KDE-Shortcuts=`. This registers app-launch shortcuts in Plasma's `[services]` namespace without touching the built-in `[kwin]`/`[ksmserver]` defaults, so removal is clean and all users (not just `/etc/skel` accounts) get the bindings. Bindings are ported from the ohmychadwm `sxhkd` keymap.

**One exception — KWin actions can't be `.desktop` launchers.** `Super+Shift+Q` → Close Window (2026.06.19) is a built-in KWin action, not an app launch, so it can't ride the `X-KDE-Shortcuts` mechanism (which only runs an `Exec=`). It ships instead as a **minimal** `etc/skel/.config/kglobalshortcutsrc` carrying only the `[kwin] Window Close` line (`Alt+F4\tMeta+Shift+Q,Alt+F4,Close Window`) — not the old full frozen snapshot, so every other shortcut still defaults. New-accounts-only by design (skel). If another KWin/ksmserver action ever needs binding, extend this one minimal skel file rather than reaching for the all-users autostart route (considered, rejected for first-login reload fragility).

Standard EDU scaffold (`setup.sh`, `up.sh`) plus the required markdown files.

The PKGBUILD lives in [KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD](/home/erik/KIRO-PKG-BUILD-APPS/kiro-plasma-keybindings/PKGBUILD) and copies both `usr/` and `etc/` into the package.

### History

Previously shipped a full baked `etc/skel/.config/kglobalshortcutsrc` (snapshot of the whole shortcuts file, new-accounts-only, frozen, non-revertable). Replaced 2026.06.19 with the add-only `.desktop` mechanism above.

## Patterns & decisions

- Bash scripts follow the canonical template from Kiro-HQ — see [Kiro-HQ/CLAUDE.md](/home/erik/Insync/Kiro/Kiro-HQ/CLAUDE.md) and the canonical [up.sh](/home/erik/Insync/Kiro/Kiro-HQ/up.sh) / [setup.sh](/home/erik/Insync/Kiro/Kiro-HQ/setup.sh) / [cleanup.sh](/home/erik/Insync/Kiro/Kiro-HQ/cleanup.sh).
- README + CHANGELOG + IDEAS + TODO are the four other required scaffold files per the ecosystem MD-scaffold rule.

## Next steps

See [TODO.md](./TODO.md) for active work.
