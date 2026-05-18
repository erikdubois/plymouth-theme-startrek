# Changelog

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
