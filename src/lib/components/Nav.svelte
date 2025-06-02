<script lang="ts">
	import * as HoverCard from '$lib/components/ui/hover-card';
	import { Button } from '$lib/components/ui/button';
	import { onNavigate } from '$app/navigation';
	import { titleCase, cn } from '$lib/utils';

	interface Link {
		label: string;
		href: string;
	}

	const links: Link[] = [
		{ label: 'Home', href: '/' },
		{ label: 'About', href: '/about' },
		{
			label: 'Contact',
			href: '/contact'
		}
	];

	import { page } from '$app/state';

	let activeTab = $derived(titleCase(page.url.pathname.split('/')[1]) || 'Home');
</script>

<nav class="flex flex-row items-center justify-center gap-4">
	{#each links as link}
		<Button
			variant="outline"
			class={cn('text-sm font-medium active:scale-95 hover:scale-105', activeTab === titleCase(link.label) ? 'border-primary' : '')}
			href={link.href}>{link.label}</Button
		>
	{/each}
</nav>
