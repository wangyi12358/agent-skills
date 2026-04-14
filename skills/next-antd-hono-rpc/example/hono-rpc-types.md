# Types from Hono RPC and Zod (server)

## From Hono client

```tsx
import type { InferRequestType, InferResponseType } from 'hono/client';
import { honoRpc } from '~/lib/hono-client';

type ListQuery = InferRequestType<typeof honoRpc.api.user.$get>['query'];
type ListBody = InferResponseType<typeof honoRpc.api.user.$get, 200>;
```

## From shared server schema (Zod)

```tsx
import type { ListUsersQuery } from '~/server/routes/user/schema';
```

## Component props

```tsx
export interface ExampleProps {
	record: { id: string };
	onOK?: () => void;
}
```
