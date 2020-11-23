<template>
	<button v-on:click="updateData">Click me!</button>
	<br />
	<Chart :chartData="chartData" />
</template>

<script>
	import getData from '../helpers/getData';
	import Chart from './Chart.vue'

	// Vue export logic
	export default {
		name: "Main",
		components: {
			Chart,
		},
		props: ['data'],
		data() {
			return {
				chartData: getData(),
			}
		},

		mounted() {
			console.log('Mounted main')
		},

		updated() {
			console.log('Updated main');
		},

		methods: {
			updateData: async function() {
				this.chartData = getData()
				await this.$nextTick()
			}
		},
	};

</script>