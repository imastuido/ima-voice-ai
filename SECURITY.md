# Security Disclosure: ima-voice-ai

## Purpose

This document explains endpoint usage, credential flow, and local data behavior for `ima-voice-ai`.

## Network Endpoints

| Domain | Used For | Trigger |
|---|---|---|
| `api.imastudio.com` | Product list, task create, task detail polling | All requests |

This skill does **not** use a secondary upload domain because it does not upload images/files.

## Credential Flow

| Credential | Where Sent | Why |
|---|---|---|
| `IMA_API_KEY` | `api.imastudio.com` | Open API auth (`Authorization: Bearer ...`) |

No credential is sent to secondary upload domains in this skill.

## Model Scope Enforcement

The script hard-limits exposed model IDs to:

- `sonic`
- `GenBGM`
- `GenSong`

Any other model ID is rejected locally.

## Cross-Skill Reads

This skill is self-contained for core API execution.
If `ima-knowledge-ai` is installed, the agent may optionally read:

- `~/.openclaw/skills/ima-knowledge-ai/references/*`

for workflow decomposition and model-selection guidance only.

## Local Data

| Path | Content | Retention |
|---|---|---|
| `~/.openclaw/memory/ima_prefs.json` | Per-user model preference cache | Until manually removed |
| `~/.openclaw/logs/ima_skills/` | Operational logs | Auto-cleaned by script after 7 days |

No secret is written into repository files.
