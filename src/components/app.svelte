<!-- <script>
    import Map from '../Map.svelte';
  
    let tooltipData = null;
    let showTooltipFlag = false;
    let tooltipPosition = { x: 0, y: 0 };
    let adjustedTooltipPosition = { x: 0, y: 0 };
    let formattedLaunchDate = '';
  
    function showTooltip(event, data) {
      tooltipData = data;
      showTooltipFlag = true;
      tooltipPosition = { x: event.pageX + 10, y: event.pageY + 10 };
    }
  
    function hideTooltip() {
      showTooltipFlag = false;
    }
  
    function adjustTooltipPosition(x, y, tooltipWidth, tooltipHeight) {
      const windowWidth = window.innerWidth;
      const windowHeight = window.innerHeight;
  
      if (x + tooltipWidth > windowWidth) x = windowWidth - tooltipWidth - 10;
      if (x < 0) x = 10;
      if (y + tooltipHeight > windowHeight) y = windowHeight - tooltipHeight - 10;
      if (y < 0) y = 10;
  
      return { x, y };
    }
  
    $: if (showTooltipFlag && tooltipData) {
    const tooltipElement = document.querySelector(".tooltip"); 
    let tooltipRect;
    if (tooltipElement) {
    tooltipRect = tooltipElement.getBoundingClientRect();
    } else {
    tooltipRect = { width: 300, height: 100 };     
    }
  
      adjustedTooltipPosition = adjustTooltipPosition(
        tooltipPosition.x,
        tooltipPosition.y,
        tooltipRect.width,
        tooltipRect.height
      );
  
      const launchDate = new Date(tooltipData.net);
      formattedLaunchDate = launchDate.toLocaleString('en-US', {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
        hourCycle: 'h24',
        timeZoneName: 'short',
      });
    }
  </script>
  
  <main>
    <Map onShowTooltip={showTooltip} onHideTooltip={hideTooltip} />
    
    <div class="title-container">
        <h1 id="title">Rocket Launch Map</h1>
      </div>
    
      <div id="map"></div>
    
      <ul>
          <li>
              <div class="legend-color" style="background-color: green;"></div>
              Ready for launch
          </li>
    
          <li>
              <div class="legend-color" style="background-color: orange;"></div>
              Confirmed
          </li>
    
          <li>
              <div class="legend-color" style="background-color: red;"></div>
              Determined
          </li>
      </ul>

    {#if showTooltipFlag && tooltipData}
    <section
      class="tooltip"
      style="top: {adjustedTooltipPosition.y}px; left: {adjustedTooltipPosition.x}px;"
    >
      <h2>{tooltipData.name}</h2>
      <p>Status: {tooltipData.status.name}</p>
      <p>Rocket: {tooltipData.rocket.configuration.full_name}</p>
      <p>Agency: {tooltipData.launch_service_provider.name}</p>
      <p>Location: {tooltipData.pad.location.name}</p>
      <p>Launch Date: {formattedLaunchDate}</p>
      <img src="{tooltipData.image.thumbnail_url}" alt="{tooltipData.name}" width="150">
    </section>
    {/if}
  </main>
  
  <style>
    .tooltip {
      position: absolute;
      background-color: rgba(255, 255, 255, 0.706);
      border: 1px solid #ddd;
      padding: 10px;
      border-radius: 4px;
      pointer-events: none;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      max-width: 300px;
      word-wrap: break-word;
    }
  </style> -->