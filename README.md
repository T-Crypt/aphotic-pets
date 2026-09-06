# aphotic-pets

Custom themeable pets for [Aphotic-Hypr](https://github.com/T-Crypt/aphotic-hypr).
See the [`pet` plugin](https://github.com/T-Crypt/aphotic-plugins/tree/main/pet)
for the manifest contract, the sheet layout, and how a pet is drawn.

A pet is a directory holding one sprite sheet and a `pet.json` manifest.
Installing one copies that directory into `~/.config/aphotic/pets/`. That
is the whole install, and the filesystem is the registry.

Every pet here declares which of its colours belong to the theme, so the
shell repaints it when your accent colour changes, whether that colour
came from a theme you picked or from a wallpaper you generated one from.

## Available pets

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

These three also travel with the `pet` plugin, so a current Aphotic
already has them and there is nothing to copy. This repository is where
they are kept, and where pets that do not ship go.

## Installing a pet

```sh
git clone https://github.com/T-Crypt/aphotic-pets ~/aphotic-pets
cp -r ~/aphotic-pets/kozumi ~/.config/aphotic/pets/kozumi
```

Then pick it in **Settings > Appearance > Desktop Pet**. The picker lists
that folder every time the pane opens, so a pet added while Settings is
already up wants one trip out of the pane and back.

The plugin reads `pet.json` and the sheet it names. It ignores everything
else in the directory, so copying the whole thing is safe.

The pet plugin is off until you ask for it:

```sh
aphotic plugin install pet
```

## What is in a pet directory

| Path | What it is |
|---|---|
| `pet.json` | The manifest. Names the sheet and the pet, and says the sheet uses the 11-row layout. |
| `spritesheet.webp` | The sheet, 8 columns by 11 rows of 192 by 208 cells. |
| `spritesheet.png` | The same art as PNG. Qt reads WebP only where `qt6-imageformats` is installed, and the plugin swaps to this when the first copy will not decode. |
| `preview.png` | The strip above. Five frames, for this README. |
| `look-directions.json` | Which cell of rows 9 and 10 faces which of the 16 directions. |
| `pet_request.json` | What the pet was generated from: the description, the atlas size, and what each row was asked to show. |
| `references/` | The canonical drawing every frame was matched against. |
| `qa/` | Contact sheets and the checks each row was accepted on. |

Rows 0 through 8 are idle, running-right, running-left, waving, jumping,
failed, waiting, running and review. Rows 9 and 10 are the look
directions.

## Adding your own

Open a PR with a new directory laid out like the three above. The three
files a pet cannot go without are `pet.json`, a sheet, and a
`preview.png` for the table.

Say in the PR which hue window your pet's accent colours live in and how
you measured it. The shader selects accents by hue rather than by a mask,
so a pet whose accent colours overlap its skin tones cannot be separated
this way, and that pet is better shipped with no accent block than tinted
into a rash. Kozumi is the worked example: her skin starts at hue 25 and
her coat trim runs to 15, so her window stops short of it.

The `pet` plugin's README covers the ways to draw or generate a sheet,
what each row has to contain, and the layout rules the art has to follow.

Art must be yours to license under GPL-3.0, or public domain.
