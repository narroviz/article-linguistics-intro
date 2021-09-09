<script>
	import { stratify, pack, hierarchy } from 'd3-hierarchy'
	import { getContext } from 'svelte';
	import { format } from 'd3-format';
	import { scaleSqrt } from "d3-scale";
	import Circle from './Circle.svelte'

	const { width, height, data } = getContext('LayerCake');

  // export let name = '';
  export let idKey = 'id';
  export let parentKey = undefined;
  export let valueKey = 'value';
	export let fillKey = 'color';
	export let strokeKey = 'stroke';
	export let textColor = '#fff';

	export let sortBy = (a, b) => b.value - a.value; // 'depth' is also a popular choice

  export let circlePadding = 0;



  /* --------------------------------------------
   * This component will automatically group your data
   * into one group if no `parentKey` was passed in.
   * Stash $data here so we can add our own parent
   * if there's no `parentKey`
   */
  let parent = {};
  $: dataset = $data;

  $: if (parentKey === undefined) {
    parent = { [idKey]: 'all' };
    dataset = [...dataset, parent]
  }

	$: stratifier = stratify()
		.id(d => d[idKey])
    .parentId(d => {
      if (d[idKey] === parent[idKey]) return '';
      return d[parentKey] || parent[idKey];
    });

	$: packer = pack()
		.size([$width, $height])
		.radius(d=> {
		  var sizeScale = scaleSqrt()
		  	.domain([0, 15555])
		  	.range([1.5,.11*$height]);
			return sizeScale(d.data.data[valueKey])
		})
		.padding(circlePadding);

	$: stratified = stratifier(dataset);

	$: root = hierarchy(stratified)
		.sum((d, i) => {
			return d.data[valueKey] || 1;
		})
		.sort(sortBy);

	$: packed = packer(root);

  $: descendants = packed.descendants();


	const titleCase = d => d.replace(/^\w/, w => w.toUpperCase());
	const commas = format(',');
</script>

<div class="circle-pack" data-has-parent-key="{parentKey !== undefined}">
	{#each descendants as d}
  	<Circle
  		x="{d.x}"
  		y="{d.y}"
  		r="{d.r}"
  		stroke="{d.data.data[strokeKey]}"
  		fill="{d.data.data[fillKey]}"
  		hover="{d.data.data[strokeKey]}"
  		textColor="{textColor}"
  		label="{commas(d.data.data[valueKey])}"
  		id="{titleCase(d.data.id)}"
  	/>
	{/each}
</div>

<style>
	.circle-pack {
		position: relative;
		width: 100%;
		height: 100%;
	}


</style>
