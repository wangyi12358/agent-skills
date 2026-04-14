# Composing the Hono `app` (Next.js / Vercel adapter)

```ts
import { Hono } from 'hono';
import { auth } from '@/lib/auth';
import { authMiddleware } from './middlewares/auth';
import { prismaMiddleware } from './middlewares/prisma';
import { userRoute } from './routes/user/route';

export const app = new Hono()
	.use(prismaMiddleware)
	.use(authMiddleware)
	.basePath('/api')
	.route('/user', userRoute);

app.on(['POST', 'GET'], '/auth/*', (c) => {
	return auth.handler(c.req.raw);
});
```

- Order **global middleware** before `.basePath` / `.route` if your Hono version expects that.
- The **`/auth/*`** block is specific to **Better Auth**; remove or replace for other auth.

Mount in Next.js with `hono/vercel` `handle(app)` (or your runtime’s adapter).
