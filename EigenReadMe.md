Kaart: https://d3js.org/d3-geo/cylindrical#geoMercator
Voor verder schrijven van de kaart: https://www.geeksforgeeks.org/d3-js-geomercator-function/


onMount: 
Wordt geactiveerd wanneer de component voor het eerst wordt geladen en de daarin geleverde functie wordt uitgevoerd.

async: 
Maakt de functie asynchroon, waardoor je 'await' erin kunt gebruiken voor taken zoals het ophalen van gegevens uit een API. Waarom dit nodig is omdat
er veel data van de API wordt opgehaalt dit kost tijd. Dit werkt goed samen met 'await' die als het ware 'async' de tijd geeft.


Waarom async/wachten gebruiken?
Gemakkelijker te lezen: we gebruiken await om te pauzeren totdat elke asynchrone taak is voltooid.
Meer synchroon in stijl: met async/await kunt u asynchrone code schrijven die op synchrone code lijkt.
Foutafhandeling: Met async/await kunnen we try...catch-blokken gebruiken om fouten af ​​te handelen, waardoor het gemakkelijker wordt om fouten in asynchrone taken af ​​te handelen.



while (url && pageCount < 5) { 
    const response = await fetch(url);
    const data = await response.json();
    launchData = launchData.concat(data.results); 

    url = data.next; 
    pageCount += 1; 

await:
Laat de opbouw van de DOM wachten totdat alle data binnen is.

Wat er gebreurt:
1. Eerst wachten: we "pauzeren" de functie en wachten tot fetch klaar is met het ophalen van de gegevens uit de API.

2. Tweede wachttijd: nadat het ophalen is voltooid, is response.json() ook een asynchrone bewerking, dus gebruiken we await om opnieuw te "pauzeren" totdat het     parseren van JSON is voltooid.

3. Zodra beide taken zijn voltooid, hebben we de gegevens gereed voor gebruik en loggen we deze.

Next:
Is om data van de volgende paginas's te halen.  url = data.next;  zet de url als het ware tot de volgende pagina.

While:
Zegt blijf data door fetchen tot in dit geval pagina 4, waarom? Omdat de API niet meer toe laat dan 4.

pageCount += 1; 
Laat hem tellen dat hij weet wanneer hij bij de 4 pagina's is.




globaal()
Hiermee vertel je Svelte om deze stijlen buiten het bereik van de component toe te passen, waardoor ze invloed kunnen hebben op de root-<html>- en <body>-elementen. Dit ben ik gaan gebruiken om een onodige scroll uit de pagina te krijgen. 





Tooltip:
Dit heb ik gevonden voor de popup. Het is een component van Svelte zelf om een hover te creeëren met een popup die informatie dan laat zien.

Positionering van tooltip in geval van ruimte gebrek:


$: if (showTooltipFlag && tooltipData) {
const tooltipElement = document.querySelector(".tooltip"); 
  const tooltipRect = tooltipElement
  

$: 
duidt reactieve aan, deze code in dit blok wordt dus automatisch opnieuw uitgevoerd wanneer de reactieve variabelen (in dit geval 
showTooltipFlag of tooltipData) veranderen.

&&:
Is als letterlijk en en in dit geval dus: if (showTooltipFlag en en tooltipData) waar zijn.

tooltipElement.getBoundingClientRect():
Geeft informatie over de afmetingen van het element en de positie ten opzichte van de viewport. 
Dit zorgt ervoor dat de grootte van de tooltip dynamisch en nauwkeurig wordt berekend, zelfs als de inhoud erin verandert 
(bijvoorbeeld langere tekst).
Gebruikte video: https://www.youtube.com/watch?v=MjDOZmfsKvs&ab_channel=AndreasWik 

In het kort wat er gebeurt:
De grootte (tooltipRect.width en tooltipRect.height) wordt doorgegeven aan de functie AdjustTooltipPosition.
AdjustTooltipPosition zorgt ervoor dat de tooltip binnen de viewport blijft.
Elke keer dat showTooltipFlag of tooltipData verandert, wordt dit blok opnieuw uitgevoerd, waarbij de afmetingen en positie van de tooltip 
dynamisch opnieuw worden berekend. Dat dan weer zorgt dat de tooltip altijd zichtbaar is.
