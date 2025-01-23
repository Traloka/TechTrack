<script>
    export let onClose; 
</script>
  
  <div class="popup-overlay">
    <div class="popup-content">
      <h2>Welcome to the Rocket Launch Map</h2>
      <p>
        This website provides real-time updates on rocket launches worldwide this and upcoming months.
        Explore the map, view rocket details, and stay informed about the latest launches.
      </p>
      <button on:click={onClose}>Let's go!</button>
    </div>
  </div>
  
  <style>
    .popup-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: rgba(0, 0, 0, 0.7);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;
    }

    .popup-content {
    background: white;
    padding: 20px;
    border-radius: 8px;
    max-width: 500px;
    text-align: center;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    line-height: 28px;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      background: #1a1a2e;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    button:hover {
      background: #0056b3;
    }
  </style>
  