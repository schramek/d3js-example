<template>
  <div class="hello">
    <div id="test">
    <svg :id="randomid"></svg>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import * as d3 from 'd3';
import _ from 'lodash';
import dataObject from './data-simple.json';

export default {
  name: 'SunburstD3',
  data() {
    return {};
  },
  methods: {
    color(d) {
      //console.log('df', d);
      while (d.depth > 1) {
        d = d.parent;
      }
      //console.log(d);
      //console.log('depth', d.depth);
      //console.log('height', d.height);
      console.log(d.ancestors().map(d => d.data.mlShould));

      return "rgb(255,122,0, 0.2)";
    }
   },
  mounted() {
    this.$nextTick(function () {
      const data = dataObject;
      const radius = 928 / 2;

      const partition = data => d3.partition()
          .size([2 * Math.PI, radius])
          (d3.hierarchy(data)
              .sum(d => d.value)
              .sort((a, b) => b.value - a.value));

      const arc = d3.arc()
          .startAngle(d => d.x0)
          .endAngle(d => d.x1)
          .padAngle(d => Math.min((d.x1 - d.x0) / 2, 0.005))
          .padRadius(radius / 2)
          .innerRadius(d => d.y0)
          .outerRadius(d => d.y1 - 1);

      const root = partition(data);

      const format = d3.format(",d");

      const svg = d3.select("#" + this.randomid);

      svg.append("g")
          .attr("fill-opacity", 0.6)
          .selectAll("path")
          .data(root.descendants().filter(d => d.depth))
          .join("path")
          .attr("fill", d => this.color(d))
          .attr("d", arc)
          .append("title")
          .text(d => `${d.ancestors().map(d => d.data.name).reverse().join("/")}\n${format(d.value)}`);

      svg.append("g")
          .attr("pointer-events", "none")
          .attr("text-anchor", "middle")
          .attr("font-size", 10)
          .attr("font-family", "sans-serif")
          .selectAll("text")
          .data(root.descendants().filter(d => d.depth && (d.y0 + d.y1) / 2 * (d.x1 - d.x0) > 10))
          .join("text")
          .attr("transform", function(d) {
            const x = (d.x0 + d.x1) / 2 * 180 / Math.PI;
            const y = (d.y0 + d.y1) / 2;
            return `rotate(${x - 90}) translate(${y},0) rotate(${x < 180 ? 0 : 180})`;
          })
          .attr("dy", "0.35em")
          .text(d => d.data.name);

      const autoBox = [-200, -200, 800, 800];
      return svg.attr("viewBox", autoBox).node();
    });
  },
  computed: {
    randomid: function () {
      return _.uniqueId('d3_')
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
