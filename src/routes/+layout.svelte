<script>
	import Footer from '$lib/Footer.svelte';
	import Header from '$lib/Header.svelte';
	import '../app.css';
	let { children } = $props();
</script>

<div class="flex flex-col min-h-screen">
	<Header />

	{@render children()}
	
	<Footer />
</div>