# AGENTS.md

这个文件为在本仓库中工作的 AI 编码代理（Claude Code、Cursor、Copilot 等）提供指导。

## 仓库概览

这是一个为 Claude.ai 和 Claude Code 提供、用于操作 Vercel 部署的技能集合。技能是打包好的指令和脚本，用来扩展 Claude 的能力。

## 创建新 Skill

### 目录结构

```
skills/
  {skill-name}/           # 目录名使用 kebab-case
    SKILL.md              # 必需：skill 定义
    scripts/              # 必需：可执行脚本
      {script-name}.sh    # Bash 脚本（优先推荐）
  {skill-name}.zip        # 必需：用于分发的打包文件
```

### 命名规范

- **Skill 目录**：`kebab-case`（例如 `vercel-deploy`、`log-monitor`）
- **SKILL.md**：必须全大写，且文件名必须是这个精确字符串
- **脚本**：`kebab-case.sh`（例如 `deploy.sh`、`fetch-logs.sh`）
- **Zip 文件**：文件名必须与目录名完全一致：`{skill-name}.zip`

### SKILL.md 格式

```markdown
---
name: {skill-name}
description: {用一句话描述何时使用这个 skill。包含触发短语，比如 "Deploy my app"、"Check logs" 等。}
---

# {Skill Title}

{简要描述这个 skill 做什么。}

## How It Works

{用编号列表说明 skill 的工作流程}

## Usage

```bash
bash /mnt/skills/user/{skill-name}/scripts/{script}.sh [args]
```

**Arguments:**
- `arg1` - 参数说明（默认值为 X）

**Examples:**
{展示 2–3 个常见使用方式}

## Output

{展示用户会看到的示例输出}

## Present Results to User

{说明 Claude 在向用户展示结果时应该用的格式模板}

## Troubleshooting

{常见问题与解决方案，尤其是网络/权限相关错误}
```

### 上下文效率最佳实践

Skills 是按需加载的——启动时只会加载 skill 的名称和描述。完整的 `SKILL.md` 只有在代理认为这个 skill 相关时才会被加载进上下文。为了最小化上下文占用：

- **保持 SKILL.md 小于 500 行** —— 更详细的参考资料放在单独文件中
- **写得足够具体** —— 帮助代理准确判断何时激活这个 skill
- **使用渐进披露** —— 通过引用其他文件，让代理只在需要时再去读取
- **优先使用脚本而非内联代码** —— 脚本执行本身不占用上下文（只有输出会占用）
- **文件引用只支持一层深度** —— 从 SKILL.md 直接引用的文件才会被访问

### 脚本要求

- 使用 `#!/bin/bash` shebang
- 使用 `set -e` 以便在出错时快速失败
- 使用 `echo "Message" >&2` 将状态信息写到 stderr
- 以 JSON 形式将机器可读输出写到 stdout
- 使用 trap 做临时文件清理
- 在说明中用 `/mnt/skills/user/{skill-name}/scripts/{script}.sh` 作为脚本路径

### 创建 Zip 包

在创建或更新 skill 之后：

```bash
cd skills
zip -r {skill-name}.zip {skill-name}/
```

### 终端用户安装

需要在文档中给出两种安装方式：

**Claude Code:**
```bash
cp -r skills/{skill-name} ~/.claude/skills/
```

**claude.ai:**
在项目知识中添加该 skill，或者直接把 SKILL.md 的内容粘贴进对话里。

如果 skill 需要网络访问，提示用户在 `claude.ai/settings/capabilities` 中添加所需的域名白名单。