<script>
  import CardTextBadge from "./CardTextBadge.svelte";
  import { color, format, formatPrefix } from "d3";  
  
  const formatMN = formatPrefix(",.0", 1e6);
  const formatK = format("~s");
  const { selectedData } = $props();
</script>

<div class="card" >
  <div class="heading">
    <CardTextBadge label="MYTH" />
    <span class="heading-text">{selectedData.id}</span>
    <span class="sub-heading-label">category: <span class="sub-heading-value">{selectedData.parent.id}</span></span>
  </div>
  <div class="stats" >
    <span class="stat-value">{selectedData.value < 1000000 ? formatK(selectedData.value) : formatMN(selectedData.value)}</span>
    <span class="stat-label">Google Hits</span>
  </div>
  <div class="description">
    <CardTextBadge label="EVIDENCE" />
    <p class="description-text">{selectedData.data.remaining_text}</p>
  </div>
  {#if selectedData.data.source}    
    <div class="links">    
      <a href="{selectedData.data.source}" class="learn-more" target="blank">Learn more</a>
    </div>  
  {/if}
</div>

<style>
  .card {
    /* height: 80%; */
    width: 90%;
    position: relative;
    padding: 40px;
    display: flex;
    flex-direction: column;
    align-items: left;
    gap: 24px;    
    color: inherit;
    border-radius: 0 12px 12px 0 ;
    transition: all 1ms ease-in-out 0s;
  }
  

  .heading {
    display: flex;
    flex-direction: column;
    color: inherit;
  }



  .heading-text {
    font-size: 2rem;
    font-weight: 900;
    padding: 4px 0;
    
  }

  .sub-heading-label {
    font-size: 1.2rem;
  }

  .sub-heading-value {
    font-weight: bold;
  }

  .stats {
    display: flex;
    flex-direction: column;
    padding: 8px 0;
  }

  .stat-value {
    font-size: 3.2rem;
    font-weight: bold;
    line-height: 1.1;
    text-transform: uppercase;
  }
  
  .stat-label {
    font-size: 1.2rem;

  }

  .description {
    display: flex;
    flex-direction: column;
  }
  .description-text {
    font-size: 1.6rem;
    line-height: 1.4;
    padding-top: 4px;
  }

  .learn-more {
    font-size: 1.4rem;
    color: black;
    text-underline-offset: 4px;
  }
</style>