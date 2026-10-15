# plaiiin Slide Themes

The deck themes of [plaiiin MD](https://plaiiin.com/plaiiin-md). The app reads this repository directly: it fetches `catalog.json` to see which themes exist and at which version, then a theme's colours and thumbnail, and its photo and backgrounds the first time a deck uses it. When a theme's `version` goes up, every app fetches it again.

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
  "home": "https://github.com/plaiiin-hq/Data-SlideThemes",
  "themes": [ { "id": "riviera", "version": 1 } ] }
```

| Field | Meaning |
|---|---|
| `format` | `1`. |
| `name` | The source's name, shown above its themes. |
| `home` | Optional: where the source is published. A document set in one of its themes names it (`source=` in its `<!-- plaiiin … -->` line), so a reader without the source is offered to add it; and a folder on a Mac whose catalog names a home stands in for that published source there, as its working copy. |
| `themes` | Each theme's `id` and `version`. |

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
| `photo.blur` | Points, optional, default `24`: how far the background is blurred behind tables, callouts and code. `0` keeps it sharp. |
| `rules` | `false`, optional: no horizontal rules after the title slide (Editorial's header rule and divider frame). |
| `type`, `masters` | Optional: changes to the look itself — its faces, and anything its title slide, dividers, header and footer draw — written as only what differs. See [Changing the look](#changing-the-look). |

### Changing the look

A look is data: [slide designs](https://github.com/plaiiin-hq/Lib-Swift-DiagramKit/blob/main/docs/slide-designs.md) describes it, and each of the 4 is a file there (`Sources/MarkdownSlidesUI/Looks/`). A theme changes its look by naming the elements it changes by `id`:

```json
"masters": {
  "section": { "elements": [
    { "id": "card", "radius": 24, "fill": { "color": "section", "opacity": 0.1 } },
    { "id": "number", "remove": true }
  ] }
}
```

Soft with the section card's corners at 24 pt, a lighter tint and no number; everything else stays Soft's. A new `id` adds an element. A `theme.json` whose changes do not hold together is not read, and the app says what is wrong.

The content's boxes and tables go under `masters.content.boxes`: a callout's, quote's or code block's bar, fill, border and corners; a table's header, stripes, row and column lines, border, and highlighted rows (cells bold throughout, like a total). See [boxes and tables](https://github.com/plaiiin-hq/Lib-Swift-DiagramKit/blob/main/docs/slide-designs.md#boxes-and-tables).

## Your own themes

Any folder on the web laid out like this one is a theme source: a fork of this repository, or a repository of your own. Add it in MD under Settings → Slides → Theme Sources, as the repository's GitHub page or as the folder's URL.

**Making one in the app.** In the Themes window, switch to **Decks** and choose **New Theme Repository…**: it makes a folder laid out like this one, with your source's name and `home`, and starts it with a copy of the theme you had selected. Published themes are read-only there; **Duplicate** copies one into your repository to change. Every change is saved as you make it, and the app raises the theme's `version` (in `theme.json` and `catalog.json`) once per editing. Commit and push to publish.

**Trying one out.** A folder on your Mac is a theme source too: Settings → Slides → Theme Sources → **Add Folder…**. It is read where it is and every open deck in one of its themes redraws when a file in it changes, so an edit made in any editor shows at once, without a push or a version raised. A `theme.json` that does not read is listed there with what is wrong in it.

**Sharing a document.** A document set in a theme from your source carries its `home`. A reader who has your source sees your theme; one who has not sees plaiiin's theme in the same look, and a bar offering to add your source. A theme from a folder with no `home` is marked "only on this Mac" in the theme menu: others will see the stand-in.
