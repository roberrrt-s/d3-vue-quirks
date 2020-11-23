<template>
	<div id="chart-wrapper">
		{{ chartData }}
		<svg id="d3-chart"></svg>
	</div>
</template>

<script>
	import * as d3 from 'd3';

	// Vue export logic
	export default {
		name: "Chart",
		props: ['chartData'],
		data() {
			return {
				svgElement: d3.select('#d3-chart'),
				x: d3.scaleBand().range([0, width]).padding(0.1),
				y: d3.scaleLinear().range([height, 0])
			}
		},
		mounted() {
			console.log('mounted chart');
			console.log(this.svgElement);

			const margin = {
					top: 20,
					right: 20,
					bottom: 30,
					left: 40
				},
				width = 960 - margin.left - margin.right,
				height = 500 - margin.top - margin.bottom;

			// append the svg object to the body of the page
			// append a 'group' element to 'svg'
			// moves the 'group' element to the top left margin
			this.svgElement
				.attr("width", width + margin.left + margin.right)
				.attr("height", height + margin.top + margin.bottom)
				.append("g")
				.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

			let data = this.chartData;

			// format the data
			data.forEach(function(d) {
				d.sales = +d.sales;
			});

			// Scale the range of the data in the domains
			this.x.domain(data.map(function(d) {
				return d.name;
			}));
			this.y.domain([0, d3.max(data, function(d) {
				return d.sales;
			})]);

			// append the rectangles for the bar chart
			svg.selectAll(".bar")
				.data(data)
				.enter().append("rect")
				.attr("class", "bar")
				.attr("x", function(d) {
					return this.x(d.name);
				})
				.attr("width", this.x.bandwidth())
				.attr("y", function(d) {
					return this.y(d.sales);
				})
				.attr("height", function(d) {
					return height - this.y(d.sales);
				});

			// add the x Axis
			svg.append("g")
				.attr("transform", "translate(0," + height + ")")
				.call(d3.axisBottom(this.x));

			// add the y Axis
			svg.append("g")
				.call(d3.axisLeft(this.y));

		},

		updated() {
			console.log('Updated chart');
			console.log(this.chartData)

			let svg = d3.select("#d3-chart").select('g').selectAll('.bar')

			svg
				.data(this.chartData)
				.enter()
				.transition()
				.duration(1000)

		},

		computed() {
			console.log('Computed chart');
			console.log(this)
		}

	};

</script>