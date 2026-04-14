# Backend skill — examples

Referenced from **`../SKILL.md`**. Snippets illustrate **Hono 4 + `hono/factory` + Zod (`@hono/zod-validator`) + services layer**.

Replace `@/server` / `@/lib` with your project’s path aliases.

| File | Topic |
|------|--------|
| [`env-factory.md`](./env-factory.md) | Typed `Env` + `createFactory` |
| [`middleware-prisma-auth.md`](./middleware-prisma-auth.md) | Attach `db` / session to context |
| [`index-compose.md`](./index-compose.md) | Mount middlewares, `basePath`, routes, auth handler |
| [`route-schema-service.md`](./route-schema-service.md) | `zValidator` + service calls + JSON envelope |
