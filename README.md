# rugen stage template

Click **Use this template** on GitHub, name the repository after your stage,
clone it, and you have a repository the [rugen stage catalogue](https://rugen.dev/stages)
can read. Learn the format with the tutorial
[Your first stage](https://rugen.dev/docs/creating/first-stage/).

## What goes where

```
MyStage/                the stage folder: rename it and the files in it
  MyStage.def           camera, bounds, player positions, [BGDef] and the layers
  MyStage.sff           sprites (binary, add from Fighter Factory)
rugen.json              catalogue manifest: name, author, summary, screenshots
screenshots/            pictures for the catalogue page (first one = card)
LICENSE                 keep MIT or CC-BY-NC (see below)
.github/workflows/      convert on push, zip + release on tag
```

Keep the stage in one folder with a single `.def` (a `.mp3`/`.ogg` for the
music can sit next to it). The workflows find that folder on their own.

**Arena stages.** A `.def` with a `[Platform]` or `[Arena]` section (rugen's
extensions: raised platforms, Smash-style stocks and blast zones) is flagged
*arena* in the catalogue and offered in the arena modes.

## Fighter Factory users

Keep editing in Fighter Factory: open `MyStage/MyStage.def`, work as usual,
save. The stage folder *is* the repository folder, so commit it as it is with
Git or GitHub Desktop. `.sff` is binary: commit it, do not rebase or rewrite
history to "clean" it.

## Check on every push

`.github/workflows/verify.yml` downloads the Linux release of rugen and runs
`rugen convert --stage` on the folder, which fails when the `.def` or its
sprites cannot be read. Until the sprites are in, the step is
`continue-on-error`; remove that line once `MyStage.sff` exists. Stages have
no fairness verdict.

## Release

```sh
git tag v1.0
git push --tags
```

`release.yml` zips the stage folder as `<repo>-v1.0.zip` and attaches it to a
GitHub release. Players drop the zip into rugen's `stages/`; the catalogue
links to the release asset (nothing is re-hosted). Every new tag updates the
catalogue page automatically within a few hours.

## License

Edit `LICENSE` and keep one option:

- **MIT**: anyone can reuse, remix and redistribute the stage, including in
  commercial projects, as long as your notice stays.
- **CC-BY-NC 4.0**: credit required, no commercial use. The usual choice for
  fan-made content built on someone else's art.

The catalogue requires a LICENSE file and shows the SPDX id it detects
(`MIT`, `CC-BY-NC-4.0`, ...). Only art you have the rights to.

## What the catalogue reads

| source | used for |
|---|---|
| `README.md` | rendered on the stage page (images rewritten to raw URLs) |
| `LICENSE` | the license badge |
| latest release, first `.zip` asset | download button and tag; the `.def` inside sets the arena flag |
| `screenshots/` and README images | pictures (first = card) |
| `rugen.json` | name, author, summary, screenshot order |

`rugen.json` example:

```json
{
  "kind": "stage",
  "name": "My Stage",
  "author": "you",
  "summary": "One sentence about the stage.",
  "screenshots": ["screenshots/wide.png"]
}
```

When the repository has a LICENSE and a release, submit it at
<https://rugen.dev/stages/submit>. A moderator approves and the page goes live.
