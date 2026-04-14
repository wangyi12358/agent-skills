# Route module: Zod + service layer

## `routes/user/schema.ts`

```ts
import { z } from 'zod';

export const userIdParamsSchema = z.object({
	id: z.string().min(1, 'id is required'),
});

export const listUsersQuerySchema = z.object({
	limit: z.coerce.number().int().min(1).max(50).default(10),
});

export type UserIdParams = z.infer<typeof userIdParamsSchema>;
export type ListUsersQuery = z.infer<typeof listUsersQuerySchema>;
```

## `services/user.ts`

```ts
import type { ListUsersQuery } from '@/server/routes/user/schema';

export async function listUsers(_query: ListUsersQuery) {
	// Prisma / domain logic; no Hono Context here
	return [];
}

export async function getUserById(_id: string) {
	return null;
}
```

## `routes/user/route.ts`

```ts
import { zValidator } from '@hono/zod-validator';
import { factory } from '@/server/factory';
import {
	listUsersQuerySchema,
	userIdParamsSchema,
} from '@/server/routes/user/schema';
import { getUserById, listUsers } from '@/server/services/user';

export const userRoute = factory
	.createApp()
	.get('/', zValidator('query', listUsersQuerySchema), async (c) => {
		const query = c.req.valid('query');
		const users = await listUsers(query);
		return c.json({ data: users });
	})
	.get('/:id', zValidator('param', userIdParamsSchema), async (c) => {
		const { id } = c.req.valid('param');
		const user = await getUserById(id);
		if (!user) {
			return c.json({ message: 'User not found' }, 404);
		}
		return c.json({ data: user });
	});
```
