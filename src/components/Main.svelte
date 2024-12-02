<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  let launchData = []; 
  let tooltipData = null; 
  let showTooltipFlag = false; 
  let tooltipPosition = { x: 0, y: 0 }; 
  let adjustedTooltipPosition = { x: 0, y: 0 };
  let formattedLaunchDate = '';

  
// Hier worden de kleuren aan de stippen gegeven afhankelijk van de status
  
  
  const statusColors = {
  "tbc": 'orange', // To Be Confirmed
  "go": 'green',   // Go for launch
  "tbd": 'red',    // To Be Determined
  "hold": 'black', // On hold 
  };


//Hier wordt de lanceer data opgehaalt van de API maakt de functie asynchroon, waarbij je 'await' gebruikt.
// Waarom dit nodig is omdat er veel data van de API wordt opgehaalt dit kost tijd. Je zegt letterlijk wacht met het bouwen.


  async function fetchLaunchData() {
    const response = await fetch('https://ll.thespacedevs.com/2.3.0/launches/?ordering=-last_updated');
    const data = await response.json();
    launchData = data.results; 
  }

// Voor het ophalen van de maximale hoeveelheid data van de API 


  // async function fetchLaunchData() {
  // let url = 'https://ll.thespacedevs.com/2.3.0/launches/?ordering=-last_updated&page_size=40'; 
  // let pageCount = 0; 

  // while (url && pageCount < 4) { //vraag je af waarom 4? Omdat de API mij niet meer toe laat op te halen.
  // const response = await fetch(url);
  // const data = await response.json();
  // launchData = launchData.concat(data.results); 

  // url = data.next; 
  // pageCount += 1; 
  //  }
  // }

  
// Hier wordt de lanceer data, defineren van geografische projectie, een SVG gemaakt voor de kaart en 
// stippen voor lanceer locaties gemaakt.


onMount(async () => {
  await fetchLaunchData();

  const width = window.innerWidth;
  const height = window.innerHeight;

  const projectionMap = d3.geoEquirectangular()
      .center([0, 0])
      .translate([width / 2, height / 2])
      .scale(512 / (2 * Math.PI) * (width / 512));

  const svg = d3.select("#map")
      .append("svg")
      .attr("width", width)
      .attr("height", height);

  const data = await d3.json("https://raw.githubusercontent.com/janasayantan/datageojson/master/world.json");

  svg.append("g")
    .selectAll("path")
    .data(data.features)
    .enter()
    .append("path")
    .attr("fill", "grey")
    .attr("d", d3.geoPath().projection(projectionMap))
    .style("stroke", "#ffff");

svg
  .selectAll('circle')
  .data(launchData)
  .enter()
  .append('circle')
  .attr('cx', (d) => projectionMap([d.pad.longitude, d.pad.latitude])[0])
  .attr('cy', (d) => projectionMap([d.pad.longitude, d.pad.latitude])[1])
  .attr('r', 8)
  .attr('fill', (d) => statusColors[d.status.abbrev.toLowerCase()]) //hier word bepaald welke kleur de stip heeft afhankelijk van de status
  .on('mousemove', (event, d) => showTooltip(event, d))
  .on('mouseout', hideTooltip);
});


//Hier wordt de tooltip gemaakt, aangegeven wanneer wel wanneer niet te tonnen en welke informetie er getoond moet worden


function showTooltip(event, d) {
  tooltipData = d;
  showTooltipFlag = true; 
  tooltipPosition = { x: event.pageX + 10, y: event.pageY + 10 }; //om de popup zichtbaar in beeld te houden
}

function hideTooltip() {
  showTooltipFlag = false; //hover ik niet over een stip wordt de Tooltip niet getoond
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
const tooltipElement = document.querySelector(".tooltip"); // Zoek de weergegeven tooltip op de pagina, zodat we de grootte en positie ervan kunnen achterhalen
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

const launchDate = new Date(tooltipData.net); //net = Not Earlear Than 
formattedLaunchDate = launchDate.toLocaleString('en-US', {  //Hier zou ook makkelijk nl-NL kunnen om de datum en tijd in het Nederlands te weergeven
weekday: 'long',
year: 'numeric',
month: 'long',
day: 'numeric',
hour: '2-digit',
minute: '2-digit',
hourCycle: 'h24', //geeft anders tijd in AM/PM aan
timeZoneName: 'short',
});}

</script>


<!-- Hier is de stijling van de visualistaie -->


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


.title-container {
display: flex;
justify-content: center;
align-items: center;
position: absolute;
top: 0;
left: 0;
width: 100vw;
height: 10vh;
z-index: 1;
}

h1 {
font-size: 2rem;
padding: .5em;
color: white;
text-align: center;
font-family: 'Orbitron', sans-serif;
}

main {
font-family: 'Orbitron', sans-serif;
}

:global(html, body) {
margin: 0;
padding: 0;
overflow: hidden;
width: 100vw;
height: 100vh;
}

#map {
width: 100vw; 
height: 100vh; 
padding-top: 3em;
overflow: hidden;
background-color: #1a1a2e;
}

ul {
  position: absolute;
  top: 12px;
  right: 12px;
  background-color: white;
  border: 1px solid #ddd;
  padding: 1px;
  border-radius: 4px;
  font-family: 'Orbitron', sans-serif;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

li {
  display: flex;
  align-items: center;
  margin: 0.8em;
}

.legend-color {
  width: 15px;
  height: 15px;
  margin-right: 10px;
  border-radius: 50%;
}
</style>

<!-- De tilel van de pagina, een legenda voor de kleuren van stippen en de informatie die in de tooltip wordt getoond wordt hier gemaakt -->

<main>
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

      <li>
        <div class="legend-color" style="background-color: black"></div>
        On hold 
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

<!-- De inline-stijl stelt de positie van de tooltip dynamisch in op basis van de berekende adjustedTooltipPosition -->