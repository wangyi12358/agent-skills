# Typed `Env` + `createFactory`

## `type.d.ts` (or `env.d.ts`)

```ts
import type { PrismaClient } from '@prisma/client';

export interface Env {
	Variables: {
		db: PrismaClient;
		user: { id: string; email: string } | null;
		session: { id: string } | null;
	};
}
```

Extend `Variables` whenever middleware adds `c.set('key', value)`.

## `factory.ts`

```ts
import { createFactory } from 'hono/factory';
import type { Env } from '@/server/type';

export const factory = createFactory<Env>();
```

Use this `factory` for `createApp()` and `createMiddleware()` so `c.var` is typed.
