<script>
  import Flashcard from './Flashcard.svelte';
  import Deck from './Deck.svelte';
  import ImportModal from './ImportModal.svelte';

  let newQuestion = "";
  let newAnswer = "";
  let selectedDeck = null;
  let newDeckName = "";
  let decks = [];
  let showDeckInput = false;
  let showImportModal = false;

  function handleEditDeck(event) {
    const { newName } = event.detail;
    decks = decks.map(deck => 
      deck === selectedDeck 
        ? { ...deck, name: newName }
        : deck
    );
    selectedDeck = { ...selectedDeck, name: newName };
  }

  function handleDeleteDeck() {
    decks = decks.filter(deck => deck !== selectedDeck);
    selectedDeck = null;
  }

  function addFlashcard() {
    if (newQuestion.trim() !== "" && newAnswer.trim() !== "" && selectedDeck !== null) {
      const newCard = { question: newQuestion, answer: newAnswer };
      selectedDeck.flashcards = [...selectedDeck.flashcards, newCard];
      newQuestion = "";
      newAnswer = "";
    }
  }

  function addDeck() {
    if (newDeckName.trim() !== "") {
      const newDeck = { name: newDeckName, flashcards: [] };
      decks = [...decks, newDeck];
      selectedDeck = newDeck;  // Auto-select new deck
      newDeckName = "";
      showDeckInput = false;
    }
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
        if (!selectedDeck) {
            return;
        }

        const text = e.target.result;

        const importedFlashcards = text.split("\n").map(line => {
            const [q, a] = line.split("|").map(part => part.trim());
            return q && a ? { question: q, answer: a } : null;
        }).filter(Boolean);

        selectedDeck.flashcards = [...selectedDeck.flashcards, ...importedFlashcards];
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

  .add-button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
    align-self: flex-end;
  }

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
  <div class="input-container">
    {#if showDeckInput}
      <input
        type="text"
        class="text-input"
        bind:value={newDeckName}
        placeholder="Enter deck name"
      />
      <button class="add-button" on:click={addDeck}>Save Deck</button>
    {:else}
      <button class="add-button" on:click={() => showDeckInput = true}>Add Deck</button>
    {/if}
  </div>
  <div class="deck-list">
    {#each decks as deck}
      <button type="button" class="deck-item" on:click={() => selectDeck(deck)} on:keydown={(e) => e.key === 'Enter' && selectDeck(deck)} role="button">
        {deck.name}
      </button>
    {/each}
  </div>
  {#if selectedDeck}
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
      <button class="add-button" on:click={addFlashcard}>Add</button>
    </div>
    <Deck 
      flashcards={selectedDeck.flashcards} 
      deckName={selectedDeck.name}
      on:edit={handleEditDeck}
      on:delete={handleDeleteDeck}
      on:importDeck={openImportModal}
    />
  {/if}
</div>