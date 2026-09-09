# crossecute brand mark

The mark is a lockup: a **check** joined to **spokes fanning out to four nodes**,
above the wordmark.

- **Check:** the verification step. Nothing moves until what you signed is proven
  to be what was built.
- **Spokes to nodes:** one origin carrying control outward to every destination
  chain. The nodes are uniform and interchangeable, because *any* chain works.

## Files

| File | Use |
|------|-----|
| `crossecute-icon-white.png` | Dark mark on a white tile, for light backgrounds |
| `crossecute-icon-black.png` | Light mark on a black tile, for dark backgrounds |
| `crossecute-avatar-512.png` | Square 512×512 avatar, mark only, no wordmark |

All three are raster PNGs with rounded corners and transparent corner pixels, so
they sit cleanly on any background. Pick the variant matching the surface behind
it, or serve both via `<picture>` and `prefers-color-scheme` (see
`profile/README.md`).

## Palette

The mark is monochrome.

| Token | Hex | Role |
|-------|-----|------|
| Ink | `#000000` | Mark on light surfaces; tile on the black variant |
| Paper | `#FFFFFF` | Mark on dark surfaces; tile on the white variant |

## Setting the GitHub org avatar

GitHub avatars are UI-only, must be raster, and are cropped to a square:

1. Go to **https://github.com/organizations/crossecute/settings/profile**
2. Under **Profile picture**, click **Upload a picture…**
3. Choose `crossecute-avatar-512.png`, position, and save.

Use the avatar file, not the lockups. The lockups are non-square, and their
wordmark becomes unreadable once GitHub crops and scales them down. The avatar is
the mark alone at 78% coverage, which stays clear at small sizes and survives the
circular crop GitHub applies in comment threads and member lists.
