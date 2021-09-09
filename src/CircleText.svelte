<!-- https://www.developintelligence.com/blog/2016/09/how-to-make-circularcurved-text-with-javascript/ -->

<script>
  import { getRandomSubarray } from './utils.js'

  export let radius = 0
  export let characters = []
  export let colors = []
  export let offset = 0
  export let opacity = 0

  characters = getRandomSubarray(characters, 12)

  let origin = 0;
</script>

  <!--the holders/targets for the text, reuse as desired-->
  <div class="circTxt" id="test">
    {#each characters as char, i}
      <p style="
          height: {radius}%;
          top: calc(50% - {radius}%);
          left: 50%;
          position:absolute;
          transform:rotate({offset + 30 * i}deg);
          margin: 0;
          color: {colors[i % colors.length]};
          transform-origin:0 100%;
          font-size: 10px;
          opacity: {opacity};
          font-weight: 700;
          ">
        {char}
      </p>
    {/each}
  </div>


<style>

  div.circTxt {
    top: 0;
    left: 0;
    position: absolute;
    /*allows for centering*/
    display: inline-block;
    /*adjust as needed*/
    color: red;
    display:table-cell;
    vertical-align:middle;
    height: 100%;
    width: 100%;
    overflow: hidden;
  }
</style>