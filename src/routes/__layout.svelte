<script>
	import Nav from '$lib/Nav.svelte';
	import Footer from '$lib/Footer.svelte';
	import { onMount } from 'svelte';
	onMount(() => {
		const appHeight = () => {
			const doc = document.documentElement;
			doc.style.setProperty('--app-height', `${window.innerHeight}px`);
		};

		window.addEventListener('resize', appHeight);
		appHeight();
	});
</script>

<div id="content">
	<Nav />
	<slot />
	<Footer />
</div>

<style>
	:root {
		--app-height: 100%;
	}
	#content {
		height: var(--app-height);
	}
</style>
