# Detail page: dynamic segment + SWR + ProDescriptions

For `GET /api/user/:id`, the typed client often exposes `$get` on the dynamic key (e.g. `honoRpc.api.user[':id'].$get` — confirm with editor autocomplete).

```tsx
'use client';

import { useParams } from 'next/navigation';
import useSWR from 'swr';
import { ProCard } from '@ant-design/pro-components';
import { PageContainer } from '~/components/pro-components/page-container';
import { ProDescriptions } from '~/components/pro-components/pro-descriptions';
import { honoRpc } from '~/lib/hono-client';

export default function UserDetailPage() {
	const params = useParams();
	const id = params.id as string;

	const { data, isLoading } = useSWR(
		id ? ['api-user', id] : null,
		async () => {
			const res = await honoRpc.api.user[':id'].$get({
				param: { id },
			});
			if (!res.ok) {
				throw new Error('Not found');
			}
			return res.json();
		},
	);

	return (
		<PageContainer title='User' loading={isLoading}>
			<ProCard title='Information'>
				<ProDescriptions
					dataSource={data?.data}
					column={2}
					columns={[{ title: 'Email', dataIndex: 'email' }]}
				/>
			</ProCard>
		</PageContainer>
	);
}
```

If generated typings differ for your route shape, follow your server route module and the inferred client on `honoRpc`.
