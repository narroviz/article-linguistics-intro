<script>
  import { spring } from "svelte/motion";
  import { cubicOut } from "svelte/easing";
  import { extent } from "d3-array";
  import { interpolateHcl } from "d3-interpolate";
  import { scaleSqrt, scaleLinear } from "d3-scale";
  import ScatterplotCircle from './ScatterplotCircle.svelte'

  // utility function for translating elements
  const move = (x, y) => `transform: translate(${x}px, ${y}px`;

  export let data = [];
  // accessor functions to quickly access data structures
  export let xAccessor = d => d[0];
  export let yAccessor = d => d[1];
  export let rAccessor = d => d[2];
  export let fillAccessor = d => d[3];
  export let strokeAccessor = d => d[4];
  export let dataAccessor = d => d[5];
  export let activeData = {};
  export let spacer = 4;


  export let margins = {
    // typical d3 margin convention
    top: 0,
    right: 0,
    bottom: 0,
    left: 0
  };

  let width = 500;
  $: height = 50;
  $: mainWidth = width - margins.right - margins.left;
  $: mainHeight = height - margins.top - margins.bottom;

  // make me some scales!
  const xScale = scaleLinear()
    .domain(extent(data, xAccessor))
    .range(extent(data, xAccessor));
  const yScale = scaleLinear()
    .domain(extent(data, yAccessor))
    .range([mainHeight, 0]);
  const rScale = scaleLinear()
    .domain(extent(data, rAccessor))
    .range([0,20]);

  const dots = data.map((d, i) => ({
      x: 15 + xScale(xAccessor(d)),
      y: 25,
      r: rAccessor(d),
      fill: fillAccessor(d),
      stroke: strokeAccessor(d),
      data: dataAccessor(d)
    }));

  $: data, mainWidth;
</script>

<figure class="c" bind:clientWidth="{width}">
  <svg {width} {height}>
    <g style="{move(margins.top, margins.left)}">
      {#each dots as { x, y, r, fill, stroke, data }}
        
        <ScatterplotCircle
          x="{x}"
          y="{y}"
          r="{Math.max(0, r)}"
          stroke="{stroke}"
          fill="{fill}"
          hover="{stroke}"
          data="{data}"
          bind:activeData={activeData}
          spacer="{spacer}"
      />
      
      {/each}
    </g>
  </svg>
</figure>

<style>

figure {
  margin: 0;
  height:  50px;
}

</style>