<template>
<div>
	<div class="_section" style="padding: 0;">
		<MkTab class="_content" v-model:value="tab" :items="[{ label: $t('_channel.featured'), value: 'featured', icon: faFireAlt }, { label: $t('_channel.following'), value: 'following', icon: faHeart }, { label: $t('_channel.owned'), value: 'owned', icon: faEdit }]"/>
	</div>

	<div class="_section">
		<div class="_content grwlizim featured" v-if="tab === 'featured'">
			<MkPagination :pagination="featuredPagination" #default="{items}">
				<MkChannelPreview v-for="channel in items" class="uveselbe" :channel="channel" :key="channel.id"/>
			</MkPagination>
		</div>

		<div class="_content grwlizim following" v-if="tab === 'following'">
			<MkPagination :pagination="followingPagination" #default="{items}">
				<MkChannelPreview v-for="channel in items" class="uveselbe" :channel="channel" :key="channel.id"/>
			</MkPagination>
		</div>

		<div class="_content grwlizim owned" v-if="tab === 'owned'">
			<MkButton class="new" @click="create()"><Fa :icon="faPlus"/></MkButton>
			<MkPagination :pagination="ownedPagination" #default="{items}">
				<MkChannelPreview v-for="channel in items" class="uveselbe" :channel="channel" :key="channel.id"/>
			</MkPagination>
		</div>
	</div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faSatelliteDish, faPlus, faEdit, faFireAlt } from '@fortawesome/free-solid-svg-icons';
import { faHeart } from '@fortawesome/free-regular-svg-icons';
import MkChannelPreview from '@/components/channel-preview.vue';
import MkPagination from '@/components/ui/pagination.vue';
import MkButton from '@/components/ui/button.vue';
import MkTab from '@/components/tab.vue';

export default defineComponent({
	components: {
		MkChannelPreview, MkPagination, MkButton, MkTab
	},
	data() {
		return {
			INFO: {
				title: this.$t('channel'),
				icon: faSatelliteDish
			},
			tab: 'featured',
			featuredPagination: {
				endpoint: 'channels/featured',
				noPaging: true,
			},
			followingPagination: {
				endpoint: 'channels/followed',
				limit: 5,
			},
			ownedPagination: {
				endpoint: 'channels/owned',
				limit: 5,
			},
			faSatelliteDish, faPlus, faEdit, faHeart, faFireAlt
		};
	},
	methods: {
		create() {
			this.$router.push(`/channels/new`);
		}
	}
});
</script>

<style lang="scss" scoped>
.grwlizim {
	padding: 16px 0;

	&.my .uveselbe:first-child {
		margin-top: 16px;
	}

	.uveselbe:not(:last-child) {
		margin-bottom: 8px;
	}

	@media (min-width: 500px) {
		.uveselbe:not(:last-child) {
			margin-bottom: 16px;
		}
	}
}
</style>
