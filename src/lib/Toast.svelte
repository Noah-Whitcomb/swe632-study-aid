<script>
    export let message = "";
    export let show = false;
  </script>
  
  <style>
    .toast {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background-color: #4caf50; /* Green */
      color: white;
      padding: 15px 20px;
      border-radius: 5px;
      font-size: 16px;
      font-weight: bold;
      box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);
      transition: opacity 0.5s ease-in-out;
      z-index: 9998;
    }
    .hidden {
      opacity: 0;
      pointer-events: none;
    }
  </style>
  
  <div class="toast {show ? '' : 'hidden'}">
    {message}
  </div>
  