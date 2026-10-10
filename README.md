# Codex skills

A collection of reusable Codex skills. Each skill has its own directory under `skills/`, with a `SKILL.md` entrypoint and any supporting files beside it.

## Skills

- [`neuronize`](skills/neuronize/SKILL.md) turns technical sources into Obsidian-ready engineering notes focused on practical use in projects.
- [`adr-management`](skills/adr-management/SKILL.md) helps teams assess, create, and maintain Architecture Decision Records for consequential engineering decisions.

## Install a skill

Copy the skill directory into your personal Codex skills directory:

```sh
mkdir -p ~/.codex/skills
cp -R skills/<skill-name> ~/.codex/skills/
```

Start a new Codex session to load the skill, then invoke it with `$<skill-name>` or make a request that matches its description.

## Add another skill

Create `skills/<skill-name>/SKILL.md` with YAML frontmatter containing a matching `name` and a clear `description`. Put supporting documentation in that skill's `references/` directory when needed.

## License

[MIT](LICENSE)
