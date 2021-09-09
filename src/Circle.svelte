<script>
	export let stroke = "none"
	export let fill = "#fff"
	export let hover = "#000"
	export let textColor = '#fff';
	export let label = ''
	export let id = ''
	export let x = 0
	export let y = 0
	export let r = 10
  const labelVisibilityThreshold = r => r > 18;

	let fillColor = fill
	function handleMouseOver(e) {
		fillColor = hover
	}
	function handleMouseOut(e) {
		fillColor = fill;
	}

</script>
<div
  class="circle-group"
  data-id="{id}"
  data-visible="{labelVisibilityThreshold(r)}"
>
	<div
		on:mouseover={(e) => handleMouseOver()}
		on:focus={(e) => handleMouseOver()}
		on:mouseout={(e) => handleMouseOut()}
		on:blur={(e) => handleMouseOut()}
		class="circle"
		style="
			left:{x}px;
			top:{y}px;
			width:{r * 2}px;
			height:{r * 2}px;
			background-color:{fillColor};
			border: 1px solid {stroke};"
	/>
	<div
		class="text-group"
		style="
			color:{textColor};
			left:{x}px;
			top:{y - (labelVisibilityThreshold(r) ? 0 : (r + 4))}px;
			border: 1px solid {labelVisibilityThreshold(r) ? '#ffffff00' : stroke};
			background: {fillColor};
		"
	>
		<div class="text">{id}</div>
		{#if label}
			<div class="text value" >{label}</div>
		{/if}
	</div>
</div>

<style>
	/*.circle,*/
	.text-group {
		position: absolute;
	}
  .circle {
		transform: translate(-50%, -50%);
  }

  .circle-group {
    z-index: 999999;
  } 
	.circle-group[data-visible="false"] .text-group {
		display: none;
		z-index: 999999;
		padding: 10px 10px;
		border-radius: 100px;
		/*background: #fff;*/
		color: #000;
		transform: translate(-50%, -100%);
    top: -4px;
	}
  .circle-group[data-visible="false"]:hover .text-group {
		z-index: 999999;
		display: block !important;
	}
	.circle {
    border-radius: 50%;
    top: 0;
    left: 0;
    position: absolute;
	}
	.text-group {
		width: auto;
    top: 50%;
    left: 50%;
		text-align: center;
		transform: translate(-50%, -50%);
		white-space: nowrap;
		pointer-events: none;
		cursor: pointer;
		line-height: 14px;
		border-radius: 50px;

	}
	.text {
		color: #fff;
		width: 100%;
		font-size: 13px;
		/* text-shadow: -1px -1px 0 #fff, 1px -1px 0 #fff, -1px 1px 0 #fff, 1px 1px 0 #fff; */
	}
	.text.value{
		font-size: 12px;
	}
	.text-group {
		position: absolute;
	}

	@media only screen and (max-width: 600px) {
		.text-group {
			line-height: 12px;
		}
		.text {
			font-size: 11px;
		}
		.text.value {
			font-size: 10px;
		}
	}

</style>