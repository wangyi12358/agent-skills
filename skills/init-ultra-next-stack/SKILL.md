---
name: init-ultra-next-stack
description: 自动执行“用 Next.js + Tailwind + shadcn/ui + Jotai + Biome + Lefthook（pnpm）初始化一个全新的前端工程”的标准流程；触发语包括“init-ultra-next-stack”、“初始化 ultra next stack”、“帮我创建一个带 Biome 和 Lefthook 的 Next.js 项目”等。
---

# init-ultra-next-stack

自动化初始化现代前端工程的标准操作流程 (SOP)。目标是构建一个极速、类型安全且易于扩展的前端架构，完全使用 Biome 和 Lefthook 替代传统的 ESLint、Prettier 和 Husky，并统一使用 `pnpm` 作为包管理器。

## How It Works

1. **创建基础项目 (Next.js + TypeScript + Tailwind)**  
   - 提示用户提供 `<project-name>`。  
   - 使用 `create-next-app` 创建项目，启用 App Router / `src` 目录 / `@/*` alias，并显式关闭 ESLint，选择 `pnpm`：

   ```bash
   npx create-next-app@latest <project-name> \
     --typescript \
     --tailwind \
     --app \
     --src-dir \
     --import-alias "@/*" \
     --eslint=false \
     --use-pnpm

   cd <project-name>
   ```

2. **安装 UI 组件与状态管理 (shadcn/ui + Jotai)**  
   - 初始化 shadcn/ui（默认 New York + Zinc，静默配置）：

   ```bash
   npx shadcn@latest init -d
   ```

   - 安装原子化状态管理库 Jotai：

   ```bash
   pnpm add jotai
   ```

3. **初始化 Ultracite（基于 Biome）**  
   - 运行：

   ```bash
   npx ultracite init
   ```

   - 在交互提示中，引导用户选择 **Biome** 作为 Toolchain，并根据当前编辑器（例如 `VS Code` 或 `Cursor`）生成对应的工作区配置。

4. **配置 Git hooks（Lefthook）**  
   - 安装并初始化 Lefthook：

   ```bash
   pnpm add -D lefthook
   npx lefthook install
   ```

5. **创建 `lefthook.yml`**  
   - 在项目根目录创建或覆盖 `lefthook.yml`，内容为：

   ```yaml
   pre-commit:
     parallel: true
     commands:
       lint-and-format:
         glob: "*.{js,ts,jsx,tsx,json,jsonc}"
         run: npx @biomejs/biome check --write --no-errors-on-unmatched --files-ignore-unknown=true {staged_files}
   ```

6. **完成与项目启动提示**  
   - 告知用户初始化完成，并建议运行：

   ```bash
   pnpm dev
   ```

## Usage

```bash
bash /mnt/skills/user/init-ultra-next-stack/scripts/init-ultra-next-stack.sh <project-name>
```

**Arguments:**
- `project-name` - 要创建的 Next.js 项目目录名称（必填）。

**Examples:**
- 在当前目录下创建一个名为 `awesome-app` 的项目：

  ```bash
  bash /mnt/skills/user/init-ultra-next-stack/scripts/init-ultra-next-stack.sh awesome-app
  ```

## Output

- 结构化的终端输出，包括每个步骤的状态（创建项目、安装依赖、配置 Ultracite、安装 Lefthook、写入 `lefthook.yml` 等）。  
- 一段简明的总结，说明项目已成功初始化，并给出推荐的下一步命令（例如 `cd <project-name> && pnpm dev`）。  
- 如有部分步骤失败，应在输出中标明失败的阶段和错误提示摘要。

## Present Results to User

当向用户汇报结果时，使用如下结构化格式：

- **Summary**: 一句话概括本次初始化的结果（成功/部分成功/失败）。  
- **Project**: 显示项目路径和名称。  
- **Steps**:
  - 列出关键步骤及其状态（成功/跳过/失败 + 简要说明）。  
- **Next actions**:
  - 推荐用户可以直接复制执行的命令，例如：
    - `cd <project-name>`
    - `pnpm dev`

如果有错误或需要手动修复的地方，用简洁的 bullet points 提示。

## Troubleshooting

- **`npx create-next-app` 失败 / 网络问题**  
  - 提示用户检查网络代理或重试命令。  
  - 如使用公司私有 npm 源，提醒用户在终端中先配置好镜像。

- **shadcn/ui 初始化失败**  
  - 建议用户在项目目录中单独执行：
    ```bash
    npx shadcn@latest init -d
    ```
  - 检查 Node.js 版本是否满足 shadcn 要求。

- **Ultracite 或 Biome 初始化出错**  
  - 建议用户删除生成的配置文件（如 `.ultracite` 相关文件）并重新运行 `npx ultracite init`。  
  - 如果编辑器未正确识别 Biome 插件，提示在对应编辑器中安装/启用 Biome 支持。

- **Lefthook 钩子未生效**  
  - 提醒用户确认 `.git` 目录存在，且项目已初始化为 Git 仓库。  
  - 如果是新目录，先执行：
    ```bash
    git init
    npx lefthook install
    ```
