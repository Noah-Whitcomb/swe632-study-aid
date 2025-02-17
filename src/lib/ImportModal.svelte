<script>
    import { createEventDispatcher } from "svelte";
  
    export let show = false;  // Controls modal visibility
    let file = null;
  
    const dispatch = createEventDispatcher();
  
    function handleFileChange(event) {
      file = event.target.files[0];
    }
  
    function handleImport() {
      if (file) {
        dispatch("import", { file });
        closeModal();
      } else {
        alert("Please select a file first.");
      }
    }
  
    function closeModal() {
      dispatch("close");
    }
  </script>
  
  <style>
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999;
    }
  
    .modal-content {
      background: white;
      padding: 20px;
      border-radius: 8px;
      text-align: center;
      position: relative;
      z-index: 10000;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
  
    .modal button {
      margin-top: 10px;
      padding: 8px 16px;
      cursor: pointer;
    }

    input, textarea {
        width: 100%;
        padding: 10px;
        margin: 10px 0;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
  </style>
  
  {#if show}
    <div class="modal" on:click={closeModal}>
      <div class="modal-content" on:click|stopPropagation>
        <h2>Import Deck</h2>
        <input type="file" accept=".txt" on:change={handleFileChange} />
        <button on:click={handleImport}>Import</button>
        <button on:click={closeModal}>Cancel</button>
      </div>
    </div>
  {/if}
  