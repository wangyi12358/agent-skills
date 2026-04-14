# agent-skills

A repository for managing custom agent skills.

**Languages:** [中文](README.zh-CN.md)

## Repository layout

```text
.
├── skills/
│   ├── example-skill/SKILL.md
│   ├── honojs/
│   ├── init-ultra-next-stack/
│   └── next-antd-hono-rpc/
├── templates/
│   └── SKILL.template.md
└── .gitignore
```

## Skills in this repository

| Skill | Path | Summary |
| --- | --- | --- |
| **example-skill** | [`skills/example-skill/SKILL.md`](skills/example-skill/SKILL.md) | Minimal template: purpose, when to use, workflow, and output format—use as a starting point for new skills. |
| **honojs** | [`skills/honojs/SKILL.md`](skills/honojs/SKILL.md) | Hono HTTP APIs with Zod validation, typed `Env`, and a routes / services split; runnable snippets under [`example/`](skills/honojs/example/). |
| **next-antd-hono-rpc** | [`skills/next-antd-hono-rpc/SKILL.md`](skills/next-antd-hono-rpc/SKILL.md) | Next.js App Router UI with a typed Hono RPC client (`hono/client` `hc`); Ant Design–friendly patterns; examples under [`example/`](skills/next-antd-hono-rpc/example/). |
| **init-ultra-next-stack** | [`skills/init-ultra-next-stack/SKILL.md`](skills/init-ultra-next-stack/SKILL.md) | Step-by-step scaffold: Next.js + Tailwind + shadcn/ui + Jotai + Biome + Lefthook, `pnpm` as the package manager. |

## How to use

1. Copy `templates/SKILL.template.md`.
2. Create a new directory under `skills/`, for example `skills/code-review/`.
3. Name the file `SKILL.md`.
4. In `SKILL.md`, define the skill’s purpose, triggers, steps, and output format.

## Conventions

- One skill maps to one directory.
- The main skill file is always named `SKILL.md`.
- Use kebab-case for directory names, for example `api-design`, `bug-triage`.
- If a skill needs extra resources, place them in that directory, for example `examples/`, `scripts/`, or `assets/`.

## Quick start

See the example: `skills/example-skill/SKILL.md`
