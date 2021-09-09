<script>
	export let stroke = "none"
	export let fill = "#fff"
	export let hover = "#000"
	export let textColor = '#fff';
	export let activeData = {}
	export let data = {}
	export let spacer = 4
	export let x = 0
	export let y = 0
	export let r = 10
  const labelVisibilityThreshold = r => r > 6.8;
  const move = (x, y) => `transform: translate(${x}px, ${y}px`;

	let fillColor = fill + "cc"

  const updateActiveData = (data, fill, stroke) => {
    activeData = Object.assign({}, data, {fill, stroke});
    fillColor = hover
    console.log("activeData", activeData)
  }

  const emptyActiveData = (e) => {
  	fillColor = fill + "cc";
    activeData = {}
  }


</script>

<g>
	<circle
	  style="{move(x, y)}"
	  r="{Math.max(0, r)}"
	  height="100%"
	  fill="{fillColor}"
	  stroke="{stroke}"
	/>
  {#if labelVisibilityThreshold(r)}
    <text style="{move(x, y+1)}"
      dominant-baseline="middle"
      text-anchor="middle"
      fill="{textColor}"
      font-size="12px"
      font-weight="500"
    >{data.grapheme.toLowerCase()}</text>
  {/if}
  <rect
    style="{move(x - r - 0.5 * spacer, 0)}"
    stroke-opacity="0"
    fill-opacity="0"
    height='50px'
    width="{2 * r + spacer}"
    cursor="pointer"
    on:mouseover={() => updateActiveData(data, fill, stroke)}
    on:focus={() => updateActiveData(data, fill, stroke)}
    on:mouseout={() => emptyActiveData()}
    on:blur={() => emptyActiveData()}
  />
</g>

<style>
  rect:focus {
    outline:  black 1005px
  }
  
</style>
