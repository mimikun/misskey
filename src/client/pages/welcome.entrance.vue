<template>
<div class="rsqzvsbo">
	<div class="_section">
		<div class="_content _panel about" v-if="meta">
			<div class="body">
				<div class="desc" v-html="meta.description || $t('introMisskey')"></div>
				<MkButton @click="signup()" style="display: inline-block; margin-right: 16px;" primary>{{ $t('signup') }}</MkButton>
				<MkButton @click="signin()" style="display: inline-block;">{{ $t('login') }}</MkButton>
			</div>
		</div>
	</div>
	<div class="_section">
		<div class="_content">
			<XNotes :pagination="featuredPagination"/>
		</div>
	</div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { toUnicode } from 'punycode';
import XSigninDialog from '@/components/signin-dialog.vue';
import XSignupDialog from '@/components/signup-dialog.vue';
import MkButton from '@/components/ui/button.vue';
import XNotes from '@/components/notes.vue';
import { host } from '@/config';
import * as os from '@/os';

export default defineComponent({
	components: {
		MkButton,
		XNotes,
	},

	data() {
		return {
			featuredPagination: {
				endpoint: 'notes/featured',
				limit: 10,
				noPaging: true,
			},
			host: toUnicode(host),
		};
	},

	computed: {
		meta() {
			return this.$store.state.instance.meta;
		},
	},

	created() {
		os.api('stats').then(stats => {
			this.stats = stats;
		});
	},

	methods: {
		signin() {
			os.popup(XSigninDialog, {
				autoSet: true
			}, {}, 'closed');
		},

		signup() {
			os.popup(XSignupDialog, {
				autoSet: true
			}, {}, 'closed');
		}
	}
});
</script>

<style lang="scss" scoped>
.rsqzvsbo {
	> ._section {
		> .about {
			> .body {
				padding: 32px;

				@media (max-width: 500px) {
					padding: 16px;
				}
			}
		}
	}
}
</style>
