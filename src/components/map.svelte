<!-- <script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    export let onShowTooltip = () => {}; 
    export let onHideTooltip = () => {}; 
  
    let launchData = [];
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
  
    onMount(async () => {
      await fetchLaunchData();
  
      const width = window.innerWidth;
      const height = window.innerHeight;
  
      const projectionMap = d3.geoEquirectangular()
        .center([0, 0])
        .translate([width / 2, height / 2])
        .scale((512 / (2 * Math.PI)) * (width / 512));
  
      const svg = d3.select('#map')
        .append('svg')
        .attr('width', width)
        .attr('height', height);
  
      const data = await d3.json(
        'https://raw.githubusercontent.com/janasayantan/datageojson/master/world.json'
      );
  
      svg.append('g')
        .selectAll('path')
        .data(data.features)
        .enter()
        .append('path')
        .attr('fill', 'grey')
        .attr('d', d3.geoPath().projection(projectionMap))
        .style('stroke', '#ffff');
  
      svg.selectAll('circle')
        .data(launchData)
        .enter()
        .append('circle')
        .attr('cx', (d) => projectionMap([d.pad.longitude, d.pad.latitude])[0])
        .attr('cy', (d) => projectionMap([d.pad.longitude, d.pad.latitude])[1])
        .attr('r', 8)
        .attr('fill', (d) => statusColors[d.status.abbrev.toLowerCase()])
        .on('mousemove', (event, d) => onShowTooltip(event, d)) 
        .on('mouseout', () => onHideTooltip()); 
    });
  </script>
  
  <div id="map"></div>
  
  <style>
    #map {
      width: 100vw;
      height: 100vh;
      background-color: #1a1a2e;
    }
  </style>
   -->