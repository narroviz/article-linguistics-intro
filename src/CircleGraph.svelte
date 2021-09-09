<script>
	import { LayerCake, Html } from 'layercake';

	import CirclePack from './CirclePack.svelte';

  export let data = []

	const idKey = 'id';
	const valueKey = 'count';
	const fillKey = 'fill';
	const strokeKey = 'stroke';

	data.forEach(d => {
		d[valueKey] = +d[valueKey];
	});
</script>

<div class="chart-container">
	<LayerCake
		padding={{ top: 0, bottom: 0, left: 0 }}
		data={data}
	>
		<Html>
      <CirclePack
        idKey={idKey}
        valueKey={valueKey}
				fillKey={fillKey}
				strokeKey={strokeKey}
				textColor="#fff !important"
      />
		</Html>
	</LayerCake>
</div>

<style>
	/*
		The wrapper div needs to have an explicit width and height in CSS.
		It can also be a flexbox child or CSS grid element.
		The point being it needs dimensions since the <LayerCake> element will
		expand to fill it.
	*/
	.chart-container {
		z-index: 999999;
		width: 100%;
		height: 100%;
	}
</style>
