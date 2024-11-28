<!-- <script>
    export let statusColors;
  </script> -->
  
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
    <!-- {#each Object.entries(statusColors) as [status, color]}
      <li>
        <div class="legend-color" style="background-color: {color};"></div>
        {status.toUpperCase()}
      </li>
    {/each} -->
  </ul>
  
  <style>
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
  