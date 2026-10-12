# plaiiin Slide Themes

The deck themes of [plaiiin Docs](https://plaiiin.com). The app reads this repository directly: it fetches `catalog.json` to see which themes exist and at which version, then a theme's colours and thumbnail, and its photo and backgrounds the first time a deck uses it. When a theme's `version` goes up, every app fetches it again.

| Theme | Look | Page | Title slide |
|---|---|---|---|
| Prism | Bold | dark | top left |
| Daybreak | Bold | light | left |
| Dune | Bold | light | bottom left |
| Botanic | Soft | light | top left |
| Sakura | Soft | light | left |
| Skyline | Soft | light | top left |
| Riviera | Editorial | light | left |
| Tower | Editorial | light | left |
| Pavilion | Editorial | light | top left |
| Orbit | Spotlight | dark | top left |
| Aurora | Spotlight | dark | bottom left |
| Graphite | Spotlight | dark | top left |

Each theme sets the title slide on its photo and each section's divider on a veiled copy of it. Content slides take the theme's backgrounds in turn — quieter pictures with room for text — under a veil of the page; a theme with none keeps a plain page.

## Layout

```
catalog.json                 which themes, at which version
themes/<id>/theme.json       colours, look, where the title sits
themes/<id>/photo.jpg        the photo, 16:9
themes/<id>/thumbnail.jpg    480 px wide, for the picker
themes/<id>/background-N.jpg any number, 16:9, behind content slides in turn
```

### catalog.json

```json
{ "format": 1, "name": "plaiiin Slide Themes",
  "themes": [ { "id": "riviera", "version": 1 } ] }
```

### theme.json

| Field | Meaning |
|---|---|
| `format` | `1`. An app skips a theme of a format it does not know. |
| `id` | Folder name: letters, digits, `-`. |
| `name` | Shown in the picker. |
| `version` | Raise it with every change, and the same number in `catalog.json`. |
| `look` | `bold`, `soft`, `editorial` or `spotlight`: type, dividers, header and footer. |
| `appearance` | `light` or `dark`: the content slides' page is fixed. |
| `page`, `text`, `accent` | `#RRGGBB`: the content slides' page, body text and accent. |
| `sections` | `#RRGGBB` list: the colours sections take in turn. |
| `photo.image`, `photo.thumbnail` | File names in the theme's folder. |
| `photo.title` | `topLeading`, `leading` or `bottomLeading`: the photo's quiet side. |
| `photo.ink` | `#RRGGBB`: the title's colour on the photo. |
| `photo.scrim` | `0`–`1`: a shade behind the title, the opposite of its ink, fading across the slide. |
| `photo.backgrounds` | File names, optional: the pictures content slides take in turn, as many as you like. |
| `photo.veil` | `0`–`1`, optional, default `0.35`: how much of the page lies over a background so text reads. |

## Your own themes

Any folder on the web laid out like this one is a theme source: a fork of this repository, or a repository of your own. Add it in plaiiin Docs under Settings → Slides → Theme Sources, as the repository's GitHub page or as the folder's URL.
