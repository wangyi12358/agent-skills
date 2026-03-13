# agent-skills

一个用于管理自定义 agent skills 的仓库。

## 目录结构

```text
.
├── skills/
│   └── example-skill/
│       └── SKILL.md
├── templates/
│   └── SKILL.template.md
└── .gitignore
```

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
