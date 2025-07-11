<script>
  import Card from "./lib/Card.svelte";
  import Chart from "./lib/Chart.svelte";
  import Footer from "./lib/Footer.svelte";
  import Header from "./lib/Header.svelte";
  import data from './lib/data.js';
  import colorData from './lib/color_data.js';
  import { stratify, transition } from "d3";
  import { fade } from 'svelte/transition';

  let hoveredData = $state(null)
  let eventData = $state(null)
  
  const hierarchyGenerator = stratify()
    .id(d => d.child)
    .parentId(d => d.parent);
  
  const root = hierarchyGenerator(data);
  const descendants = root.descendants();
  const leaves = root.leaves();  

  let showToottip = $state(false);

  descendants.forEach(d => {
    switch(d.depth) {
      case 0:
        d.fill = "#f3f4f6";
        d.stroke = "#99a1af";
        d.strokeWidth = 0.6;
        d.textColor = colorData.filter(c => c.category === d.id && c.color_level === "600")[0]?.color_code;
        break;
      case 1:
        d.fill = colorData.filter(c => c.category === d.id && c.color_level === (c.color_name === "yellow" ? "300" : "200"))[0]?.color_code;
        d.stroke = colorData.filter(c => c.category === d.id && c.color_level === (c.color_name === "yellow" ? "600" : "500"))[0]?.color_code;
        d.textColor = colorData.filter(c => c.category === d.id && c.color_level === "700")[0]?.color_code;
        d.strokeWidth = 0.6;
        break;        
      case 2:
        d.fill = colorData.filter(c => c.category === d.parent.id && c.color_level === (c.color_name === "yellow" ? "400" : "300"))[0]?.color_code;
        d.backgroundFill = colorData.filter(c => c.category === d.parent.id && c.color_level === (c.color_name === "yellow" ? "200" : "200"))[0]?.color_code;
        d.stroke = colorData.filter(c => c.category === d.parent.id && c.color_level === "600")[0]?.color_code;
        d.textColor = colorData.filter(c => c.category === d.id && c.color_level === "700")[0]?.color_code;
      d.strokeWidth = 0.6;
      break;        
    }    
  });

  let selectedData = $state(descendants.filter(d => d.depth === 2)[0]);  

  function onMouseOver(eData, hData) {
    hoveredData = hData;
    eventData = eData;
  }

  function onMouseLeave() {
    hoveredData = null;
    eventData = null;
  }

  function updateSelectedData(sData) {
    if (sData.depth === 2) {
      selectedData = sData;
    }
  }

  function selectRandomMyth() {
    const arr = descendants.filter(d => d.depth === 2);
    selectedData = arr[Math.floor(Math.random() * arr.length)];
  }

  selectRandomMyth();

</script>

<main>
    <div class="header">
      <Header />
      <button 
        class="btn-shuffle"
        style:background-color={selectedData.backgroundFill}
        style:color={selectedData.stroke}        
        onclick="{selectRandomMyth}"
        title="Pick a random Myth"
      >
        Random
      </button>
      <img src="src/assets/info.png" alt="" class="info-icon" onmouseover="{() => showToottip = true}" onmouseout="{() => showToottip = false}">
      {#if showToottip}        
        <div class="tooltip" transition:fade={{ duration: 200 }}>
          <span>Bubble size indicates Google hits. Click on the bubbles for more details</span>
          <span>Data source: <strong>Information is beautiful</strong></span>
        </div>
      {/if}
    </div>
    <div class="main-container">
      <div class="chart-container">
        <Chart {root} {descendants} {leaves} {onMouseOver} {onMouseLeave} {hoveredData} {eventData} {updateSelectedData} {colorData} {selectedData}/>
      </div>
        <div class="card" style:background-color={selectedData.backgroundFill}>
          <Card {selectedData} />
        </div>
    </div>
    <div class="footer">
      <Footer />
    </div>
</main>

<style>

  main {
    max-width: 1400px;
    display: grid;
    align-content: center;
    align-content: center;
    grid-template-columns: 1fr;
    grid-template-rows: 160px 1fr 60px;
    grid-template-areas: 
      "header"
      "main"
      "footer"
    ;
    gap: 0px;
    
  }

  .card {
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 0 12px 12px 0;
    /* transition: all 200ms ease-in-out 0s; */
    
  }

  .main-container {
    grid-area: main;
    display: grid;
    grid-template-columns: 1fr 1fr;
    background-color: white;
    grid-template-rows: 1fr;
    border-radius: 12px;
    box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
  }

  .chart-container {
    position: relative;    
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
  }
  .header {
    grid-area: header;
    position: relative;
  }

  .btn-shuffle {
    position: absolute;
    right: 28px;
    bottom: -56px;
    padding: 8px 16px;
    border-radius: 36px;
    cursor: pointer;
    font-weight: bold;
    border: 1.4px solid;
  }
  
  .btn-shuffle:hover {    
    box-shadow: rgba(0, 0, 0, 0.16) 0px 3px 6px, rgba(0, 0, 0, 0.23) 0px 3px 6px;
    
  }
  
  .btn-shuffle:active {    
    transform: scale(0.94);    
  }

  .info-icon {
    position: absolute;
    right: 12px;
    bottom: 32px;
    width: 32px;
    opacity: 0.8;
  }

  .tooltip {
    background-color: white;
    border-radius: 4px;
    position: absolute;
    right: 56px;
    bottom: 44px;
    display: flex;
    flex-direction: column;
    gap: 4px;
    width: 360px;
    padding: 8px;
    border: 1px solid gray;    
  }

  .footer {
    grid-area: footer;
  }
</style>
