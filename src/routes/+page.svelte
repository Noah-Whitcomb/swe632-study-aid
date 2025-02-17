<script>
  import FlashcardList from '$lib/FlashcardList.svelte';
	import Menu from '$lib/Menu.svelte';

  let decks = $state([]);
  let selectedDeck = $state(null);
  let newDeckName = $state("");
  let showDeckInput = $state(false);
  let showMenu = $state(false);

  function handleDeckChange(deck) {
    console.log("handling deck change in page.svelet");
    selectedDeck = deck;
    console.log("selectedDeck = " + JSON.stringify(selectedDeck, null, 2));
  }

  function addDeck(newDeckName) {
    if (newDeckName.trim() !== "") {
      const newDeck = { name: newDeckName, flashcards: [] };
      decks = [...decks, newDeck];
      //selectedDeck = newDeck;  // Auto-select new deck
      handleDeckChange(decks.length - 1);  // Notify parent component of the new deck
      console.log("decks in flashcardlist = " + decks);
    }
  }

  function addFlashcard(newQuestion, newAnswer) {
    if (newQuestion.trim() !== "" && newAnswer.trim() !== "" && selectedDeck !== null) {
      const newCard = { question: newQuestion, answer: newAnswer };
      decks[selectedDeck].flashcards = [...decks[selectedDeck].flashcards, newCard];
      console.log("inside addFlashcard:" + JSON.stringify(selectedDeck, null, 2));
    }
  }

  function handleShowMenu() {
    console.log("Button clicked");
    console.log("decks =" + decks);
    showMenu = !showMenu;
}

</script>

<main class="overflow-auto w-full">
  <!-- {JSON.stringify(decks, null, 2)} -->
  <div class="flex">
    <div class="flex flex-col justify-start">
      <div class="flex" >
        <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-start mb-2" onclick={handleShowMenu}> Show active decks? </button>
        <div class="flex justify-center w-[800px] mb-4 box-border">
          {#if showDeckInput}
            <input
              type="text"
              class="text-input"
              bind:value={newDeckName}
              placeholder="Enter deck name"
            />
            <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-end" onclick={() => { addDeck(newDeckName); newDeckName = ""; showDeckInput = false;} }>Save Deck</button>
          {:else}
            <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-end" onclick={() => showDeckInput = true}>Add Deck</button>
          {/if}
        </div>
      </div>
      {#if showMenu}
      <Menu decks={decks} handleDeckChange={handleDeckChange}/> 
      {/if}
      
    </div>
    <div class="justify-center">
      <FlashcardList decks={decks} handleDeckChange={handleDeckChange} selectedDeck={selectedDeck} addDeck={addDeck} addFlashcard={addFlashcard}/>
    </div>
    
  </div>
</main>