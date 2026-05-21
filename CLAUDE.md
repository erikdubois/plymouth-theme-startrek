# Plymouth Theme: startrek

## Project overview
A Plymouth boot splash theme for Arch/Kiro Linux, styled after Star Trek aesthetics. Distributed as a standalone EDU repo and likely packaged in `nemesis_repo`.

## Structure
- `usr/share/plymouth/themes/startrek/` — theme assets: 24 progress PNGs (progress-0 to progress-23), `startrek.plymouth`, `startrek.script`
- `setup.sh` — git remote/identity configuration
- `up.sh` — pull → clean → commit → push workflow

## Animation pattern
The breathing glow uses two layered sprites:
- `logo_sprite`: `progress-0.png` at screen center, opacity 0.8→1.0
- `glow_sprite`: `glow.png` — pre-blurred 700×700 RGBA halo baked from progress-0.png (white + Gaussian blur σ=18 + 100px transparent border), `SetZ(-1)` (behind), opacity 0.0→0.45
- Sin wave: `breath = (Math.Sin(state.time * 0.05) + 1) / 2` — ~2.5s cycle at 50 Hz
- Why pre-baked: `progress-0.png` is opaque grayscale (no alpha) — runtime `Image.Scale()` on it produces an opaque rectangle, not a halo. The bake adds the alpha channel needed for a soft glow.

## Current state (2026-05-21)
Breathing-glow logo with real pre-blurred halo (`glow.png`) verified on Kiro ISO boot. Repo clean on `main`.

## Next steps
- Test theme on Kiro ISO boot (use `plymouthd --no-daemon --debug` + `plymouth --show-splash` to test without reboot)
- Consider packaging as AUR or `nemesis_repo` PKGBUILD
- Update README.md with install instructions
