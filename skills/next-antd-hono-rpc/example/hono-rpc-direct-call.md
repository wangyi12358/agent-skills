# Hono RPC — direct call (no hook)

```tsx
import { honoRpc } from '~/lib/hono-client';

const res = await honoRpc.api.user.$get({
	query: { limit: 10 },
});

if (!res.ok) {
	// handle error
}

const body = await res.json();
```

Replace `honoRpc`, `api.user`, and query shapes with your app’s client export and routes.
