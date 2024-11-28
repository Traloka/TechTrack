<script>
    export let showTooltipFlag;
    export let tooltipData;
    export let adjustedTooltipPosition;
    export let formattedLaunchDate;
  </script>
  
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
  </style>
  