# Modal: NiceModal + SchemaForm + RPC / fetch

```tsx
import NiceModal, { antdModalV5, useModal } from '@ebay/nice-modal-react';
import { Form } from 'antd';
import { Modal, useApp } from '~/components/antd';
import { SchemaForm } from '~/components/pro-components/schema-form';

export interface MyModalProps {
	record?: { id: string };
	onOK?: () => void;
}

export const MyModal = NiceModal.create<MyModalProps>(({ record, onOK }) => {
	const modal = useModal();
	const antdV5Modal = antdModalV5(modal);
	const { message } = useApp();
	const [form] = Form.useForm();

	const handleSubmit = async () => {
		try {
			const values = await form.validateFields();
			// await honoRpc.api.someResource.$post({ json: values });
			message.success('操作成功!');
			antdV5Modal.onCancel();
			onOK?.();
		} catch (_error) {
			message.error('操作失败，请检查输入!');
		}
	};

	return (
		<Modal title='Modal Title' {...antdV5Modal} onOk={handleSubmit}>
			<SchemaForm
				form={form}
				submitter={false}
				columns={[
					{
						title: 'Field Label',
						dataIndex: 'fieldName',
						formItemProps: {
							rules: [{ required: true, message: '请输入…' }],
						},
					},
				]}
			/>
		</Modal>
	);
});
```

Wire `honoRpc` or `fetch` in `handleSubmit` to match your API.
