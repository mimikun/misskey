<template>
<div class="_section">
	<section class="_card _vMargin">
		<div class="_title"><Fa :icon="faCog"/> {{ $t('general') }}</div>
		<div class="_content">
			<div>{{ $t('defaultNavigationBehaviour') }}</div>
			<MkSwitch v-model:value="defaultSideView">{{ $t('openInSideView') }}</MkSwitch>
		</div>
		<div class="_content">
			<div>{{ $t('whenServerDisconnected') }}</div>
			<MkRadio v-model="serverDisconnectedBehavior" value="reload">{{ $t('_serverDisconnectedBehavior.reload') }}</MkRadio>
			<MkRadio v-model="serverDisconnectedBehavior" value="dialog">{{ $t('_serverDisconnectedBehavior.dialog') }}</MkRadio>
			<MkRadio v-model="serverDisconnectedBehavior" value="quiet">{{ $t('_serverDisconnectedBehavior.quiet') }}</MkRadio>
		</div>
		<div class="_content">
			<MkSwitch v-model:value="imageNewTab">{{ $t('openImageInNewTab') }}</MkSwitch>
			<MkSwitch v-model:value="showFixedPostForm">{{ $t('showFixedPostForm') }}</MkSwitch>
			<MkSwitch v-model:value="enableInfiniteScroll">{{ $t('enableInfiniteScroll') }}</MkSwitch>
			<MkSwitch v-model:value="disablePagesScript">{{ $t('disablePagesScript') }}</MkSwitch>
		</div>
		<div class="_content">
			<div>{{ $t('chatOpenBehavior') }}</div>
			<MkRadio v-model="chatOpenBehavior" value="page">{{ $t('showInPage') }}</MkRadio>
			<MkRadio v-model="chatOpenBehavior" value="window">{{ $t('openInWindow') }}</MkRadio>
			<MkRadio v-model="chatOpenBehavior" value="popout">{{ $t('popout') }}</MkRadio>
		</div>
		<div class="_content">
			<MkSelect v-model:value="lang">
				<template #label>{{ $t('uiLanguage') }}</template>

				<option v-for="x in langs" :value="x[0]" :key="x[0]">{{ x[1] }}</option>
			</MkSelect>
		</div>
	</section>

	<section class="_card _vMargin">
		<div class="_title"><Fa :icon="faCog"/> {{ $t('appearance') }}</div>
		<div class="_content">
			<MkSwitch v-model:value="disableAnimatedMfm">{{ $t('disableAnimatedMfm') }}</MkSwitch>
			<MkSwitch v-model:value="reduceAnimation">{{ $t('reduceUiAnimation') }}</MkSwitch>
			<MkSwitch v-model:value="useBlurEffectForModal">{{ $t('useBlurEffectForModal') }}</MkSwitch>
			<MkSwitch v-model:value="useOsNativeEmojis">
				{{ $t('useOsNativeEmojis') }}
				<template #desc><Mfm text="🍮🍦🍭🍩🍰🍫🍬🥞🍪"/></template>
			</MkSwitch>
		</div>
		<div class="_content">
			<div>{{ $t('fontSize') }}</div>
			<MkRadio v-model="fontSize" value="small"><span style="font-size: 14px;">Aa</span></MkRadio>
			<MkRadio v-model="fontSize" :value="null"><span style="font-size: 16px;">Aa</span></MkRadio>
			<MkRadio v-model="fontSize" value="large"><span style="font-size: 18px;">Aa</span></MkRadio>
			<MkRadio v-model="fontSize" value="veryLarge"><span style="font-size: 20px;">Aa</span></MkRadio>
		</div>
		<div class="_content">
			<div>{{ $t('instanceTicker') }}</div>
			<MkRadio v-model="instanceTicker" value="none">{{ $t('_instanceTicker.none') }}</MkRadio>
			<MkRadio v-model="instanceTicker" value="remote">{{ $t('_instanceTicker.remote') }}</MkRadio>
			<MkRadio v-model="instanceTicker" value="always">{{ $t('_instanceTicker.always') }}</MkRadio>
		</div>
	</section>

	<section class="_card _vMargin">
		<div class="_title"><Fa :icon="faColumns"/> {{ $t('deck') }}</div>
		<div class="_content">
			<div>{{ $t('defaultNavigationBehaviour') }}</div>
			<MkSwitch v-model:value="deckNavWindow">{{ $t('openInWindow') }}</MkSwitch>
		</div>
		<div class="_content">
			<MkSwitch v-model:value="deckAlwaysShowMainColumn">
				{{ $t('_deck.alwaysShowMainColumn') }}
			</MkSwitch>
		</div>
		<div class="_content">
			<div>{{ $t('_deck.columnAlign') }}</div>
			<MkRadio v-model="deckColumnAlign" value="left">{{ $t('left') }}</MkRadio>
			<MkRadio v-model="deckColumnAlign" value="center">{{ $t('center') }}</MkRadio>
		</div>
	</section>

	<MkButton @click="cacheClear()" primary style="margin: var(--margin) auto;">{{ $t('cacheClear') }}</MkButton>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faImage, faCog, faColumns, faCogs } from '@fortawesome/free-solid-svg-icons';
import MkButton from '@/components/ui/button.vue';
import MkSwitch from '@/components/ui/switch.vue';
import MkSelect from '@/components/ui/select.vue';
import MkRadio from '@/components/ui/radio.vue';
import MkRange from '@/components/ui/range.vue';
import { langs } from '@/config';
import { clientDb, set } from '@/db';
import * as os from '@/os';

export default defineComponent({
	components: {
		MkButton,
		MkSwitch,
		MkSelect,
		MkRadio,
		MkRange,
	},

	emits: ['info'],

	data() {
		return {
			INFO: {
				title: this.$t('general'),
				icon: faCogs
			},
			langs,
			lang: localStorage.getItem('lang'),
			fontSize: localStorage.getItem('fontSize'),
			faImage, faCog, faColumns
		}
	},

	computed: {
		serverDisconnectedBehavior: {
			get() { return this.$store.state.device.serverDisconnectedBehavior; },
			set(value) { this.$store.commit('device/set', { key: 'serverDisconnectedBehavior', value }); }
		},

		reduceAnimation: {
			get() { return !this.$store.state.device.animation; },
			set(value) { this.$store.commit('device/set', { key: 'animation', value: !value }); }
		},

		useBlurEffectForModal: {
			get() { return this.$store.state.device.useBlurEffectForModal; },
			set(value) { this.$store.commit('device/set', { key: 'useBlurEffectForModal', value: value }); }
		},

		disableAnimatedMfm: {
			get() { return !this.$store.state.device.animatedMfm; },
			set(value) { this.$store.commit('device/set', { key: 'animatedMfm', value: !value }); }
		},

		useOsNativeEmojis: {
			get() { return this.$store.state.device.useOsNativeEmojis; },
			set(value) { this.$store.commit('device/set', { key: 'useOsNativeEmojis', value }); }
		},

		imageNewTab: {
			get() { return this.$store.state.device.imageNewTab; },
			set(value) { this.$store.commit('device/set', { key: 'imageNewTab', value }); }
		},

		disablePagesScript: {
			get() { return this.$store.state.device.disablePagesScript; },
			set(value) { this.$store.commit('device/set', { key: 'disablePagesScript', value }); }
		},

		showFixedPostForm: {
			get() { return this.$store.state.device.showFixedPostForm; },
			set(value) { this.$store.commit('device/set', { key: 'showFixedPostForm', value }); }
		},

		defaultSideView: {
			get() { return this.$store.state.device.defaultSideView; },
			set(value) { this.$store.commit('device/set', { key: 'defaultSideView', value }); }
		},

		deckNavWindow: {
			get() { return this.$store.state.device.deckNavWindow; },
			set(value) { this.$store.commit('device/set', { key: 'deckNavWindow', value }); }
		},

		chatOpenBehavior: {
			get() { return this.$store.state.device.chatOpenBehavior; },
			set(value) { this.$store.commit('device/set', { key: 'chatOpenBehavior', value }); }
		},

		instanceTicker: {
			get() { return this.$store.state.device.instanceTicker; },
			set(value) { this.$store.commit('device/set', { key: 'instanceTicker', value }); }
		},

		enableInfiniteScroll: {
			get() { return this.$store.state.device.enableInfiniteScroll; },
			set(value) { this.$store.commit('device/set', { key: 'enableInfiniteScroll', value }); }
		},

		deckAlwaysShowMainColumn: {
			get() { return this.$store.state.device.deckAlwaysShowMainColumn; },
			set(value) { this.$store.commit('device/set', { key: 'deckAlwaysShowMainColumn', value }); }
		},

		deckColumnAlign: {
			get() { return this.$store.state.device.deckColumnAlign; },
			set(value) { this.$store.commit('device/set', { key: 'deckColumnAlign', value }); }
		},
	},

	watch: {
		lang() {
			localStorage.setItem('lang', this.lang);

			return set('_version_', `changeLang-${(new Date()).toJSON()}`, clientDb.i18n)
				.then(() => location.reload())
				.catch(() => {
					os.dialog({
						type: 'error',
					});
				});
		},

		fontSize() {
			if (this.fontSize == null) {
				localStorage.removeItem('fontSize');
			} else {
				localStorage.setItem('fontSize', this.fontSize);
			}
			location.reload();
		},

		enableInfiniteScroll() {
			location.reload()
		},
	},

	mounted() {
		this.$emit('info', this.INFO);
	},

	methods: {
		cacheClear() {
			// Clear cache (service worker)
			try {
				navigator.serviceWorker.controller.postMessage('clear');

				navigator.serviceWorker.getRegistrations().then(registrations => {
					for (const registration of registrations) registration.unregister();
				});
			} catch (e) {
				console.error(e);
			}

			// Force reload
			location.reload(true);
		}
	}
});
</script>
