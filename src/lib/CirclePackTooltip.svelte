<script>
  import { format, formatPrefix } from "d3";  
  import { fade } from 'svelte/transition';
  
  const formatMN = formatPrefix(",.0", 1e6);
  const formatK = format("~s");
  let { data, eventData } = $props();

</script>


<div style="top: {eventData.offsetY + 36}px; left: {eventData.offsetX + 36}px"  transition:fade={{ duration: 200 }}>
  {#if data.depth === 1}
    <p>
      <span class="item-label" style:text-transform="capitalize">{data.id} Total</span>
    </p>
    <p>
      <span class="category-label">hits</span>: <span class="category value">{data.value < 1000000 ? formatK(data.value) : formatMN(data.value)}</span>
    </p>
  {:else}
    <p class="item">
      <span class="item-label">{data.id}</span>
    </p>
    <p>
      <span class="label">Category: <span class="value">{data.parent.id}</span></span>
    </p>
    <p>
      <span class="label">Hits: <span class="value">{data.value < 1000000 ? formatK(data.value) : formatMN(data.value)}</span></span>
      <!-- <span class="sub-label">{data.data.remaining_text}</span> -->
    </p>
  {/if}
</div>

<style>
  div {
    position: absolute;    
    background-color: white;
    opacity: 0.9;
    padding: 8px 16px;
    z-index: 1000;
    pointer-events: none;
    border-radius: 4px;
    max-width: 600px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 3px 6px, rgba(0, 0, 0, 0.23) 0px 3px 6px;
  }
  p {
    margin: 0;
    padding: 0;
  }

  .item {
    display: flex;
    justify-content: left;
    gap: 20px;
    font-size: 1.4rem;
    padding-bottom: 8px;
    border-bottom: 1px solid lightgray;
    margin-bottom: 8px;
    /* padding-bottom: 8px;     */
  }

  .category-label {
    color: "gray";
    text-transform: capitalize;
    font-size: 1.2rem;
    display: inline-block;
    padding-bottom: 8px;
  }

  span.label {
    color: "gray";    
    font-size: 1.2rem;
  }

  span.item-label {
    color: "gray";    
    font-size: 1.4rem;
  }

  span.sub-label {
    font-size: 1.2rem;
  }

  span.value {
    color: var(--slate-gray-700);
    font-weight: bold;
    font-size: 1.4rem;
  }

  span.category {
    font-size: 1.4rem;
  }
</style>