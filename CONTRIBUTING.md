# Contributing

1. Edit one thing: `jsons/` for rules data, `Images/` for art. This is a data-only [Unciv](https://github.com/yairm210/Unciv) extension mod — JSON rules and PNG art, no code to build.
2. Run it in the game, following [Test your change](#test-your-change).
3. Open one pull request for that change and link the issue it fixes. Smaller pull requests get merged faster.

## Match Brave New World

**Cite your source in the pull request when you change rules data.** A link to the relevant [Civilization Wiki](https://civilization.fandom.com/wiki/Civilization_V) page — [Mughal Fort](https://civilization.fandom.com/wiki/Mughal_Fort_(Civ5)), [Cultural Exchange](https://civilization.fandom.com/wiki/Cultural_Exchange_(Civ5)), whatever you touched — is enough. It makes review fast and shows whether a difference is a bug or intentional.

The goal is to reproduce [Civilization V: Brave New World](https://civilization.fandom.com/wiki/Civilization_V:_Brave_New_World) as closely as Unciv allows. Check these before you override an object or file a bug:

- **Copy the whole object when you override one.** Unciv replaces objects wholesale, it does not merge fields: every field you leave out of a building, unit or nation reverts to Unciv's default rather than the base ruleset's value. Start from the whole object in the [Gods & Kings base ruleset](https://github.com/yairm210/Unciv/tree/master/android/assets/jsons) and edit that.
- **Expect this mod to differ from *Civ V - Gods & Kings*.** It sits on top of that ruleset, but BNW is a different ruleset, so an object that differs from the version it overrides is usually deliberate. The [Fall 2013 patch](https://civilization.fandom.com/wiki/Fall_2013_patch_(Civ5)) page is useful for the late changes.
- **Mark missing mechanics `// TODO:` in the JSON.** Some BNW mechanics have no Unciv equivalent. Keep `TODO` out of any `Comment [...]` unique — those render in the Civilopedia and players see them.

## Find the right file

| Path | What it is |
| --- | --- |
| `jsons/` | The mod. Rulesets, plus `translations/`. |
| `Images/` | Source art. Unciv packs this into the atlas. |
| `game.atlas`, `game.png` | **Generated.** Don't hand-edit — see [Add art](#add-art). |
| `jsons/ModOptions.json` | Manifest: base-ruleset requirement, and the `*ToRemove` lists. |
| `CREDITS.md` | Attribution. Required by most of the icon licenses. |

## Edit the JSON

Keep the `//` comments in any file you touch. The `.json` files are JSONC, and those comments record intent — often the only explanation of why something is the way it is.

Install the [VS Code extension](https://marketplace.visualstudio.com/items?itemName=robloach.unciv) for Unciv JSON; `.vscode/extensions.json` already recommends it. [`.editorconfig`](.editorconfig) sets tabs, UTF-8, LF, and a trailing newline, and most editors pick this up automatically.

## Test your change

Your edit is saved; now run it. Allow 10 to 15 minutes the first time, for the install step.

1. Install the mod from the in-game Mods menu so Unciv creates its folder, then replace that copy with your working copy. On desktop, `mods/` sits next to `Unciv.jar`.
2. Start a new game with base ruleset *Civ V - Gods & Kings* and the *Civ V Brave New World* extension enabled.
3. Run Unciv's linter with `java -jar Unciv.jar mod-ci`. CI only lints `main`, so run it yourself before you open the pull request.

A new map loads and your edited building, unit or text shows in the Civilopedia: Unciv parsed the ruleset.

A large number of `WarningOptionsOnly:` lines from the linter are expected. The cause: this is an extension mod, so the linter cannot resolve names that live in the base ruleset (`Factory`, `University`, `Mounted`, and so on) when it checks the mod on its own. Read the lines starting `Error:` instead.

## Add art

1. Put source PNGs in the matching `Images/` subfolder — `Images/BuildingIcons/`, `Images/UnitIcons/`, `Images/TileSets/HexaRealm/Units/`, and so on. The filename must exactly match the object's `name` in the JSON.
2. Add yourself to [`CREDITS.md`](CREDITS.md), and credit the original artist with a link.

`game.atlas` and `game.png` are build artifacts that happen to be committed, because Unciv's mod browser serves the mod straight from this repository. The desktop client regenerates them, and so does CI — see Unciv's [texture atlas notes](https://github.com/yairm210/Unciv/blob/master/docs/Modders/Images-and-Audio.md#images-and-the-texture-atlas). Adding art means a large atlas diff; that's normal.

## Update a translation

Edit `jsons/translations/<Language>.properties` directly. Translate the right-hand side only — never the text inside `[square brackets]`, which Unciv substitutes by matching the English name:

```properties
Tutorial Task: [You Have a [Great Writer]!] = <your language>: [Great Writer]
```

- Exact case matters, in both the filename and the keys left of the `=`.
- Leave `# Requires translation!` markers in place until the line below them is filled in — they're how the next translator finds the gaps.
- Translate the reworded value in `English.properties`, not the key. That file holds deliberate BNW rewordings rather than translations, such as `Christianity = Catholicism` and `Theatre = Zoo`; translating the key keeps the base-game wording, even where Unciv already translates the original word. Unciv's guide covers this under "More about translating".

### Add missing entries

Don't type missing entries by hand. The language itself has to already exist in Unciv — mods can't add new ones.

1. Install the mod, and make sure its translation file has at least one valid entry.
2. Run **Options → Advanced → Generate translation files** in the desktop client and select this mod. See Unciv's [translation generation guide](https://github.com/yairm210/Unciv/blob/master/docs/Translating/Translation-mods.md).

The language file now holds every missing key under a `# Requires translation!` marker, ready to fill in.

## Ask about a rule

Join the [Discord channel](https://discord.com/channels/586194543280390151/1055580642806603866) and ask questions there too!
