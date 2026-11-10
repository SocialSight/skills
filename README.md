# SocialSight Skills

[![Version](https://img.shields.io/badge/version-0.1.0-green.svg)](./VERSION)
[![Skills](https://img.shields.io/badge/skills-4-blueviolet.svg)](#skills)

Agent skills for image and video generation through the [SocialSight](https://socialsight.ai) MCP server. One set of `SKILL.md` files for Claude, a Google ADK agent, and any other client that speaks MCP. There is no CLI variant.

## Install

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

Copy the four directories under `skills/` into the host skills path, or load them from this repo:

| Client | Path |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| ADK / other | `~/.agents/skills/` or `load_skills_from_dir("skills")` |

```bash
cp -R skills/media-refs skills/video-generation skills/image-generation skills/marketing-studio ~/.claude/skills/
```

More options in [INSTALL.md](./INSTALL.md). Agent-driven install (paste into your agent): [INSTALL_FOR_AGENTS.md](./INSTALL_FOR_AGENTS.md).

## Skills

| Skill | Invoke | Description |
|---|---|---|
| [`media-refs`](./skills/media-refs) | `/socialsight:media-refs` | Turn URLs, local files, and completed jobs into `media_id` values for `params.medias`. |
| [`video-generation`](./skills/video-generation) | `/socialsight:video-generation` | Pick the video model and aspect, make the storyboard (4–6 panels), then write the video prompt. |
| [`image-generation`](./skills/image-generation) | `/socialsight:image-generation` | Write a full image prompt — studio / lifestyle / editorial stills, edits, storyboards, and cast sheets. |
| [`marketing-studio`](./skills/marketing-studio) | `/socialsight:marketing-studio` | Pick the ad mode (UGC, tutorial, unboxing, review, showcase, TV spot, try-on, conceptual, explainer, app demo) before the prompts. |

They chain: import with `media-refs`, pick the video model and aspect (`video-generation`), make the storyboard (`image-generation`), then the clip. The catalog is **not** duplicated here. Agents query `models_explore` at runtime so values cannot drift. `media-refs` stays in the repo for external clients; some hosts do not load it.

### Image treatments

| Mode | What it's for |
|---|---|
| Studio product shot | Product on a controlled sweep / catalog background |
| Lifestyle scene | Product in a real environment |
| Close-up with hands | Tight crop with hands or partial face |
| Moodboard | Vertical pin, art-direction collage |
| Hero banner | Wide site / email / campaign header |
| Editorial | Fashion / magazine still |
| Storyboard | 4–6 panels, one per beat — the preview every video starts from |
| Cast sheet | Character / product sheet only when the user asks for one |

## Quick reference

| What you want | Skill | Note |
|---|---|---|
| Import a URL or local file | `media-refs` | `params.medias[].value` is a `media_id` or completed `job_id`, never a URL |
| Chain a following clip from the last frame | `media-refs` | Use MediaItem `last_frame_media_id` as `start_image` |
| Make a video | `video-generation` | Pick video model + aspect via `models_explore`, then the storyboard, then the clip; never send schema defaults |
| Studio / lifestyle / editorial still, storyboard, cast sheet | `image-generation` | Prompts are written in full — no backend enhancer |
| Ad feel (UGC, TV spot, explainer, …) | `marketing-studio` | Pick one mode from the user's words |
| How a job is started and paid | host rule, else `video-generation/references/submit.md` | Some hosts save a plan and submit later |
