<script lang="ts">
	import RenderScan from './RenderScan.svelte';

	let enabled = $state(false);

	// Fixed colors for enabled/disabled states
	const enabledColor = '#2189b5';
	const disabledColor = '#9ca3af';
</script>

{#if enabled}
	<RenderScan />
{/if}

<!-- Floating button -->
<button
	class="fixed bottom-4 right-4 flex h-12 w-12 items-center justify-center rounded-full shadow-lg transition-all hover:scale-110 active:scale-95"
	style:background-color={enabled ? enabledColor : disabledColor}
	on:click={() => (enabled = !enabled)}
	title={enabled ? 'Disable render scanning' : 'Enable render scanning'}
>
	<svg
		xmlns="http://www.w3.org/2000/svg"
		width="24"
		height="24"
		viewBox="0 0 24 24"
		fill="none"
		stroke="white"
		stroke-width="2"
		stroke-linecap="round"
		stroke-linejoin="round"
		class="transition-transform"
		style:transform={enabled ? 'rotate(360deg)' : 'rotate(0deg)'}
	>
		<path d="M2 12s3-7 10-7 10 7 10 7-3 7-10 7-10-7-10-7Z" />
		<circle cx="12" cy="12" r="3" />
	</svg>
</button>

<style>
	button {
		transition:
			transform 0.2s ease,
			background-color 0.3s ease;
		z-index: 100000;
	}

	svg {
		transition: transform 0.5s ease;
	}
</style>
