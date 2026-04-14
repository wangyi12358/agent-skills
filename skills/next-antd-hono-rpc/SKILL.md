# Frontend development (Next.js + Hono RPC)

Portable guidelines for AI agents and contributors building a **Next.js (App Router)** UI that talks to a **Hono** API via a **typed RPC client** (`hono/client` `hc`). Adapt paths, package manager, and UI libraries to the target repository.

## When to customize per project

Record stack-specific choices in the repo’s **`AGENTS.md`** / **`README.md`** (or a short **`PROJECT.md`**), for example:

- Next.js / React versions
- UI kit (e.g. Ant Design + Pro Components, shadcn, MUI)
- i18n (e.g. `next-intl` and route segment layout)
- Global state (e.g. Jotai, Zustand) and data fetching (optional SWR / TanStack Query)
- Path aliases (`@/`, `~/` → `src/`)
- Lint/format commands (`pnpm`, `npm`, `yarn`, or `bun`)

This **`SKILL.md`** stays generic; **concrete snippets** live under **`example/`** (same folder as this file).

## Typical concerns (checklist)

- **Framework**: App Router, server vs client components (`'use client'` where needed)
- **API**: One Hono `app` mounted under e.g. `/api`; client is `hc<typeof app>(baseURL)` so routes stay type-safe
- **Auth**: Session/auth client aligned with your backend (e.g. Better Auth, NextAuth, custom cookies)
- **Tables/forms**: Prefer your project’s **wrapped** table/form components over raw vendor imports when wrappers exist
- **Modals**: If using NiceModal (or similar), keep submit logic in `onOk` / handlers; use design-system `message` for feedback

## Project structure (template)

Adjust names to match the repo:

```
<src>/
  app/
    [locale]/              # optional: i18n segment
    api/[[...route]]/      # optional: Hono catch-all for Next.js
  components/              # UI + wrappers (antd, pro-components, …)
  hooks/
  lib/                     # hono client, auth client, utils
  server/                  # if colocated: Hono routes/services (often separate service repo)
```

## Coding standards (portable defaults)

- **TypeScript**: strict; avoid `any` — prefer `unknown`, `InferRequestType` / `InferResponseType` from `hono/client`, or shared types from the server/Zod schemas
- **Formatting**: follow the repo’s Biome/ESLint/Prettier config
- **File naming**: kebab-case for files; PascalCase for React components when that is the project norm
- **Imports**: follow the repo’s import-order convention (external → aliased `lib` → `hooks` → `components` → relative)
- **No manual memoization by default**: do **not** add `useMemo`, `useCallback`, or `React.memo` in pages and ordinary UI unless the team explicitly requests a performance pass. Prefer inline column definitions, objects, and event handlers; keep code obvious and let the framework/compiler handle re-renders.

## Backend API: Hono RPC

### Single typed client

Export a shared client (e.g. `honoRpc`) from one module so `typeof app` stays the single source of truth:

- Server: `app.basePath('/api')` (or your chosen prefix) and `.route('/user', userRoute)` → client paths often look like `honoRpc.api.user.$get`
- Always check **`res.ok`** before treating the body as success data

### Examples (copy from `example/`)

| Topic | File |
|--------|------|
| Direct `$get` / `$post` | [`example/hono-rpc-direct-call.md`](./example/hono-rpc-direct-call.md) |
| SWR + `InferRequestType` | [`example/hono-rpc-swr.md`](./example/hono-rpc-swr.md) |
| Response / query types | [`example/hono-rpc-types.md`](./example/hono-rpc-types.md) |
| ProTable list + `request` | [`example/protable-list-page.md`](./example/protable-list-page.md) |
| NiceModal + SchemaForm | [`example/nice-modal-schema-form.md`](./example/nice-modal-schema-form.md) |
| Detail + dynamic param + SWR | [`example/detail-page-swr.md`](./example/detail-page-swr.md) |

See [`example/README.md`](./example/README.md) for the index.

## Component patterns (Ant Design Pro–style)

If the project uses **ProTable** / **ProDescriptions** / **SchemaForm**:

- Use the repo’s **wrapped** `ProTable` if provided (consistent `rowKey`, `actionRef.reload()`, etc.)
- **`hideInSearch` / `hideInTable`**: search-only vs table-only columns
- **`valueType: 'option'`** for action columns
- Put reusable **select options** / `valueEnum` in shared modules or small **`hooks/`** when the same data is reused; return **plain objects** (no `useMemo` unless the project explicitly allows it)

## Common tasks

| Task | Steps |
|------|--------|
| New list page | Add route under `app/…`; `PageContainer` + table; `request` calls typed RPC |
| New modal | `NiceModal.create` + form; submit calls RPC or `fetch`; `onOK` refreshes parent |
| New API surface | Implement on server, export `app`; client types update via `hc<typeof app>` |
| Prisma / schema changed | Run the repo’s generate script (e.g. `prisma generate`) |

## Error handling and UX

- **try/catch** (or Result types) around user-triggered mutations
- Surface failures with the design system’s **message** / **notification**
- For RPC: branch on `!res.ok` and map status codes to user-visible copy when needed

## Styling

- Prefer the repo’s standard (e.g. **Tailwind** utility classes, CSS modules, or styled components)
- Keep spacing/typography consistent with existing pages

## Common mistakes

1. Duplicating ad hoc `fetch('/api/...')` strings when a typed RPC route already exists on `app`
2. Using the wrong **SWR key** when query params or ids change
3. Putting **hooks** inside table `request` callbacks — keep `request` as async logic only
4. Ignoring **`res.ok`** for RPC responses

## Related skills

- HTTP/route/service layering and Zod validation: **`../backend/SKILL.md`**
- Prisma schema and seeds: **`../database/SKILL.md`**
