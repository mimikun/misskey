<template>
<div>
	<div class="_section">
		<div class="_content">
			<MkButton @click="generateToken">{{ $t('generateAccessToken') }}</MkButton>
		</div>
	</div>
	<div class="_section">
		<MkA to="/api-console" :behavior="isDesktop ? 'window' : null">API console</MkA>
	</div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faKey } from '@fortawesome/free-solid-svg-icons';
import MkButton from '@/components/ui/button.vue';
import MkInput from '@/components/ui/input.vue';
import * as os from '@/os';

export default defineComponent({
	components: {
		MkButton, MkInput
	},

	emits: ['info'],

	data() {
		return {
			INFO: {
				title: 'API',
				icon: faKey
			},
			isDesktop: window.innerWidth >= 1100,
		};
	},

	mounted() {
		this.$emit('info', this.INFO);
	},

	methods: {
		generateToken() {
			os.popup(import('@/components/token-generate-window.vue'), {}, {
				done: async result => {
					const { name, permissions } = result;
					const { token } = await os.api('miauth/gen-token', {
						session: null,
						name: name,
						permission: permissions,
					});

					os.dialog({
						type: 'success',
						title: this.$t('token'),
						text: token
					});
				},
			}, 'closed');
		},
	}
});
</script>
