<script>
    import { onMount } from 'svelte';
    import Title from '../Title.svelte';
    import Map from '../Map.svelte';
    import Legend from '../Legend.svelte';
    import Tooltip from '../Tooltip.svelte';
  
    let launchData = [];
    let tooltipData = null;
    let showTooltipFlag = false;
    let tooltipPosition = { x: 0, y: 0 };
    let adjustedTooltipPosition = { x: 0, y: 0 };
    let formattedLaunchDate = '';
  
    const statusColors = {
      tbc: 'orange',
      go: 'green',
      tbd: 'red',
    };
  
    async function fetchLaunchData() {
      const response = await fetch(
        'https://ll.thespacedevs.com/2.3.0/launches/?ordering=-last_updated'
      );
      const data = await response.json();
      launchData = data.results;
    }
  
    function showTooltip(event, d) {
      tooltipData = d;
      showTooltipFlag = true;
      tooltipPosition = { x: event.pageX + 10, y: event.pageY + 10 };
    }
  
    function hideTooltip() {
      showTooltipFlag = false;
    }
  
    function adjustTooltipPosition(x, y, tooltipWidth, tooltipHeight) {
      const windowWidth = window.innerWidth;
      const windowHeight = window.innerHeight;
  
      if (x + tooltipWidth > windowWidth) {
        x = windowWidth - tooltipWidth - 10;
      }
      if (x < 0) {
        x = 10;
      }
  
      if (y + tooltipHeight > windowHeight) {
        y = windowHeight - tooltipHeight - 10;
      }
      if (y < 0) {
        y = 10;
      }
  
      return { x, y };
    }
  
    $: if (showTooltipFlag && tooltipData) {
      const tooltipElement = document.querySelector('.tooltip');
      let tooltipRect;
      if (tooltipElement) {
        tooltipRect = tooltipElement.getBoundingClientRect();
      } else {
        tooltipRect = { width: 300, height: 100 };
      }
  
      const tooltipWidth = tooltipRect.width;
      const tooltipHeight = tooltipRect.height;
  
      adjustedTooltipPosition = adjustTooltipPosition(
        tooltipPosition.x,
        tooltipPosition.y,
        tooltipWidth,
        tooltipHeight
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
  
    onMount(fetchLaunchData);
  </script>
  
  <main>
    <Title />
    <Map {launchData} {statusColors} on:showTooltip={showTooltip} on:hideTooltip={hideTooltip} />
    <Legend {statusColors} />
    <Tooltip {showTooltipFlag} {tooltipData} {adjustedTooltipPosition} {formattedLaunchDate} />
  </main>
  