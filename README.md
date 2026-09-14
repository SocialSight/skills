# SocialSight Skills

[![Version](https://img.shields.io/badge/version-0.1.0-green.svg)](./VERSION)
[![Skills](https://img.shields.io/badge/skills-3-blueviolet.svg)](#skills)

Agent skills for image and video generation through the [SocialSight](https://socialsight.ai) MCP server. One set of `SKILL.md` files for Claude, a Google ADK agent, and any other client that speaks MCP. There is no CLI variant.

## Install

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

Copy the three directories under `skills/` into the host skills path, or load them from this repo:

| Client | Path |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| ADK / other | `~/.agents/skills/` or `load_skills_from_dir("skills")` |

```bash
cp -R skills/media-refs skills/video-generation skills/photo-modes ~/.claude/skills/
```

More options in [INSTALL.md](./INSTALL.md). Agent-driven install (paste into your agent): [INSTALL_FOR_AGENTS.md](./INSTALL_FOR_AGENTS.md).

## Skills

| Skill | Invoke | Description |
|---|---|---|
| [`media-refs`](./skills/media-refs) | `/socialsight:media-refs` | Turn URLs, local files, and completed jobs into `media_id` values for `params.medias`. |
| [`video-generation`](./skills/video-generation) | `/socialsight:video-generation` | Discover live model constraints, quote credits, submit `generate_video`, report the `job_id`, and stop. |
| [`photo-modes`](./skills/photo-modes) | `/socialsight:photo-modes` | Write a full photographic prompt for `generate_image` — studio, lifestyle, close-up, moodboard, hero, editorial. |

They chain: import with `media-refs`, then generate a still (`photo-modes`) or a clip (`video-generation`). The catalog is **not** duplicated here. Agents query `models_explore` at runtime so values cannot drift.

### Photo modes

| Mode | What it's for |
|---|---|
| Studio product shot | Product on a controlled sweep / catalog background |
| Lifestyle scene | Product in a real environment |
| Close-up with hands | Tight crop with hands or partial face |
| Moodboard | Vertical pin, art-direction collage |
| Hero banner | Wide site / email / campaign header |
| Editorial | Fashion / magazine still |

## Quick reference

| What you want | Skill | Note |
|---|---|---|
| Import a URL or local file | `media-refs` | `params.medias[].value` is a `media_id` or completed `job_id`, never a URL |
| Chain a following clip from the last frame | `media-refs` | Use MediaItem `last_frame_media_id` as `start_image` |
| Make a video | `video-generation` | Query `models_explore` first; never send schema defaults |
| Quote credits before spending | `video-generation` / `photo-modes` | `get_cost: true` returns `{ credits, model_id, job_type }` |
| Studio / lifestyle / editorial still | `photo-modes` | Prompts are written in full — no backend enhancer |
| After submit | either generation skill | Report `job_id` and stop. Do not call `job_status` |

## Not in this release

There is no `model-catalog` skill. The catalog is queryable via `models_explore`; a markdown copy would drift.

There is no `shot-planning` skill yet. SEEDANCE_2_5 accepts up to 30 seconds, so most deliverables fit in a single generation and grouping / join-frame logic goes unused. **That is the next skill to add** if longer deliverables or short-duration models become common.
