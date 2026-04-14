# List page: ProTable + Hono RPC `request`

```tsx
'use client';

import type { ActionType } from '@ant-design/pro-components';
import { useRef } from 'react';
import { PageContainer } from '~/components/pro-components/page-container';
import { ProTable } from '~/components/pro-components/pro-table';
import { honoRpc } from '~/lib/hono-client';

type Row = { id: string; email: string; name: string };

export default function UserListPage() {
	const actionRef = useRef<ActionType>(null);

	return (
		<PageContainer>
			<ProTable<Row>
				columns={[
					{ title: 'Email', dataIndex: 'email' },
					{
						title: '操作',
						valueType: 'option',
						render: (_text, record) => (
							<div className='space-x-3'>{/* actions */}</div>
						),
					},
				]}
				actionRef={actionRef}
				request={async (params) => {
					const { current = 1, pageSize = 10 } = params;
					const res = await honoRpc.api.user.$get({
						query: { limit: pageSize },
					});
					const json = await res.json();
					const rows = json.data ?? [];

					return {
						data: rows,
						success: res.ok,
						// Replace with server `total` / `pagination` when the API supports it
						total: rows.length,
					};
				}}
			/>
		</PageContainer>
	);
}
```

Adjust `request` to match your API’s query and pagination contract. Avoid putting React hooks inside `request`; keep it as plain async logic.
