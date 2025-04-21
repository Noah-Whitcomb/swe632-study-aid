<!-- Header.svelte -->

<script>
	import logo from '$lib/assets/logo.png';
</script>

<div class="bg-gray-800 text-white py-2 flex items-center">
    <img src="{logo}" alt="Logo" class="h-16 w-auto ml-4 rounded-2xl">
    <div class="flex-1 text-center">
        <h1 class="text-3xl font-bold mb-4 text-white">
          Welcome to the Flashcard Creator Tool (FCT)!
        </h1>
        <p class="mb-8">
          Create and study flashcards to enhance your learning experience.
        </p>
    </div>
</div>
