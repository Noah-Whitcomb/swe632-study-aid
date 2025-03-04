<script>
	import Footer from '$lib/Footer.svelte';
	import Header from '$lib/Header.svelte';
	import '../app.css';
	let { children } = $props();
</script>

<div class="flex flex-col h-screen max-w-screen max-h-screen">
	<div>
		<Header />
	</div>
	<div class="flex flex-col grow">
		{@render children()}
	</div>
	
	<div class="items-end">
		<Footer />
	</div>
</div>