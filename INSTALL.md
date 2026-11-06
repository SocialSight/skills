# Install SocialSight Skills

Four skills ship in this repo. They are one body of instructions for any client that talks to the SocialSight MCP server — Claude, a Google ADK agent, Cursor, Codex. There is no CLI variant.

- **`media-refs`** — turn URLs, local files, and completed jobs into `media_id` values for `params.medias` (external clients; some hosts do not load it)
- **`video-generation`** — pick the video model and aspect first, make the cast sheet or key frame, then write the video prompt
- **`image-generation`** — write full image prompts for stills, edits, cast sheets, and key frames (no backend prompt enhancer)
- **`marketing-studio`** — pick the ad mode (UGC, TV spot, explainer, app demo, …) before writing prompts

They chain: import media with `media-refs`, make the still a video starts from with `image-generation`, then the clip with `video-generation`.

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
cp -R skills/media-refs skills/video-generation skills/image-generation skills/marketing-studio ~/.claude/skills/
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

Pulls `.claude-plugin/marketplace.json` and registers `/socialsight:media-refs`, `/socialsight:video-generation`, `/socialsight:image-generation`, and `/socialsight:marketing-studio`.

## Option 5 — Google ADK

Point the agent at this repo's `skills/` directory. Every immediate subdirectory that contains a `SKILL.md` is a skill:

```python
from google.adk.skills import load_skills_from_dir
from google.adk.tools.skill_toolset import SkillToolset

skills = load_skills_from_dir("skills")  # list of Skill objects
toolset = SkillToolset(skills=skills)
```

`load_skills_from_dir("skills")` must return `media-refs`, `video-generation`, `image-generation`, and `marketing-studio`. Connecting the SocialSight MCP server is still required; the skills only describe how to call it.

## Verify

In your agent, ask:

> "Import this image URL and generate a studio product shot."

The agent should load `media-refs` then `image-generation` (or `video-generation` for a clip — pick the video model and aspect, then the image, then the clip), call SocialSight MCP tools, and report a `job_id` without polling.

## Updating

| Method | Update command |
|---|---|
| Copy | re-copy the four directories from a fresh clone, then delete any leftover `photo-modes` (`rm -rf <path>/photo-modes`) so the old skill cannot be selected |
| `npx skills` | re-run `npx skills add ...`; remove leftover `photo-modes` if still present |
| `gh skill install` | `gh skill update <this-repo>`; remove leftover `photo-modes` if still present |
| Claude Code marketplace | `/plugin update socialsight@socialsight` |
| ADK | pull the repo; `load_skills_from_dir("skills")` reads from disk; delete any leftover `photo-modes` dir |
