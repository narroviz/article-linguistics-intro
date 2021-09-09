<script>
	import CircleText from "./CircleText.svelte"
  	import { getIntervalArray } from "./utils.js";

  	export let activeIndex = 0;

	let radii = getIntervalArray(20, 100, 4)
	let opacity = [];
	let colors = []
	let phonemesAndGraphemes = ["AA","AE","AH","AH:L","AH:M","AH:R","AH:W","AO","AW","AY","B","CH","D","DH","EH","ER","EY","F","G","G:ZH","G:Z","HH","HH:W","IH","IY","JH","K","K:S","K:SH","L","M","N","NG","NG:K","OW","OY","P","R","S","SH","T","T:S","T:TH","TH","UH","UW","V","W","W:AA","W:AH","W:EY","Y","Y:AE","Y:AH","Y:EH","Y:IH","Y:UH","Y:UW","Z","ZH"]

	const getColors = () => {
		return activeIndex === 0 ? ["rgba(233,131,160)", "rgba(91,143,205)", "rgba(196,128,229)"] : ['gray']
	}
	const getOpacity = () => {
		opacity = radii.map(r => activeIndex === 0 ? (r - 10) / 100 : 0.1)
		return opacity
	}
</script>

{#each radii as radius, i}
	<CircleText {radius} colors={getColors(activeIndex)} characters={phonemesAndGraphemes} opacity={getOpacity(activeIndex)[i]} offset={i % 3 === 0 ? 10 : (i % 3 === 1 ? 20 : 0)}></CircleText>
{/each}