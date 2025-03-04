<script>
import Deck from './Deck.svelte';
let { decks, handleDeckChange } = $props();

</script>

<div>
	<div class="flex flex-col gap-2">
        {#each decks as deck, index}
          <button class="flex justify-center bg-cyan-800 text-white rounded-2xl text-2xl p-4 cursor-pointer" onclick={() => handleDeckChange(index)} >
            {deck.name}
          </button>
        {/each}
    </div>
</div>