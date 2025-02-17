<script>
  import FlashcardList from '$lib/FlashcardList.svelte';
	import Menu from '$lib/Menu.svelte';

  let decks = $state([]);
  let selectedDeck = $state(null);

  function handleDeckChange(deck) {
    console.log("handling deck change in page.svelet");
    selectedDeck = deck;
  }

</script>

<main class="overflow-auto w-full">
  <div class="flex justify-start">
    <Menu decks={decks} handleDeckChange={handleDeckChange}/> 
    <FlashcardList bind:decks={decks} handleDeckChange={handleDeckChange}/>
  </div>
</main>