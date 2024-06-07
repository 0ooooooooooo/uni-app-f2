<template>
	<view>
	    <f2-canvas @init="initChart" :width="300" :height="300"></f2-canvas>
		<button @click="addData">aaaaaaa</button>
	</view>
</template>

<script>
	import f2Canvas from '@/components/f2Canvas.vue';
	
	export default {
	    components: { f2Canvas },
	    methods: {  
			addData() {
				const data = [
					{ value: 63.4, city: 'New York', date: '2011-10-01' },
					{ value: 62.7, city: 'Alaska', date: '2011-10-01' },
					{ value: 72.2, city: 'Austin', date: '2011-10-01' },
					{ value: 58, city: 'New York', date: '2011-10-02' },
					{ value: 59.9, city: 'Alaska', date: '2011-10-02' },
					{ value: 67.7, city: 'Austin', date: '2011-10-02' },
					{ value: 53.3, city: 'New York', date: '2011-10-03' },
					{ value: 59.1, city: 'Alaska', date: '2011-10-03' },
					{ value: 69.4, city: 'Austin', date: '2011-10-03' },
				];
				this.chart.source(data, {
					date: {
						range: [0, 1],
						type: 'timeCat',
						mask: 'MM-DD'
					},
					value: {
						max: 300,
						tickCount: 4
					}
				});
				this.chart.area().position('date*value').color('city').adjust('stack');
				this.chart.line().position('date*value').color('city').adjust('stack');
				this.chart.render();
			},
	        initChart({ F2, config }) {
	            this.chart = new F2.Chart(config);
	        }
	    }
	}
</script>

<style>
</style>
