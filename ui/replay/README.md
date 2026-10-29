# DLC era key art (`ui/replay`)

One PNG per DLC era, used by the MikuSB admin panel's era replay page
(`GET /api/replay/image/{id}`). They are client art, so they live here in the
resource repository rather than in the server repository.

| File | Where it comes from |
| --- | --- |
| `dlc-5.png` .. `dlc-11.png`, `dlc-23.png` .. `dlc-34.png`, `dlc-35.png`, `dlc-36.png` | the lobby entry button of that era: `Game/Content/UI/Picture/DLC{nn}/gui_dlc*btn001_01_Mul` |
| `dlc-1.png` | the era's promo key art (`activities.json` `BG` -> `Picture/Activity/pic_activity19_001`), because era 1 has no DLC button asset in any unpack |

Eras 2, 3 and 4 have no file: their `Picture/DLC02..04` folders do not exist in any
unpacked build, and every `Picture/Localization/DLC2..4` asset is a 324-byte
placeholder. The panel shows a neutral gradient card for them instead (see
`DlcEraPolicy` - those eras are withdrawn anyway).

## Regenerating

From the server repository, with the client unpacked next to it:

```
python scripts/export_era_images.py [path/to/Game/Content/UI ...]
```

The script decodes BC1/BC3 with pure Python and ASTC through `scripts/deps/bin/astcenc-avx2.exe`,
picks the entry button first and falls back to promo art, and writes
`Resources/ui/replay/dlc-{id}.png` at 640 px wide. Newer eras can simply be re-run:
the admin panel caches on the folder's file count and newest timestamp, so it picks
up the change without a restart.
