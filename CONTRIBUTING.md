# Contributing

Thanks for helping out! This is a data-only [Unciv](https://github.com/yairm210/Unciv) extension mod — there is no code to build. Everything is JSON rules and PNG art.

Smaller pull requests get merged faster. One change per pull request, and link the issue it fixes.

If you want to talk something through first, there's a [Discord channel](https://discord.com/channels/586194543280390151/1055580642806603866).

## Match Brave New World

The goal is to reproduce [Civilization V: Brave New World](https://civilization.fandom.com/wiki/Civilization_V:_Brave_New_World) as closely as Unciv allows.

**When changing rules data, cite your source in the pull request.** A link to the relevant [Civilization Wiki](https://civilization.fandom.com/wiki/Civilization_V) page — [Mughal Fort](https://civilization.fandom.com/wiki/Mughal_Fort_(Civ5)), [Cultural Exchange](https://civilization.fandom.com/wiki/Cultural_Exchange_(Civ5)), whatever you touched — is enough. It makes review fast and settles "is this a bug or is this intentional" before it becomes an argument.

Two things worth knowing before you file a bug:

- **This mod sits on top of *Civ V - Gods & Kings*, but BNW is a different ruleset.** Where an object here differs from the Gods & Kings version it overrides, that is usually deliberate. The [Fall 2013 patch](https://civilization.fandom.com/wiki/Fall_2013_patch_(Civ5)) page is useful for the late changes.
- **Unciv replaces objects wholesale, it does not merge fields.** If you override a building, unit or nation, every field you leave out reverts to Unciv's default rather than keeping the base ruleset's value. When overriding something, copy the whole object from the [Gods & Kings base ruleset](https://github.com/yairm210/Unciv/tree/master/android/assets/jsons) and edit from there.

Some BNW mechanics have no Unciv equivalent. Those are marked `// TODO:` in the JSON. Please don't put `TODO` inside a `Comment [...]` unique — those render in the Civilopedia and players see them.

## Layout

| Path | What it is |
| --- | --- |
| `jsons/` | The mod. Rulesets, plus `translations/`. |
| `Images/` | Source art. Unciv packs this into the atlas. |
| `game.atlas`, `game.png` | **Generated.** Don't hand-edit — see [Art](#art). |
| `jsons/ModOptions.json` | Manifest: base-ruleset requirement, and the `*ToRemove` lists. |
| `CREDITS.md` | Attribution. Required by most of the icon licenses. |

## JSON style

The `.json` files are JSONC — `//` comments are allowed and widely used to record intent. Keep them; they are often the only explanation of why something is the way it is.

[`.editorconfig`](.editorconfig) sets tabs, UTF-8, LF, and a trailing newline. Most editors pick this up automatically. There is a [VS Code extension](https://marketplace.visualstudio.com/items?itemName=robloach.unciv) for Unciv JSON — `.vscode/extensions.json` already recommends it.

## Testing your change

1. Install the mod from the in-game Mods menu so Unciv creates its folder, then replace that copy with your working copy. On desktop, `mods/` sits next to `Unciv.jar`.
2. Start a new game with base ruleset *Civ V - Gods & Kings* and the *Civ V Brave New World* extension enabled.

Unciv's own linter runs in CI on every push. You can run it yourself with `java -jar Unciv.jar mod-ci`.

A large number of `WarningOptionsOnly:` lines are expected and are **not** your fault — this is an extension mod, so the linter cannot resolve names that live in the base ruleset (`Factory`, `University`, `Mounted`, and so on) when it checks the mod on its own. Look for lines starting `Error:` instead.

## Art

Put source PNGs in the matching `Images/` subfolder — `Images/BuildingIcons/`, `Images/UnitIcons/`, `Images/TileSets/HexaRealm/Units/`, and so on. The filename must exactly match the object's `name` in the JSON.

`game.atlas` and `game.png` are build artifacts that happen to be committed, because Unciv's mod browser serves the mod straight from this repository. The desktop client regenerates them, and so does CI — see Unciv's [texture atlas notes](https://github.com/yairm210/Unciv/blob/master/docs/Modders/Images-and-Audio.md#images-and-the-texture-atlas). Adding art means a large atlas diff; that's normal.

Add yourself to [`CREDITS.md`](CREDITS.md), and credit the original artist with a link.

## Translations

Translations live in `jsons/translations/<Language>.properties`. Exact case matters, in both the filename and the keys left of the `=`.

To update an existing language, edit its file directly. Translate the right-hand side only — never the text inside `[square brackets]`, which Unciv substitutes by matching the English name:

```properties
Tutorial Task: [You Have a [Great Writer]!] = <your language>: [Great Writer]
```

Leave `# Requires translation!` markers in place until the line below them is filled in — they're how the next translator finds the gaps.

**To add missing entries**, don't type them by hand. Install the mod, make sure its translation file has at least one valid entry, then run **Options → Advanced → Generate translation files** in the desktop client and select this mod. See Unciv's [translation generation guide](https://github.com/yairm210/Unciv/blob/master/docs/Translating/Translation-mods.md). The language itself has to already exist in Unciv — mods can't add new ones.

One catch specific to this mod: `English.properties` holds deliberate BNW rewordings rather than translations, such as `Christianity = Catholicism` and `Theatre = Zoo`. Even where Unciv already translates the original word, **translate the reworded value**, not the key — otherwise your language keeps the base-game wording. Unciv's guide covers this under "More about translating".
