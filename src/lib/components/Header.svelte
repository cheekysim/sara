<script lang="ts">
	import Nav from './Nav.svelte';
	import { Button } from '$lib/components/ui/button';
</script>

<header
	class="border-grid bg-background/95 supports-[backdrop-filter]:bg-background/60 sticky top-0 z-50 w-full border-b backdrop-blur"
>
	<div class="mx-auto my-2 flex max-w-screen-2xl items-center justify-between">
		<h2 class="text-xl font-bold">Kissed Toge</h2>
		<div class="bg-background flex items-center gap-4 rounded-md px-4 py-2">
			<Nav />
			<Button href="/login" class="hover:scale-105 active:scale-95 bg-secondary/90 hover:bg-secondary">Log In / Sign Up</Button>
		</div>
	</div>
</header>
