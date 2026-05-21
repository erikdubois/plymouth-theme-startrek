# Changelog

## 2026.05.21

### What Changed
- README.md fleshed out from a single placeholder line to a full install/usage guide. Documents requirements, install steps, live-preview-without-reboot procedure, uninstall, and packaging pointer.
- TODO.md updated: "Verify theme renders during Kiro ISO boot" closed (confirmed working); "Write install instructions in README.md" closed (this commit); "Test with `plymouth --show-splash` in a VM" dropped from backlog (covered by the README's live-preview section).

### Technical Details
- README structured around the canonical Arch/Kiro Plymouth install: copy theme → add `plymouth` hook to `mkinitcpio.conf` → `plymouth-set-default-theme -R startrek` → `quiet splash` in `GRUB_CMDLINE_LINUX_DEFAULT`. Includes a TTY-based preview using `plymouthd --debug` + `plymouth --show-splash` so the theme can be tested without rebooting.
- Mirrors the structure used by other EDU theme repos so a user landing on either repo gets the same install pattern.

### Files Modified
- README.md
- TODO.md
- CHANGELOG.md

## 2026.05.18

### What Changed
Replaced the 24-frame spinning logo animation with a static logo that breathes/glows. The rotation (cycling through `progress-0.png` to `progress-23.png`) was removed entirely. A sin-wave driven opacity pulse and a scaled glow halo sprite were added instead.

### Technical Details
Plymouth's `Sprite.SetOpacity()` and `Image.Scale()` were used to build a two-layer glow effect:
- Main logo: `progress-0.png` at fixed position, opacity oscillating 0.8→1.0
- Glow sprite: same image scaled 1.25×, centered behind the logo via `SetZ(-1)`, opacity oscillating 0.0→0.45
- Both driven by `Math.Sin(state.time * 0.05)` — approximately a 2.5-second breathing cycle at Plymouth's 50 Hz refresh rate
- Glow sprite is pre-scaled once at startup (no per-frame cost)

### Files Modified
- `usr/share/plymouth/themes/startrek/startrek.script`
- CHANGELOG.md (created)
- CLAUDE.md (created)
- TODO.md (created)
- IDEAS.md (created)
