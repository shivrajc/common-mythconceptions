<script>
  import CirclePackTooltip from './CirclePackTooltip.svelte';
  import data from './data.js'
  import { pack, interpolateOrRd, scaleSequential, max } from "d3";
  import { format, formatPrefix } from "d3";

  const width = 800;
  const height = 750;
  
  const formatMN = formatPrefix(",.0", 1e6);
  const formatK = format("~s");
  
  const {root, descendants, leaves, onMouseOver, onMouseLeave, hoveredData, eventData, updateSelectedData, colorData, selectedData} = $props();  

  // const categories = data.filter(d => d.parent === "category").map(c => c.child);

  const colorScale = scaleSequential()
    .domain([0, max(data, d => d.google_hits)])
    .interpolator(interpolateOrRd);

  root.sum(d => d.google_hits);  

  const packGenerator = pack()
    .size([width, height])
    .padding(4);

  packGenerator(root);

</script>

<svg {width} {height}>
  {#each descendants as desc}
    <circle
      cx={desc.x}
      cy={desc.y}
      r={desc.r}      
      stroke-width="{desc.strokeWidth}"
      onmousemove="{(e) => onMouseOver(e, desc)}"
      onmouseleave="{onMouseLeave}"
      class:hovered={hoveredData && hoveredData.id === desc.id && hoveredData.depth > 1}      
      onclick="{() => updateSelectedData(desc)}"
      fill={desc.fill}      
      stroke={desc.stroke}      
      class:selected={selectedData.id === desc.id}
    >
    </circle>
    {#if desc.id === "science"}
      <text
        x={desc.x - desc.r * 0.6}
        y={desc.y - 120}
        class="science-label category-label"
        fill="{desc.textColor}"
        >
        {desc.id}
      </text>
      {/if}
      {#if desc.id === "mind"}
      <text
        x={desc.x + desc.r * 0.4}
        y={desc.y}
        class="mind-label category-label"
        fill="{desc.textColor}"
      >
        {desc.id}
      </text>
    {/if}
    {#if desc.id === "religion"}
      <text
        x={desc.x + desc.r * 0.01}
        y={desc.y + desc.r * 0.4}
        class="religion-label category-label"
        fill="{desc.textColor}"
      >
        {desc.id}
      </text>
    {/if}
    {#if desc.depth === 2 && desc.r >= 50}
      <text
        x={desc.x}
        y={desc.y-12}
        class="item-label"
        text-anchor="middle"
        dominant-baseline="middle"
        fill="{desc.textColor}"
      >
        {desc.id}
      </text>
      <text
        x={desc.x}
        y={desc.y + 16}
        class="item-value-label"
        text-anchor="middle"
        dominant-baseline="middle"
        fill="{desc.textColor}"
      >
        {desc.value < 1000000 ? formatK(desc.value) : formatMN(desc.value)}
      </text>

    {/if}
  {/each}
</svg>
{#if hoveredData && hoveredData.id != "category"}
    <CirclePackTooltip data={hoveredData} {eventData} />
{/if}

<style>
  svg {
    overflow: visible;
  }
  
  .hovered {
    stroke-width: 2;
    cursor: pointer;
  }  

  .selected {
    stroke-width: 2;
    stroke: black;
  }

  text {
    pointer-events: none;
  }

  .category-label {
    opacity: 0.8;
    font-weight: 500;
    text-transform: capitalize;
  }

  .science-label {
    font-size: 32px;
  }

  .mind-label {
    font-size: 20px;
  }

  .religion-label {
    font-size: 16px;
  }

  .item-label {
    font-size: 16px;
    font-weight: 600;
    opacity: 0.6;
  }
  
  .item-value-label {
    font-size: 1.4rem;
    font-weight: bold;
    opacity: 0.6;
  }
</style>