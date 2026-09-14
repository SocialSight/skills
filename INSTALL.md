# Install SocialSight Skills

Three skills ship in this repo. They are one body of instructions for any client that talks to the SocialSight MCP server — Claude, a Google ADK agent, Cursor, Codex. There is no CLI variant.

- **`media-refs`** — turn URLs, local files, and completed jobs into `media_id` values for `params.medias`
- **`video-generation`** — discover live model constraints, quote credits, submit `generate_video`, and stop
- **`photo-modes`** — write full photographic prompts for `generate_image` (no backend prompt enhancer)

They chain: import media with `media-refs`, then generate video or a still. Photo prompts always go through `generate_image` with catalog values.

## Prerequisites

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

## Option 1 — copy into the agent skills directory

From a clone of this repository, copy each directory under `skills/` into the host's skills path:

| Client | Path |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| Codex / other agents | `~/.agents/skills/` |

```bash
git clone <this-repo> socialsight-skills
cd socialsight-skills
cp -R skills/media-refs skills/video-generation skills/photo-modes ~/.claude/skills/
```

Each destination must contain a `SKILL.md` (for example `~/.claude/skills/media-refs/SKILL.md`).

## Option 2 — `npx skills` (cross-agent)

Requires Node.js. Installs every skill this repo publishes:

```bash
npx skills add <this-repo>
```

The `skills` CLI auto-detects the host agent and writes each skill to the right path.

## Option 3 — `gh skill install`

GitHub CLI v2.90+:

```bash
gh skill install <this-repo>
```

## Option 4 — Claude Code marketplace

Inside Claude Code, with this repository as the marketplace source:

```
/plugin marketplace add <this-repo>
/plugin install socialsight@socialsight
```

Pulls `.claude-plugin/marketplace.json` and registers `/socialsight:media-refs`, `/socialsight:video-generation`, and `/socialsight:photo-modes`.

## Option 5 — Google ADK

Point the agent at this repo's `skills/` directory. Every immediate subdirectory that contains a `SKILL.md` is a skill:

```python
from google.adk.skills import load_skills_from_dir
from google.adk.tools.skill_toolset import SkillToolset

skills = load_skills_from_dir("skills")  # list of three Skill objects
toolset = SkillToolset(skills=skills)
```

`load_skills_from_dir("skills")` must return `media-refs`, `video-generation`, and `photo-modes`. Connecting the SocialSight MCP server is still required; the skills only describe how to call it.

## Verify

In your agent, ask:

> "Import this image URL and generate a studio product shot."

The agent should load `media-refs` then `photo-modes` (or `video-generation` for a clip), call SocialSight MCP tools, and report a `job_id` without polling.

## Updating

| Method | Update command |
|---|---|
| Copy | re-copy the three directories from a fresh clone |
| `npx skills` | re-run `npx skills add ...` |
| `gh skill install` | `gh skill update <this-repo>` |
| Claude Code marketplace | `/plugin update socialsight@socialsight` |
| ADK | pull the repo; `load_skills_from_dir("skills")` reads from disk |
