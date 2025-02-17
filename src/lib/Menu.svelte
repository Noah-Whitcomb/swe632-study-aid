<script>
import Deck from './Deck.svelte';
let { decks, handleDeckChange } = $props();

</script>

<div>
	<div class="flex flex-col gap-2">
        {#each decks as deck, index}
          <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-start" onclick={() => handleDeckChange(index)} >
            {deck.name}
          </button>
        {/each}
    </div>
</div>