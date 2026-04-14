# Backend API (Hono + Zod + services)

Portable guidelines for building a **Hono** HTTP API with **Zod** validation, a **typed `Env`**, and a **routes / services** split. Adapt folders and path aliases to the target repository.

## When to customize per project

Document in **`AGENTS.md`** / **`README.md`** / **`PROJECT.md`**:

- Runtime adapter (**Next.js** `hono/vercel`, **Bun**, **Node**, **Cloudflare Workers**, etc.)
- Global prefix (e.g. `/api`)
- ORM (**Prisma**, Drizzle, Kysely) and how the client reaches handlers (`c.var.db` vs direct imports)
- Auth library and how session/user is attached to context
- Package manager and lint/test commands

**Runnable snippets** for this layout live under **`example/`** (see [`example/README.md`](./example/README.md)).

## Directory layout (routes / services)

Template — rename `src/server` if your repo differs:

```
<serverRoot>/
  index.ts              # Compose app: middlewares, basePath, .route(...), optional auth catch-all
  factory.ts            # createFactory<Env>()
  type.d.ts             # Env Variables (db, user, session, …)
  middlewares/          # Cross-cutting middleware only
  routes/
    <domain>/
      route.ts          # HTTP: methods, zValidator, status, c.json
      schema.ts         # Zod schemas + z.infer exports
  services/
    <domain>.ts         # Business logic + data access (no Hono Context)
```

If a domain grows, use `services/<domain>/` with a barrel `index.ts`.

### Responsibility split

| Layer | Belongs here | Avoid |
|--------|----------------|--------|
| `routes/<domain>/` | Paths, verbs, `zValidator`, HTTP status, response shape | Heavy domain rules; prefer calling services |
| `services/` | Queries, transactions, rules, reusable reads/writes | `Context`, raw `Request`, framework types |
| `middlewares/` | `c.set` for shared context | Per-route validation (keep in `schema.ts`) |

## Coding standards (defaults)

Match the **target repo** when it conflicts with this list:

- **TypeScript**: strict; avoid `any`
- **Validation**: Zod + `@hono/zod-validator` (`zValidator`); validated input via `c.req.valid(...)`
- **Route files**: kebab-case domain folders (`user`, `order-item`); `route.ts` + `schema.ts` inside each
- **Services**: `services/<domain>.ts` or `services/<domain>/...`

## Env and factory

1. Define **`Env['Variables']`** for every `c.set` key middlewares populate.
2. Export a single **`factory = createFactory<Env>()`**; build routes with `factory.createApp()` and middlewares with `factory.createMiddleware()`.

See [`example/env-factory.md`](./example/env-factory.md).

## Middlewares

- Register **global** middleware in the main `index.ts` in an order that matches your auth/DB needs.
- Keep middleware thin: attach context, optional short-circuit (e.g. require login in a dedicated middleware if you add one).

See [`example/middleware-prisma-auth.md`](./example/middleware-prisma-auth.md).

## Routes

- All request validation for a domain in **`routes/<domain>/schema.ts`**; export **`z.infer`** types for services.
- Handlers: `zValidator('query' | 'param' | 'json' | 'form' | 'header', schema)`.
- Sub-apps: `factory.createApp()` → export `userRoute` → parent `app.route('/user', userRoute)`.

## Services

- **Pure-ish functions**: `async` functions with plain arguments; return data or `null` / throw per project convention.
- **Do not** import `Context` in services; pass `db` from `c.var` if you want explicit injection for tests.

## Composing the app

See [`example/index-compose.md`](./example/index-compose.md) for `basePath`, route mounting, and an optional Better Auth-style handler.

Full **route + schema + service** sample: [`example/route-schema-service.md`](./example/route-schema-service.md).

## Checklist: new API domain

1. `routes/<name>/schema.ts` — Zod + inferred types  
2. `services/<name>.ts` — domain logic  
3. `routes/<name>/route.ts` — validators + call services + JSON/errors  
4. Register on parent app: `.route('/<name>', <name>Route)`  
5. Run the repo linter/tests and regenerate ORM client if the schema changed  

## Prisma and database schema

Schema location, migrations, and client setup belong to the **database** skill: **`../database/SKILL.md`**.

## Frontend consumption

If the client uses **`hono/client`** (`hc<typeof app>`), types follow the server `app` export. See **`../frontend/SKILL.md`** and **`../frontend/example/`**.
