<script>
import Deck from './Deck.svelte';
let { decks, handleDeckChange } = $props();
let showMenu = $state(false);

function handleShowMenu() {
    console.log("Button clicked");
    console.log("decks =" + decks);
    showMenu = !showMenu;
}

</script>

<div>
    <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-end" onclick={handleShowMenu}> toggle </button>
    {#if showMenu}
	<div>
        {#each decks as deck}
          <button class="px-4 py-2 text-base border-none rounded-md bg-blue-500 text-white cursor-pointer self-end" onclick={() => handleDeckChange(deck)} >
            {deck.name}
          </button>
        {/each}
      </div>
    {/if}
</div>