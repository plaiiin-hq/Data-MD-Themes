# plaiiin Slide Themes

The presentation themes of [plaiiin MD](https://plaiiin.com/plaiiin-md). The app reads this repository directly: it fetches `catalog.json` to see which themes exist and at which version, then a theme's colours and thumbnail, and its photo and backgrounds the first time a document uses it. When a theme's `version` goes up, every app fetches it again.

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
| Porcelain | Soft | light | left |
| Alabaster | Editorial | light | left |

Each theme sets the title slide on its photo and each section's divider on a veiled copy of it. Content slides take the theme's backgrounds in turn — quieter pictures with room for text — under a veil of the page; a theme with none keeps a plain page.

## Layout

```
catalog.json                 which themes, at which version
themes/<id>/theme.json       colours, look, where the title sits
themes/<id>/photo.jpg        the photo, 16:9
themes/<id>/thumbnail.jpg    480 px wide, for the picker
themes/<id>/background-N.jpg any number, 16:9, behind content slides in turn
themes/<id>/title-N.jpg      optional, other pictures for the title slide
themes/<id>/divider-N.jpg    optional, pictures for the dividers
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
| `look` | `bold`, `soft`, `editorial` or `spotlight`: the look the theme started from. A theme without `design` draws this look as the app's version has it. |
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
| `photo.titles`, `photo.dividers` | File names, optional: more pictures for the title slide and the dividers than the one photo. A document picks which per slide (right-click it ▸ Background); dividers otherwise take theirs in turn. None: the photo, as before. |
| `rules` | `false`, optional: no horizontal rules after the title slide (Editorial's header rule and divider frame). |
| `design` | Optional: the theme's whole design — its faces and everything its title slide, dividers, header and footer draw — drawn as written in every version of the app. What the Themes window writes. See [The design](#the-design). |
| `type`, `masters` | Optional, the older form: changes laid over the look by element `id`. Still read; the Themes window writes `design` instead on the next change. |

### The design

A design is data: [slide designs](https://github.com/plaiiin-hq/Lib-Swift-DiagramKit/blob/main/docs/slide-designs.md) describes it, and each of the 4 looks is one (`Sources/MarkdownSlidesUI/Looks/`). A theme with `design` holds its own in full — `type` and the `title`, `section` and `content` masters — so it draws the same whatever later versions do to the looks. The Themes window starts it from the theme's look and writes it on every change; **Design ▸ Start Over from** a look replaces it with that look's.

```json
"design": {
  "type": { "display": { "design": "rounded", "weight": "bold" }, "label": { "weight": "semibold" } },
  "masters": { "title": { "elements": [ … ] }, "section": { … }, "content": { … } }
}
```

A theme without `design` is its look; the older form laid changes over it by element `id` under `type` and `masters` (`{ "id": "card", "radius": 24 }` changes the card, `"remove": true` takes it out), and is still read. A `theme.json` whose design does not hold together is not read, and the app says what is wrong.

The content's boxes and tables go under `masters.content.boxes` (in `design`): a callout's, quote's or code block's bar, fill, border and corners; a table's header, stripes, row and column lines, border, and highlighted rows (cells bold throughout, like a total). See [boxes and tables](https://github.com/plaiiin-hq/Lib-Swift-DiagramKit/blob/main/docs/slide-designs.md#boxes-and-tables).

## Your own themes

Any folder on the web laid out like this one is a theme source: a fork of this repository, or a repository of your own. Add it in MD under Settings → Slides → Theme Sources, as the repository's GitHub page or as the folder's URL.

**Making one in the app.** In the Themes window, switch to **Presentation** and choose **New Theme Repository…**: it makes a folder laid out like this one, with your source's name and `home`, and starts it with a copy of the theme you had selected. Published themes are read-only there; **Duplicate** copies one into your repository to change. Every change is saved as you make it, and the app raises the theme's `version` (in `theme.json` and `catalog.json`) once per editing. Commit and push to publish.

**Trying one out.** A folder on your Mac is a theme source too: Settings → Slides → Theme Sources → **Add Folder…**. It is read where it is and every open document in one of its themes redraws when a file in it changes, so an edit made in any editor shows at once, without a push or a version raised. A `theme.json` that does not read is listed there with what is wrong in it.

**Sharing a document.** A document set in a theme from your source carries its `home`. A reader who has your source sees your theme; one who has not sees plaiiin's theme in the same look, and a bar offering to add your source. A theme from a folder with no `home` is marked "only on this Mac" in the theme menu: others will see the stand-in.

## Tables and shadows

Each theme draws tables in one style, under `design.masters.content.boxes.table`: **Ledger** (Pavilion, Tower, Graphite), **Zebra** (Riviera, Dune), **Tiles** (Botanic, Sakura, Skyline), **Blueprint** (Prism, Daybreak) and **Glass** (Aurora, Orbit). Colours come from the theme (accent, text), so a style is the same shaping in every theme. Soft themes also lift their quote, callout and code boxes and pictures with a drop shadow; Riviera and Dune lift the whole table; Aurora and Orbit set their cells in with an inner shadow. **Porcelain** is white cards on a very light page, lifted by shadows everywhere; **Alabaster** is the opposite, the same page with everything pressed in by inner shadows.
