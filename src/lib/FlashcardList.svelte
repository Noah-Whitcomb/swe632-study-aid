<script>
  import Flashcard from './Flashcard.svelte';
  import Deck from './Deck.svelte';
  import ImportModal from './ImportModal.svelte';
  let newQuestion = $state("");
  let newAnswer = $state("");
  let newDeckName = $state("");
  let showDeckInput = $state(false);
  let { decks=$bindable(), handleDeckChange, selectedDeck, addDeck, addFlashcard } = $props();
  let showImportModal = $state(false);

  function handleEditDeck(event) {
    const { newName } = event.detail;
    // decks = decks.map(deck => 
    //   deck === selectedDeck 
    //     ? { ...deck, name: newName }
    //     : deck
    // );
    decks[selectedDeck].name = newName;
    //selectedDeck = { ...selectedDeck, name: newName };
  }

  function handleDeleteDeck() {
    decks.splice(selectedDeck, 1)
    selectedDeck = null;
  }

  function openImportModal() {
    showImportModal = true;
  }

  function closeImportModal() {
    showImportModal = false;
  }

  function importDeck(event) {
    const file = event.detail.file;
    if (!file) {
      return;
    }
    const reader = new FileReader();
    reader.onload = (e) => {
        const text = e.target.result;
        const importedFlashcards = text.split("\n").map(line => {
            const [q, a] = line.split("|").map(part => part.trim());
            return q && a ? { question: q, answer: a } : null;
        }).filter(Boolean);

        decks[selectedDeck].flashcards = [...decks[selectedDeck].flashcards, ...importedFlashcards];

    };
    reader.readAsText(file);
  }

  function selectDeck(deck) {
    selectedDeck = deck;
  }
</script>

<style>
  .flashcard-list {
    max-width: 1800px;  /* Increased to match deck width */
    min-height: 1000px;
    margin: 0 auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .input-container {
    width: 800px;  /* Match deck width */
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
    box-sizing: border-box;
  }

  .text-input {
    padding: 0.5rem;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-bottom: 0.5rem;
    width: 100%;
    box-sizing: border-box;
  }

  /* .add-button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
    align-self: flex-end;
  } */


  .add-button:hover {
    background-color: #0056b3;
  }

  .deck-list {
    width: 800px;  /* Match deck width */
    margin-bottom: 1rem;
  }

  .deck-item {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-bottom: 0.5rem;
    cursor: pointer;
    background-color: #f9f9f9;
  }

  .deck-item:hover {
    background-color: #e9e9e9;
  }
</style>

<ImportModal show={showImportModal} on:import={importDeck} on:close={closeImportModal} />

<div class="flashcard-list">
  {#if typeof selectedDeck === "number" && selectedDeck >= 0}
    <div class="input-container">
      <input
        type="text"
        class="text-input"
        bind:value={newQuestion}
        placeholder="Enter question"
      />
      <input
        type="text"
        class="text-input"
        bind:value={newAnswer}
        placeholder="Enter answer"
      />
      <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-end" onclick={() => { addFlashcard(newQuestion, newAnswer); newQuestion = ""; newAnswer = ""; } }>Add</button>
    </div>
    <Deck 
      flashcards={decks[selectedDeck].flashcards} 
      deckName={decks[selectedDeck].name}
      on:edit={handleEditDeck}
      on:delete={handleDeleteDeck}
      on:importDeck={openImportModal}
    />
  {/if}
</div>
