<script>

  import { onMount } from 'svelte';
  import { tick } from 'svelte';

  export let borderColor = '#BBB'; // Hex or rgba
  export let borderWidth = 0; // px
  export let backgroundColor = '#BBB'; // Hex or rgba
  export let pattern = null; // SVG <pattern>
  export let patternId = 'pattern'; // string
  export let width = null; // px
  export let height = null; // px
  export let depth  = 80; // %
  export let roundTop = 50; // %
  export let roundBottom = 50; // %
  export let curveWidth = 50; // px
  export let padding = 0; // px
  export let inversed = false
  export let shiftX = 0;
  export let align = 'center';


  let strokeWidth = borderWidth;
  let container;
  let containerWidth = 0;
  let containerHeight = 0;
  let content;
  let contentWidth = 0;
  let contentHeight = 0;
  let contentTranslationX = 0;

  let plateauX1 = 0;
  let plateauX2 = 0;
  let plateauY = 0;
  let plateauY0 = 0;
  let plateauWidth = 0;
  let plateauX1Factor = 0;
  let plateauX2Factor = 0;
  let centerFactor = 0;
  let valleyLeftX = 0;
  let valleyLeftSlopeX1 = 0;
  let valleyLeftSlopeX2 = 0;
  let valleyRightX = 0;
  let valleyRightSlopeX1 = 0;
  let valleyRightSlopeX2 = 0;

  let W = 1000;
  let H = 100;
  let background = null;

  $: centerFactor = (align === 'center') ? 0.5 : (align === 'left') ? 0 : 1 ;
  $: plateauX1Factor = (align === 'center') ? 0.5 : (align === 'left') ? 0 : 1 ;
  $: plateauX2Factor = (align === 'center') ? 0.5 : (align === 'left') ? 1 : 1 ;
  $: plateauWidth = width + padding * 2;
  $: plateauX1 = (W * centerFactor) - (plateauWidth*plateauX1Factor) + shiftX;
  $: plateauX2 = (W * centerFactor) + (plateauWidth*plateauX2Factor) + shiftX;
  $: valleyLeftSlopeX1 = valleyLeftX + (curveWidth * roundBottom / 100);
  $: valleyLeftSlopeX2 = plateauX1 - (curveWidth * roundTop / 100);
  $: valleyLeftX = (plateauX1) - curveWidth;
  $: valleyRightX = (plateauX2) + curveWidth;
  $: valleyRightSlopeX1 = valleyRightX - (curveWidth * roundBottom / 100);
  $: valleyRightSlopeX2 = plateauX2 + (curveWidth * roundTop / 100);
  $: plateauY = (plateauY + strokeWidth);
  $: plateauY0 = (H * depth /100);
  $: background = (pattern)? `url(#`+patternId+`)` : backgroundColor;
  $: W = containerWidth;
  $: H = contentHeight;
  $: contentTranslationX = (align === 'center') ? -50 : (align === 'left') ? 0 : -100 ;

  onMount(async () => {
      await tick();
      resize();
  })

  function resize() {
      containerWidth = container.clientWidth;
      containerHeight = container.clientHeight;
      contentWidth = content.clientWidth;
      contentHeight = content.clientHeight;
      if(width === null) width = contentWidth;
      if(height === null) height = contentHeight;
  }

</script>

<svelte:window on:resize={() => resize()} />

<div class="content" on:click bind:this={container} style="height:{height}px">
  <svg class="shape" class:inversed={inversed} xmlns="http://www.w3.org/2000/svg" viewBox="0 0 {W} {H}" preserveAspectRatio="none" >
    <!-- optional pattern -->
    {#if pattern }
    <defs>
    {@html pattern }
    </defs>
    {/if}

    <!-- background path -->
    <path
        stroke="none"
        fill="{background}"
        d="
          M 0 {plateauY0}
          L {valleyLeftX} {plateauY0}
          C {valleyLeftSlopeX1} {plateauY0} {valleyLeftSlopeX2} {plateauY} {plateauX1} {plateauY}
          L {plateauX2} {plateauY}
          C {valleyRightSlopeX2} {plateauY} {valleyRightSlopeX1} {plateauY0} {valleyRightX} {plateauY0}
          L {W} {plateauY0}
          L {W} {H}
          L {0} {H}
          Z
          "
        />

    {#if borderColor }
    <!-- border path -->
    <path
          fill="none"
          stroke="{borderColor}"
          stroke-width="{strokeWidth}"
          d="
            M 0 {plateauY0}
            L {valleyLeftX} {plateauY0}
            C {valleyLeftSlopeX1} {plateauY0} {valleyLeftSlopeX2} {plateauY} {plateauX1} {plateauY}
            L {plateauX2} {plateauY}
            C {valleyRightSlopeX2} {plateauY} {valleyRightSlopeX1} {plateauY0} {valleyRightX} {plateauY0}
            L {W} {plateauY0}
            "
          />
    {/if}
  </svg>
  <div class="content" bind:this={content} class:left={align==='left'} class:right={align==='right'} style="transform: translateX(calc({contentTranslationX}% + {shiftX}px))">
    <slot>An awesome content 🤩</slot>
  </div>
</div>

<style>
  div.content { width:100%; position:relative; }
  svg.shape { width:100%; height:100%; }
  svg.shape.inversed { transform: scaleY(-1); }
  div.content { position:absolute; top:0; height:auto; width:auto; margin-left:50%; }
  div.content.left { margin-left:0; }
  div.content.right { margin-left:100%; }
</style>