# aphotic-pets

Themeable desktop pets for [Aphotic-Hypr](https://github.com/T-Crypt/aphotic-hypr).

A pet is a directory holding one sprite sheet and a `pet.json`. Installing
one copies that directory into `~/.config/aphotic/pets/`. That is the whole
install, and the filesystem is the registry.

## The pets

| Pet | What it is |
|---|---|
| [`lumen`](lumen/) | A sealed lamp adrift in the dark. Its shell stays dark and its core takes the accent colour, so it shifts the most between themes. |
| [`cipher`](cipher/) | A developer at a floating console. His jacket seams, circuitry and interface glow carry the accent colour. |
| [`kozumi`](kozumi/) | A small figure at a laptop she carries. The trim on her coat and the red through her hair take the accent colour; her face does not. |

<p align="center">
  <img src="lumen/preview.png" width="820"><br>
  <sub>Lumen: resting, greeting, hopping, working, done</sub>
</p>

<p align="center">
  <img src="cipher/preview.png" width="820"><br>
  <sub>Cipher: resting, greeting, hopping, working, done</sub>
</p>

<p align="center">
  <img src="kozumi/preview.png" width="820"><br>
  <sub>Kozumi: resting, greeting, hopping, working, done</sub>
</p>

These three travel with the `pet` plugin, so a current Aphotic already has
them. This repository is where they are kept, and where pets that do not
ship go.

## Install one

```sh
git clone https://github.com/T-Crypt/aphotic-pets ~/aphotic-pets
cp -r ~/aphotic-pets/kozumi ~/.config/aphotic/pets/kozumi
```

Then pick it in **Settings > Appearance > Desktop Pet**. The picker lists
that folder every time the pane opens, so a pet added while Settings is
already up wants one trip out of the pane and back.

The pet plugin is off until you ask for it:

```sh
aphotic plugin install pet
```

## What a pet directory holds

| Path | What it is |
|---|---|
| `pet.json` | The manifest. Names the sheet and the pet, and says which colours wear the theme. |
| `spritesheet.webp` | The sheet, 8 columns by 11 rows of 192 by 208 cells. |
| `spritesheet.png` | The same art as PNG. Qt reads WebP only where `qt6-imageformats` is installed. |
| `preview.png` | The strip above. Five frames, for this README. |
| `look-directions.json` | Which cell of rows 9 and 10 faces which of the 16 directions. |
| `pet_request.json` | What the pet was generated from: the description, the atlas size, and what each row was asked to show. |
| `references/` | The canonical drawing every frame was matched against. |
| `qa/` | Contact sheets and the checks each row was accepted on. |

Rows 0 through 8 are idle, running-right, running-left, waving, jumping,
failed, waiting, running and review. Rows 9 and 10 are the look directions,
which the plugin does not draw.

The plugin reads `pet.json` and the sheet it names. It ignores everything
else in the directory, so copying the whole thing is safe.

## `pet.json`

```json
{
  "id": "cipher",
  "displayName": "Cipher",
  "description": "A developer at a floating console, with his jacket seams, circuitry and interface glow in the accent colour.",
  "spriteVersionNumber": 2,
  "spritesheetPath": "spritesheet.webp",
  "accent": {
    "from": 205,
    "to": 285,
    "feather": 10,
    "minSat": 0.18,
    "refSat": 0.38
  }
}
```

| Key | Meaning |
|---|---|
| `id` | The folder name. |
| `displayName` | Shown in the picker. |
| `description` | One line, shown under the name. |
| `spriteVersionNumber` | `2` for an 8 by 11 atlas. Anything earlier is read as 8 by 9, and a sheet read with the wrong row count hangs the top of the next row below the pet's feet. |
| `spritesheetPath` | The image beside the manifest. A bare filename: no slash, no leading dot, no traversal. |
| `accent` | Which colours wear the theme. Optional, and the reason to read the next section. |

### `accent`

This block is what makes a pet follow your colours. Leave it out and the
pet draws exactly as it was painted, whatever theme you are running, and
the **Wear the theme** row disappears from its settings.

The shell picks those colours by hue, since a generated pet is one flat
image with no mask in it. Name the hue window your recolourable regions
live in and the saturation floor that separates them from the pet's
neutrals. The shell then replaces hue, scales saturation toward the
theme's, and leaves value alone, so every cel-shading band stays where you
drew it. Skin, hair and dark cloth fall outside the window and pass
through untouched.

| Key | Meaning |
|---|---|
| `from` / `to` | The hue window holding the recolourable regions, in degrees from 0 to 360. The window wraps, so `320` to `15` is a legal red band. |
| `feather` | Degrees of soft edge at each end, so a gradient crossing the boundary fades rather than steps. Defaults to `10`. |
| `minSat` | Below this saturation a pixel counts as neutral and is left alone. Defaults to `0.18`. |
| `refSat` | The saturation the accents were drawn at. Saturation is scaled by theme over reference, so a muted theme mutes the pet in proportion. Defaults to `0.4`. |

Pick the window from the sheet's own hue histogram rather than from the
palette you asked for. Kozumi is the worked example: her coat trim and the
red in her hair run up to hue 15, her skin starts at 25, and her window
stops at 15 with a `feather` of 6 so the soft edge never reaches her face.

A pet whose accents overlap its skin tones cannot be separated this way.
Ship that one with no `accent` block rather than tinted into a rash.

## Make your own

The three here came from the `hatch-pet` skill in Codex, which draws all
eleven rows, registers them against one canonical reference, and packages
the atlas. This is the prompt that made Cipher:

> `$hatch-pet` Create a chibi anime boy based on a futuristic AI/developer
> aesthetic. Dark messy hair, dark technical jacket with luminous seams and
> circuitry details, boots, and a small floating translucent digital
> prompt/terminal panel beside or in front of him. The panel should look
> like an abstract glowing command interface rather than contain readable
> text. Jacket trim, interface glow, and circuitry accents should be clean
> recolorable regions for Wallust/Matugen

That last sentence matters more than the rest of it. Naming the
recolourable regions up front, and asking for them flat and clean, leaves
you a hue window you can find later. Drop it and the accents come back
smeared through the skin and the cloth, with nothing left to separate.

Two steps the skill does not do for you:

1. Add the `accent` block to `pet.json`. Open the sheet, read the hue of
   the regions you asked for, and write the window round them.
2. Export a `preview.png`: five frames on one strip, 820 pixels wide.

Any route that produces an 8 by 11 atlas of 192 by 208 cells works, drawn
by hand or assembled from strips. The
[`pet` plugin](https://github.com/T-Crypt/aphotic-plugins/tree/main/pet)
covers the sheet layout, what each row has to contain, and the rules the
art follows.

## Send one in

Open a PR with a new directory laid out like the three above. The three
files a pet cannot go without are `pet.json`, a sheet, and a `preview.png`
for the table.

Say in the PR which hue window your pet's accent colours live in and how
you measured it. A reviewer cannot check that one by looking at the sheet.

Art must be yours to license under GPL-3.0, or public domain.
