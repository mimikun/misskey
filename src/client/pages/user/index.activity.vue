<template>
<div>
	<div ref="chart"></div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ApexCharts from 'apexcharts';
import * as os from '@/os';

export default defineComponent({
	props: {
		user: {
			type: Object,
			required: true
		},
		limit: {
			type: Number,
			required: false,
			default: 40
		}
	},
	data() {
		return {
			fetching: true,
			data: [],
			peak: null
		};
	},
	mounted() {
		os.api('charts/user/notes', {
			userId: this.user.id,
			span: 'day',
			limit: this.limit
		}).then(stats => {
			const normal = [];
			const reply = [];
			const renote = [];

			const now = new Date();
			const y = now.getFullYear();
			const m = now.getMonth();
			const d = now.getDate();

			for (let i = 0; i < this.limit; i++) {
				const x = new Date(y, m, d - i);
				normal.push([
					x,
					stats.diffs.normal[i]
				]);
				reply.push([
					x,
					stats.diffs.reply[i]
				]);
				renote.push([
					x,
					stats.diffs.renote[i]
				]);
			}

			const chart = new ApexCharts(this.$refs.chart, {
				chart: {
					type: 'bar',
					stacked: true,
					height: 100,
					sparkline: {
						enabled: true
					},
				},
				plotOptions: {
					bar: {
						columnWidth: '40%'
					}
				},
				dataLabels: {
					enabled: false
				},
				grid: {
					clipMarkers: false,
					padding: {
						top: 0,
						right: 8,
						bottom: 0,
						left: 8
					}
				},
				tooltip: {
					shared: true,
					intersect: false
				},
				series: [{
					name: 'Normal',
					data: normal
				}, {
					name: 'Reply',
					data: reply
				}, {
					name: 'Renote',
					data: renote
				}],
				xaxis: {
					type: 'datetime',
					crosshairs: {
						width: 1,
						opacity: 1
					}
				}
			});

			chart.render();
		});
	}
});
</script>
