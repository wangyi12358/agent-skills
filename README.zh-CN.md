# agent-skills

一个用于管理自定义 agent skills 的仓库。

**语言:** [English](README.md)

## 目录结构

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

## 本仓库中的 Skills

| Skill | 路径 | 说明 |
| --- | --- | --- |
| **example-skill** | [`skills/example-skill/SKILL.md`](skills/example-skill/SKILL.md) | 最小示例：用途、何时使用、工作流与输出格式，作为新 skill 的起点。 |
| **honojs** | [`skills/honojs/SKILL.md`](skills/honojs/SKILL.md) | 基于 Hono 的 HTTP API：Zod 校验、类型化 `Env`、routes / services 分层；可运行片段见 [`example/`](skills/honojs/example/)。 |
| **next-antd-hono-rpc** | [`skills/next-antd-hono-rpc/SKILL.md`](skills/next-antd-hono-rpc/SKILL.md) | Next.js App Router 与 Hono 类型化 RPC 客户端（`hono/client` `hc`）；偏 Ant Design 的实践；示例见 [`example/`](skills/next-antd-hono-rpc/example/)。 |
| **init-ultra-next-stack** | [`skills/init-ultra-next-stack/SKILL.md`](skills/init-ultra-next-stack/SKILL.md) | 分步初始化：Next.js + Tailwind + shadcn/ui + Jotai + Biome + Lefthook，包管理器为 `pnpm`。 |

## 使用方式

1. 复制 `templates/SKILL.template.md`。
2. 在 `skills/` 下创建一个新目录，例如 `skills/code-review/`。
3. 将文件命名为 `SKILL.md`。
4. 在 `SKILL.md` 里定义这个 skill 的用途、触发条件、执行步骤和输出格式。

## 约定

- 一个 skill 对应一个目录。
- skill 主文件统一命名为 `SKILL.md`。
- 目录名建议使用 kebab-case，例如 `api-design`、`bug-triage`。
- 如果某个 skill 需要额外资源，可以放在该目录下，例如 `examples/`、`scripts/`、`assets/`。

## 快速开始

参考示例：`skills/example-skill/SKILL.md`
