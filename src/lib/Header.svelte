<!-- Header.svelte -->
<script>
	import logo from '$lib/assets/logo.png';
</script>

<div class="bg-gradient-to-t from-gray-600 to-gray-800 text-white py-2 shadow-lg flex items-center">
	<img src="{logo}" alt="Logo" class="h-16 w-auto ml-4 rounded-full border-2 border-white shadow-sm">
	<div class="flex-1 text-center px-4">
		<h1 class="text-4xl font-extrabold mb-2 tracking-wide text-white">
			Welcome to the Flashcard Creator Tool (FCT)!
		</h1>
		<p class="text-lg mb-4 text-white">
			Create and study flashcards to enhance your learning experience.
		</p>
	</div>
</div>
