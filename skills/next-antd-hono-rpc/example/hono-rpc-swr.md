# Hono RPC + SWR (`InferRequestType`)

SWR is optional; install if you want caching/revalidation.

```tsx
'use client';

import type { InferRequestType } from 'hono/client';
import useSWR from 'swr';
import { honoRpc } from '~/lib/hono-client';

const $get = honoRpc.api.user.$get;

const fetcher =
	(arg: InferRequestType<typeof $get>) => async () => {
		const res = await $get(arg);
		if (!res.ok) {
			throw new Error('Request failed');
		}
		return res.json();
	};

export const UserListPreview = () => {
	const { data, error, isLoading } = useSWR(
		['api-user', { limit: 10 }],
		fetcher({ query: { limit: 10 } }),
	);

	if (error) {
		return <div>failed to load</div>;
	}
	if (isLoading) {
		return <div>loading…</div>;
	}

	return <pre>{JSON.stringify(data, null, 2)}</pre>;
};
```

Use a **stable SWR key** that includes anything that changes the response (query params, ids).
