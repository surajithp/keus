<script>
	import Nav from '$lib/Nav.svelte';
	import Footer from '$lib/Footer.svelte';
</script>

<div id="content">
	<Nav />
	<slot />
	<Footer />
</div>
