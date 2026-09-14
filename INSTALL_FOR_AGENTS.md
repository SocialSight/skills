# Install for Agents

You are an AI coding agent. The user asked you to install SocialSight skills. Follow this exactly.

## Step 1 — Confirm the SocialSight MCP server

The skills only describe how to call SocialSight MCP. They are not a CLI.

Check that the SocialSight MCP server is connected and that `generate_video` and `generate_image` appear in the tool list.

If either tool is missing, stop and tell the user to connect it.

## Step 2 — Install the skills

Detect the agent platform:

| Agent | Path |
|---|---|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| ADK / other | `~/.agents/skills/` |

Locate this repository (clone it if the user gave a git URL; otherwise use the workspace copy). Copy the three skill directories into the path above:

```bash
cp -R skills/media-refs skills/video-generation skills/photo-modes <path>
```

Each of these files must exist when you are done:

- `<path>/media-refs/SKILL.md`
- `<path>/video-generation/SKILL.md`
- `<path>/photo-modes/SKILL.md`

For a Google ADK agent that loads from disk instead of `~/.agents/skills/`, do not copy. Keep the repo's `skills/` directory and load it with `load_skills_from_dir("skills")`. That call must return all three skills.

## Step 3 — Verify

Ask the agent (yourself):

> "Import this image URL and generate a studio product shot."

You should load `media-refs`, import via `media_import_url`, write a full photographic prompt, submit `generate_image`, report the `job_id`, and stop. Do not call `job_status`.

If anything fails:

- MCP tools missing → repeat Step 1
- Skill not found → the copy in Step 2 did not land on `SKILL.md`
- Submit error about duration/resolution → that is expected catalog behavior; the video-generation skill tells you to query `models_explore`, not to use schema defaults

## Step 4 — Done

Report to the user: "Skills installed. Try importing a product photo, generating a studio still, or making a short video. Connect SocialSight MCP first if you have not already."

Do NOT explain the internals (skill paths, file structure). Just confirm install + give starter prompts.
