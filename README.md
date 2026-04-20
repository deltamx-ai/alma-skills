# alma-skills

Exported Alma skills for reuse in other projects.

## Directory layout

```text
alma-skills/
├── README.md
├── custom/   # user-created or locally installed skills from ~/.config/alma/skills
└── bundled/  # bundled Alma skills exported from /opt/Alma/resources/bundled-skills
```

## Source paths

- Custom skills source: `~/.config/alma/skills`
- Bundled skills source: `/opt/Alma/resources/bundled-skills`

## Export time snapshot

- Custom skill directories: 0
- Bundled skill directories: 30

## Notes

- Most skills are organized as one directory per skill.
- The main definition file is usually `SKILL.md`.
- Some skills may also include helper scripts or assets.
- Bundled skills may depend on Alma-specific commands, tools, or runtime behavior.
- If another project wants to reuse these skills directly, it may need to adapt command paths, tool names, or environment assumptions.

## Refresh / sync

Re-export from the live Alma installation with:

```bash
mkdir -p ~/workspace/ai/alma-skills/{custom,bundled}
rsync -av --delete ~/.config/alma/skills/ ~/workspace/ai/alma-skills/custom/
rsync -av --delete /opt/Alma/resources/bundled-skills/ ~/workspace/ai/alma-skills/bundled/
```

## Quick inspection

List exported skills:

```bash
find ~/workspace/ai/alma-skills/custom -mindepth 1 -maxdepth 1 -type d | sort
find ~/workspace/ai/alma-skills/bundled -mindepth 1 -maxdepth 1 -type d | sort
```

Preview all skill definition files:

```bash
find ~/workspace/ai/alma-skills -name SKILL.md | sort
```
