<script lang="ts">
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
	import { Toaster } from '$lib/components/ui/sonner';
	import Header from '$lib/components/Header.svelte';

	let { children } = $props();
</script>

<Toaster />
<ModeWatcher />
<div class="flex min-h-dvh flex-col justify-between">
	<Header />
	<main class="flex w-dvw flex-grow flex-col items-center">
		{@render children()}
	</main>

	<!-- <Footer /> -->
</div>

<!-- <style>
	:global(body) {
		font-family: 'Open Sans Variable', sans-serif;
	}
</style> -->
