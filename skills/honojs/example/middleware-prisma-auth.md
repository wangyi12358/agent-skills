# Middleware: Prisma client + optional session

## Prisma on context

```ts
import { prisma } from '@/lib/prisma';
import { factory } from '../factory';

export const prismaMiddleware = factory.createMiddleware(async (c, next) => {
	c.set('db', prisma);
	await next();
});
```

## Session (example: read from your auth library)

```ts
import { auth } from '@/lib/auth';
import { factory } from '../factory';

export const authMiddleware = factory.createMiddleware(async (c, next) => {
	const session = await auth.api.getSession({ headers: c.req.raw.headers });

	if (!session) {
		c.set('user', null);
		c.set('session', null);
		return next();
	}

	c.set('user', session.user);
	c.set('session', session.session);
	return next();
});
```

Wire `auth` to whatever your stack uses (Better Auth, Lucia, etc.).
