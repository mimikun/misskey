<template>
<div>
	<MkPagination :pagination="pagination" class="bfomjevm" ref="list">
		<template #empty>
			<div class="_fullinfo">
				<img src="https://xn--931a.moe/assets/info.jpg" class="_ghost"/>
				<div>{{ $t('nothing') }}</div>
			</div>
		</template>
		<template #default="{items}">
			<div class="token _panel" v-for="token in items" :key="token.id">
				<img class="icon" :src="token.iconUrl" alt=""/>
				<div class="body">
					<div class="name">{{ token.name }}</div>
					<div class="description">{{ token.description }}</div>
					<div class="_keyValue">
						<div>{{ $t('installedDate') }}:</div>
						<div><MkTime :time="token.createdAt"/></div>
					</div>
					<div class="_keyValue">
						<div>{{ $t('lastUsedDate') }}:</div>
						<div><MkTime :time="token.lastUsedAt"/></div>
					</div>
					<div class="actions">
						<button class="_button" @click="revoke(token)"><Fa :icon="faTrashAlt"/></button>
					</div>
					<details>
						<summary>{{ $t('details') }}</summary>
						<ul>
							<li v-for="p in token.permission" :key="p">{{ $t(`_permissions.${p}`) }}</li>
						</ul>
					</details>
				</div>
			</div>
		</template>
	</MkPagination>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faTrashAlt, faPlug } from '@fortawesome/free-solid-svg-icons';
import MkPagination from '@/components/ui/pagination.vue';
import * as os from '@/os';

export default defineComponent({
	components: {
		MkPagination
	},

	data() {
		return {
			INFO: {
				title: this.$t('installedApps'),
				icon: faPlug,
			},
			pagination: {
				endpoint: 'i/apps',
				limit: 100,
				params: {
					sort: '+lastUsedAt'
				}
			},
			faTrashAlt, faPlug
		};
	},

	methods: {
		revoke(token) {
			os.api('i/revoke-token', { tokenId: token.id }).then(() => {
				this.$refs.list.reload();
			});
		}
	}
});
</script>

<style lang="scss" scoped>
.bfomjevm {
	> .token {
		display: flex;
		padding: 16px;

		> .icon {
			display: block;
			flex-shrink: 0;
			margin: 0 12px 0 0;
			width: 50px;
			height: 50px;
			border-radius: 8px;
		}

		> .body {
			width: calc(100% - 62px);
			position: relative;

			> .name {
				font-weight: bold;
			}
		}
	}
}
</style>
