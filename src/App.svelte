<script>
	import { select, selectAll } from "d3-selection"
	import { scaleLinear } from "d3-scale";
  import { Swiper, SwiperSlide } from 'swiper/svelte';
  // Import Swiper styles
	import 'swiper/css';

	import "swiper/css/navigation"
	import "swiper/css/pagination"
	import "swiper/css/a11y"
	import "swiper/css/effect-fade"

	import SwiperCore, { Navigation, Pagination, Keyboard, A11y, EffectFade } from 'swiper';

  import Scatterplot from "./Scatterplot.svelte"
  import Header from "./Header.svelte";
  import Intro from "./Intro.svelte";
  import CircleTextBackground from "./CircleTextBackground.svelte"
  import CircleGraph from "./CircleGraph.svelte"
  import { getRandomArbitrary, dynamicSort, numberWithCommas, get, formatDecimalAsPercentage } from "./utils.js"

  import phonemeCounts from '../public/assets/data/phoneme_counts.js';
  import graphemeCounts from '../public/assets/data/grapheme_counts.js';
  import phonemeToGrapheme from '../public/assets/data/phoneme_to_grapheme.js';


  SwiperCore.use([EffectFade,Navigation,Pagination,Keyboard,A11y]);

  let sortedPhonemes = phonemeCounts.sort(dynamicSort("-count"))
  let activeData = {}

  let iteration = 0
  let spacer = 4

  const getFontSize = (adjustableString) => {
  	if (window.innerWidth > 650) {
  		return '40px'
  	}

  	if (adjustableString.length > 12) {
  		return '23px';
  	} else if (adjustableString.length >= 8) {
  		return '28px';
  	} else {
  		return '40px';
  	}
  }

  const getGraphemeData = (phoneme, fill, stroke) => {
    let graphemes = phonemeToGrapheme[phoneme]
    let sortedGraphemes = graphemes.sort(dynamicSort("-count"))

    let xTotal = 0
    const rScale = scaleLinear()
	    .domain([0, 15555])
	    .range([2, 24]);

    return sortedGraphemes
      .map((graphemeData, i) => {
      	let probability = graphemeData['count']
      	let radius = rScale(probability)
  	    let x = xTotal + radius
		    let y = 0
		    let data = Object.assign({}, graphemeData, {phoneme});

		    xTotal += (radius + radius + spacer)
		    // console.log(xTotal)
      	return [x, y, radius, fill, stroke, data]
      })
  }
  let activeIndex = 0;
  const updateActiveIndex = cb => activeIndex = cb(activeIndex)

</script>


<!-- <Header/> -->

<main>

	<div style="height:calc(100%); width: 100%; position: absolute; overflow: hidden; top: 0; pointer-events: none;">
			<CircleTextBackground bind:activeIndex={activeIndex}/>
	</div>
	<div style="height:100%; max-width: 1200px; margin: auto;">
		<Swiper effect="{'fade'}" pagination="{true}" allowTouchMove={false} initialSlide={0} navigation="{true}" fadeEffect="{{crossFade: true}}" a11y="{true}" keyboard="{true}" class="swiper" on:slideChange={(e) => updateActiveIndex(val => e.detail[0][0].activeIndex)}>

	  	<SwiperSlide>
	  		<Intro/>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
		  		<div class="title">What do all these words have in common?</div>

		  		<div class="word">away</div>
		  		<div class="word">they</div>
		  		<div class="word">sleigh</div>
		  		<div class="word">cafe</div>
		  		<div class="word">matinee</div>
		  		<div class="word">cabaret</div>
		  	</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
		  		<div class='paragraph'>
		  			Each word rhymes by ending in the same sound, or <span class='link-text'>phoneme</span>, called <span class="highlight-vowel">AY</span>. In this case, the phoneme is a <span class="highlight-vowel">vowel</span> more commonly known as "long a".
		  		</div>

		  		<div class="word">aw-<span class="highlight-vowel">ay</span></div>
		  		<div class="word">th-<span class="highlight-vowel">ey</span></div>
		  		<div class="word">sl-<span class="highlight-vowel">eigh</span></div>
		  		<div class="word">caf-<span class="highlight-vowel">e</span></div>
		  		<div class="word">matin-<span class="highlight-vowel">ee</span></div>
		  		<div class="word">cabar-<span class="highlight-vowel">et</span></div>

		  		<div class='paragraph' style="padding-top: 1rem;">
		  			However, the groups of letters that produce the sound <span class="highlight-vowel">AY</span> are all different. A single letter or group of letters that produces a phoneme is called a <span class='link-text'>grapheme</span>. In the examples, the graphemes are <span class="highlight-vowel">ay</span>, <span class="highlight-vowel">ey</span>, <span class="highlight-vowel">eigh</span>, <span class="highlight-vowel">e</span>, <span class="highlight-vowel">ee</span>, and <span class="highlight-vowel">et</span>.
		  		</div>

		  	</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
		  		<div class="title">Now, what do these words have in common?</div>
		  		<div class="word">cat</div>
		  		<div class="word">cello</div>
		  		<div class="word">ceiling</div>
		  		<div class="word">oceanic</div>
		  		<div class="word">accountancy</div>
		  	</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
		  		<div class='paragraph'>
		  			Each word contains the same <span class='bold'>grapheme</span>, <span class="highlight-consonant">c</span>. This single-letter grapheme, however, is responsible for different sounds, or phonemes, in each word.
		  		</div>

		  		<div class="word"><span class="highlight-consonant">c</span>-at</div>
		  		<div class="word"><span class="highlight-consonant">c</span>-ello</div>
		  		<div class="word"><span class="highlight-consonant">c</span>-eiling</div>
		  		<div class="word">o-<span class="highlight-consonant">c</span>-eanic</div>
		  		<div class="word">accountan-<span class="highlight-consonant">c</span>-y</div>

		  		<div class='paragraph' style="padding-top: 1rem;">
		  			Each highlighted <span class="highlight-consonant">c</span> corresponds to a different <span class='bold'>phoneme</span> (<span class="highlight-consonant">K</span>, <span class="highlight-consonant">CH</span>, <span class="highlight-consonant">S</span>, <span class="highlight-consonant">SH</span>, <span class="highlight-mixed">T:S</span>). In this case, the phonemes in blue are <span class="highlight-consonant">consonants</span>. And the purple phoneme,<span class="highlight-mixed">T:S</span>, is a consonant <span class="highlight-mixed">diphoneme</span>, or merger of 2 phonemes.
		  		</div>

		  	</div>
		  </SwiperSlide>
		  <SwiperSlide>
		  	<div class='slide-container'>
		  		<div class='paragraph'>
		  			In the previous examples, we saw a phoneme can map to multiple graphemes:
		  		</div>
	  			<div class="grammar-unit-container" style="padding-bottom: 1rem;">
		  			<span class="highlight-vowel">AY</span>&nbsp;&nbsp;&rarr;&nbsp;&nbsp;<span class="highlight-vowel">ay</span>,&nbsp;&nbsp;<span class="highlight-vowel">ey</span>,&nbsp;&nbsp;<span class="highlight-vowel">eigh</span>,&nbsp;&nbsp;<span class="highlight-vowel">e</span>,&nbsp;&nbsp;<span class="highlight-vowel">ee</span>,&nbsp;&nbsp;<span class="highlight-vowel">et</span>
			  	</div>
			  	<div class='paragraph'>
		  			Vice versa, we saw a grapheme can map to multiple phonemes:
		  		</div>
	  			<div class="grammar-unit-container" style="padding-bottom: 1rem;">
		  			<span class="highlight-consonant">c</span>&nbsp;&nbsp;&rarr; &nbsp;&nbsp;<span class="highlight-consonant">K</span>,&nbsp;&nbsp;<span class="highlight-consonant">CH</span>,&nbsp;&nbsp;<span class="highlight-consonant">S</span>,&nbsp;&nbsp;<span class="highlight-consonant">SH</span>,&nbsp;&nbsp;<span class="highlight-mixed">T:S</span>
		  		</div>
			  	<div class='paragraph'>
		  			But where does this knowledge come from? How many phonemes are there? How many graphemes? And how many mappings are there between graphemes and phonemes?
		  		</div>
		  	</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>

		  		<div class='paragraph'>
		  			To answer these questions, I looked at ~35K English words from the <a class="link-text" href="http://www.speech.cs.cmu.edu/cgi-bin/cmudict" target="_blank">CMU Pronouncing Dictionary</a>. Its data helps break words into their graphemes and phonemes, like the word <span class="bold">box</span>:
		  		</div>

		  		<div style="padding-bottom: 1.5rem;">
			  		<div class="grammar-unit-container">
			  			<p class="grammar-unit-label">Graphemes</p>
			  			<p class="grammar-unit"><span class="highlight-consonant">B</span></p>|<p class="grammar-unit"><span class="highlight-vowel">O</span></p>|<p class="grammar-unit"><span class="highlight-mixed">X</span></p>
			  		</div>
			  		<div class="grammar-unit-container">
			  			<p class="grammar-unit-label">Phonemes</p>
			  			<p class="grammar-unit"><span class="highlight-consonant">B</span></p>|<p class="grammar-unit"><span class="highlight-vowel">AA</span></p>|<p class="grammar-unit"><span class="highlight-mixed">K:S</span></p>
		  			</div>
		  		</div>

		  		<div class='paragraph'>
		  			Silent letters, like the <span class="highlight-consonant">c</span> in <span class="bold">czar</span>, become part of the nearest sound-producing grapheme.
		  		</div>
		  		<div>
			  		<div class="grammar-unit-container">
			  			<p class="grammar-unit-label">Graphemes</p>
			  			<p class="grammar-unit"><span class="highlight-consonant">C</span><span class="highlight-consonant">Z</span></p>|<p class="grammar-unit"><span class="highlight-vowel">A</span></p>|<p class="grammar-unit"><span class="highlight-consonant">R</span></p>
			  		</div>
			  		<div class="grammar-unit-container">
			  			<p class="grammar-unit-label">Phonemes</p>
			  			<p class="grammar-unit"><span class="highlight-consonant">Z</span></p>|<p class="grammar-unit"><span class="highlight-vowel">AA</span></p>|<p class="grammar-unit"><span class="highlight-consonant">R</span></p>
		  			</div>
		  		</div>

		  		<div class='paragraph' style="padding-top: 1.5rem;">
		  			 For more info on the data methodology, check out the accompanying <a href="https://github.com/narroviz/data-linguistics" target="_blank" class="link-text">Github repo</a>.
		  		</div>
		  	</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
		  		<div class="title">39 Phonemes</div>
		  		<div class='paragraph'>
		  			For its phonemes, the CMU Pronouncing Dictionary modified an alphabet of sounds known as <a class="link-text" href="https://en.wikipedia.org/wiki/ARPABET" target="_blank">ARPAbet</a>. Its version has 15 <span class="highlight-vowel">vowels</span>, 24 <span class="highlight-consonant">consonants</span> (39 total). It also has 21 <span class="highlight-mixed">diphonemes</span>.

		  		</div>
	  			<div class="circle-graph-container">
	  				<CircleGraph data={phonemeCounts}/>
	  			</div>
		  	</div>

	  	</SwiperSlide>
	  	<SwiperSlide>
	  		<div class='slide-container'>
	  			<div class="title">275 Graphemes</div>
	  			<div class='paragraph'>
		  			Due to silent letters, there are far more graphemes. By counting the mappings in the ~35K English words, we can find the following counts: 33 <span class="highlight-vowel">vowel</span> graphemes, 101 <span class="highlight-consonant">consonant</span> graphemes and 141 <span class="highlight-mixed">mixed</span>.
		  		</div>
	  			<div class="circle-graph-container">
	  				<CircleGraph data={graphemeCounts}/>
	  			</div>
	  		</div>
	  	</SwiperSlide>
	  	<SwiperSlide>
	  		
	  		<div class='slide-container'>
	  			<div class="title" style="padding-top: 30px;">Phoneme-to-Grapheme Mappings</div>

			  	<div class="table-top-container">
				  	<div class="phoneme-title">Phoneme</div>
				  	<div class="count-title">Count</div>
				  	<div class="grapheme-title">Graphemes</div>
					</div>	
					<div class="table-top-line"/>

					<div class="first">
						<div class="second">
							<table>
							{#each sortedPhonemes as {id, count, fill, light, stroke}}
			        	<tr>
			        		<td class="headcol"><p style="color: {fill}; font-weight: 500;">{id}</p></td>
			        		<td class="headcol final"><p style="color: {fill}; font-family: 'Avenir';">{numberWithCommas(count)}</p></td>
			        		<td {activeData}><Scatterplot {spacer} data={getGraphemeData(id, fill, stroke)} bind:activeData={activeData}/></td>
			        {/each}
		    			</table>
		    		</div>
		    	</div>

		    	
				    <div class="table-detail-container" >
				    	<div style="min-width: 70px; width: 20%; display: flex; flex-direction: column;">
				    		<div style="height: 50%; justify-content: center; display: flex; flex; flex-direction: column; border-bottom: 1px solid #dad9d9; border-right: 1px solid #dad9d9;">
				    			<p style="margin: auto; width: 100%; font-weight: 500; color: {get(activeData, 'stroke', 'white')};">{get(activeData, 'phoneme', '&nbsp;')}</p>
				    			<p class="subtitle">PHONEME</p>
				    		</div>
				    		<div style="height: 50%; justify-content: center; display: flex; flex; flex-direction: column; border-right: 1px solid #dad9d9;">
				    			<p style="margin: auto; width: 100%; font-weight: 500; color: {get(activeData, 'stroke', 'white')};">{get(activeData, 'grapheme', '&nbsp;').toLowerCase()}</p>
				    			<p class="subtitle">GRAPHEME</p>
				    		</div>
				    	</div>

				    	<div style="display: flex; width: 60%">
				    		{#if Object.keys(activeData).length > 0}
					    		<p style="margin: auto; font-size: {getFontSize(get(activeData, 'example', []).join('').replace('*', ''))}">
					    		{#each get(activeData, 'example', '') as substring}
						    			{#if substring.includes('*')}
						    				<span style="font-weight: 500; color: {get(activeData, 'stroke', '')};">{substring.replaceAll('*', '').toLowerCase()}</span>
						    			{:else}
						    				<span style="color: rgb(169,169,169); font-weight: 200;">{substring.toLowerCase()}</span>
						    			{/if}
						    	{/each}
						    	</p>
						    {:else}
						    	<div style="width: 100%; height: 100%; display:flex; flex-direction: column;">
							    	
							    	<div style="width: 100%; height: 70%; display:flex; flex-direction: column;" >
							    		<div style="width: 100%; height: 20%; display: -webkit-box; -webkit-box-align: end; font-size: 10px; font-weight: 500; text-align: center;">
							    			<div style="margin-left:auto; margin-right:auto"># Occurrences in 35K Word Corpus</div>
							    		</div>
								    	<div style="width: 100%; height: 55%; display:flex; flex-direction: row; justify-content: center;">
								    		<div style="width: 50px; padding-right: 5px; display: -webkit-box; -webkit-box-align: end;">
								    			<div style="margin: 0; margin-left: auto; margin-right: auto; width: 50px; height: 50px; border-radius: 100px; border: 1px solid rgb(169, 169, 169);"/>
								    		</div>
								    		<div style="width: 40px; padding-left: 5px; padding-right: 5px; display: -webkit-box; -webkit-box-align: end;">
									    		<div style="margin: 0; margin-left: auto; margin-right: auto; width: 30px; height: 30px; border-radius: 100px; border: 1px solid rgb(169, 169, 169);"/>
									    	</div>
								    		<div style="width: 30px; padding-right: 5px; display: -webkit-box; -webkit-box-align: end;">
								    			<div style="margin: 0; margin-left: auto; margin-right: auto; width: 20px; height: 20px; border-radius: 100px; border: 1px solid rgb(169, 169, 169);"/>
								    		</div>
								    		<div style="width: 20px; padding-right: 5px; display: -webkit-box; -webkit-box-align: end;">
								    			<div style="margin: 0; margin-left: auto; margin-right: auto; width: 9px; width: 9px; height: 9px; border-radius: 100px; border: 1px solid rgb(169, 169, 169);"/>
								    		</div>
								    	</div>
								    	<div style="width: 100%; height: 20%; display:flex; flex-direction: row; justify-content: center; align-items: center;">
								    		<div style="width: 50px; padding-right: 5px;">
								    			<p style="margin: auto; height: 10px; font-size: 10px;">15K</p>
								    		</div>
								    		<div style="width: 40px; padding-left: 5px; padding-right: 5px;">
								    			<p style="margin: auto; height: 10px; font-size: 10px;">10K</p>
								    		</div>
								    		<div style="width: 30px; padding-right: 5px;">
								    			<p style="margin: auto; height: 10px; font-size: 10px;">5K</p>
								    		</div>
								    		<div style="width: 20px; padding-right: 5px;">
								    			<p style="margin: auto; height: 10px; font-size: 10px;">1K</p>
								    		</div>
								    	</div>
								    	<div style="width: 100%; height: 5%; border-bottom: 1px solid #dad9d9"/>

							    	</div>

							    	<div style="width: 100%; height: 30%; display:flex; flex-direction: column;" >
								    	<div style="width: 100%; height: 50%; -webkit-box-align: end; display: -webkit-box; -webkit-box-pack: center;">
								    		<div style="margin: 0; margin-right: 10px; width: 33%; max-width: 50px; height: 5px; background: rgba(233,131,160); margin-bottom: 3px; display: -webkit-box; -webkit-box-align: end;"/>
								    		<div style="margin: 0; width: 33%; max-width: 50px; height: 5px; background: rgba(91,143,205); margin-bottom: 3px; display: -webkit-box; -webkit-box-align: end;"/>
								    		<div style="margin: 0; margin-left: 10px; width: 33%; max-width: 50px;  height: 5px; background: #c480e5; margin-bottom: 3px; display: -webkit-box; -webkit-box-align: end;"/>
								    	</div>
								    	<div style="width: 100%; ; height: 50%; display:flex; flex-direction: row; justify-content: center; align-items: center;">
								    		<p style="margin: 0; margin-right: 10px; width: 33%; max-width: 50px; height: 15px; font-size: 9px; font-weight: 500;">Vowel</p>
								    		<p style="margin: 0; width: 33%; max-width: 50px; height: 15px; font-size: 9px; font-weight: 500;">Consonant</p>
								    		<p style="margin: 0; margin-left: 10px; width: 33%; max-width: 50px;  height: 15px; font-size: 9px; font-weight: 500;">Diphoneme</p>
								    	</div>
							    	</div>


						    	</div>

						    {/if}
					    	
				    	</div>

				    	<div style="min-width: 70px; width: 20%">
				    		<div style="height: 50%; justify-content: center; display: flex; flex-direction: column; border-bottom: 1px solid #dad9d9; border-left: 1px solid #dad9d9;">
				    			<p style="margin: auto; width: 100%; font-weight: 500; color: {get(activeData, 'stroke', 'white')};">{formatDecimalAsPercentage(get(activeData, 'probability', '&nbsp;'), 1)}</p>
				    			<p class="subtitle">FREQUENCY</p>
				    		</div>
				    		<div style="height: 50%; justify-content: center; display: flex; flex; flex-direction: column; border-left: 1px solid #dad9d9;">
				    			<p style="margin: auto; width: 100%; font-weight: 500; color: {get(activeData, 'stroke', 'white')};">{numberWithCommas(get(activeData, 'count', '&nbsp;'))}</p>
				    			<p class="subtitle">COUNT</p>
				    		</div>
				    	</div>
				    </div>
	  		</div>
	  	</SwiperSlide>

	 
	  </Swiper>
	 </div>
</main>

<style>


	.subtitle {
		margin: auto;
		width: 100%;
		font-weight: 500;
		color: rgb(169,169,169);
		font-size: 10px;
	}


  table { 
  	border-spacing:0;
  	background: white;
  }
  td {
    margin:0;
  	height:  50px;  
  	padding-top: 7px;
  	padding-bottom: 7px;
    border-bottom: 1px solid #dad9d9; 
    white-space:nowrap;
  }
  div.first {
    width: 100%;
    overflow-y: scroll;
    overflow-x: hidden;
    height: 100%;           
    /*padding-bottom: 1px;*/
    position: relative;
    left:0;
    top:auto;
  }
  div.second { 
    width: calc(100% - 180px); 
    overflow-x:scroll;  
    margin-left: 180px; 
    overflow-y:visible;
    /*padding-bottom:1px;*/
    height: auto;
  }

  .headcol {
		background: white;
    position: absolute; 
    width:90px; 
    left:0;
    top:auto;
    text-align: center;
  	vertical-align: middle;
    padding-left: 0;
    padding-right: 0;

  }
  .headcol.final {
		left:90px;
    box-shadow: 4px 0 4px -2px #c9c9c9;

    -webkit-transform: translate3d(0, 0, 0);
            transform: translate3d(0, 0, 0);
    -webkit-box-shadow: 4px 0 4px -2px #c9c9c9;
            box-shadow: 4px 0 4px -2px #c9c9c9;
  }

  .headcol p {
  	margin: 0;
  	height:  50px;
  	width:  90px;
  	text-align: center;
  	vertical-align: middle;
  	display:table-cell;
  }

  .table-detail-container {
  	z-index: 999999;
  	display: flex;
  	height: 20%;
  	min-height: 150px;
  	flex-direction: row;
  	width: 100%;
  	background: white;
		border-top: 1px solid rgb(169,169,169);

    box-shadow: 0 -4px 4px -2px #c9c9c9;
	  -webkit-transform: translate3d(0, 0, 0);
	          transform: translate3d(0, 0, 0);
	  -webkit-box-shadow: 0 -4px 4px -2px #c9c9c9;
	          box-shadow: 0 -4px 4px -2px #c9c9c9;
  }

  .table-top-line {
  	z-index: 999999;
  	width: 100%;
  	background: white;
  	height:  8px;
		border-bottom: 1px solid rgb(169,169,169);
    box-shadow: 0px 4px 4px -2px #c9c9c9;
	  -webkit-transform: translate3d(0, 0, 0);
	          transform: translate3d(0, 0, 0);
	  -webkit-box-shadow: 0px 4px 4px -2px #c9c9c9;
	          box-shadow: 0px 4px 4px -2px #c9c9c9;
  }

  .table-top-container {
  	z-index: 1000;
  	display: flex;
  	height: 15px;
  	flex-direction: row;
  	width: 100%;
  	justify-content: end;
  	background: white;

  }

  .phoneme-title {
  	color: rgb(169,169,169);
  	font-size: 14px;
  	font-weight: 500;
  	width: 90px;
  	margin: auto;
  	margin-bottom: 0;
  	margin-top: 0;
  }
  .count-title {
  	color: rgb(169,169,169);
  	font-size: 14px;
  	font-weight: 500;
  	width: 90px;
  	margin: auto;
  	margin-bottom: 0;
  	margin-top: 0;
  }

  .grapheme-title {
  	color: rgb(169,169,169);
  	font-weight: 500;
  	font-size: 14px;
  	width:  calc(100% - 180px);
  	text-align: center; 
  	margin: auto;
  	margin-bottom: 0;
  	margin-top: 0;
  }

	.circle-graph-container {
		font-size: 10px;
		height: 400px;
		width: 400px;
		margin: 0 auto;
	}


	@media only screen and (max-width: 600px) {
		.circle-graph-container {
			height: 250px;
			width: 250px;
		}
		div.second { 
			width: 100%; 
			margin-left: 100px;
		}
		.headcol {
      position: absolute; 
      width: 50px;
      font-size: 12px;
    }
	  .headcol.final {
			left:50px;
			width: 50px;
			font-size: 12px;
		}
    .grapheme-title {
    	font-size: 10px;
    }
    .phoneme-title {
    	font-size: 10px;
    	width: 50px;
    	margin: 0;
    }
    .count-title {
    	font-size: 10px;
    	width: 50px;
    	margin: 0;
    }
    .table-top-line {
  		width: 100vw;
  	}
    .table-detail-container {
    	width: 100vw;
			border-radius: 0px;
			border: none;
			background: white;
			position: absolute;
			bottom: 0;
    }
    .subtitle {
    	font-size: 8px;
    }
	}

	.bold {
		font-weight: 500;
	}

	.grammar-unit-container {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		height: 30px;
		width: 100%;
	}

	.grammar-unit {
		width: 30px;
		padding-left: 10px;
		padding-right: 10px;
	}

	.grammar-unit-label {
		width: 30%;
		padding-right: 10px;
		text-align: right;
		font-size: 14px;
		font-weight:  500;
		color: rgb(169,169,169);
	}

	.slide-container {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		height:  100%;
		max-width: 80%;
		padding-left: 30px;
		padding-right: 30px;
	}

	.paragraph {
		max-width: 600px;
 		padding-bottom: 1.5rem;
 		font-size: 18px;
 		font-family: 'Avenir';
	}

	.title {
		font-size: 1.75rem;
		font-weight: 500;
		padding-bottom: 2rem;
	}

	.word {
		padding-bottom: .75rem;
		width: 100%;
		font-size: 1.2rem;
		font-family: "Avenir";
	}

	.highlight-vowel {
		color: rgba(233,131,160);
		font-weight: 500;
	}

	.highlight-consonant {
		color: rgba(91,143,205);
		font-weight: 500;
	}

	.highlight-mixed {
		color: #c480e5;
		font-weight: 500;
	}

	.link-text {
		font-weight: 500;
		border-bottom: 1px solid black;
		color: black;
	}

	.link-text:hover {
		cursor: pointer;
		text-decoration: none; 
		color: #04A933;
		font-weight: 700px;
		border-bottom: 2px solid white;
		background: linear-gradient(135deg, rgba(91,143,205,1) 0%, rgba(196,128,229,1) 50%, rgba(233,131,160,1) 100%);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		border-image: linear-gradient(135deg, rgba(91,143,205,1) 0%, rgba(196,128,229,1) 50%, rgba(233,131,160,1) 100%)
	              0 0 100% 0/ 0 0 2px 0 stretch;
	}


	@media only screen and (max-width: 350px) {
		
		.title {
			font-size: 1.35rem;
		}
		.paragraph {
			font-size: 13px;
		}

		.word {
			font-size: 13px;
		}

	}

	/*
	********************
	       Raleway
	********************
	*/

	@font-face {
	    font-family: 'Raleway';
	    src: url('/assets/fonts/raleway/Raleway-extralight.woff');
	    font-weight: 200;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	@font-face {
	    font-family: 'Raleway';
	    src: url('/assets/fonts/raleway/Raleway-medium.woff');
	    font-weight: 400;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	@font-face {
	    font-family: 'Raleway';
	    src: url('/assets/fonts/raleway/Raleway-heavy.woff');
	    font-weight: 700;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	/*
	********************
	       Avenir
	********************
	*/

	@font-face {
	    font-family: 'Avenir';
	    src: url('/assets/fonts/avenir/Avenir-Light.woff');
	    font-weight: 400;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	@font-face {
	    font-family: 'Avenir';
	    src: url('/assets/fonts/avenir/Avenir-Medium.woff');
	    font-weight: 500;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	@font-face {
	    font-family: 'Avenir';
	    src: url('/assets/fonts/avenir/Avenir-Heavy.woff');
	    font-weight: 700;
	    font-style: normal;
	    font-stretch: normal;
	    font-display: swap;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}


</style>