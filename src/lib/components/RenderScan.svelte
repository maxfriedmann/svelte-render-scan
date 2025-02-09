<script lang="ts">
	import { Eye } from 'lucide-svelte';
	import RenderScanChecker from './RenderScanChecker.svelte';

	// Props with defaults
	let { initialEnabled = true, offsetLeft = 0 } = $props<{
		initialEnabled?: boolean;
		offsetLeft?: number;
	}>();

	// State management
	let enabled = $state(initialEnabled);

	// Fixed colors for enabled/disabled states
	const enabledColor = '#2189b5';
	const disabledColor = '#9ca3af';
</script>

{#if enabled}
	<RenderScanChecker />
{/if}

<!-- Floating button -->
<button
	class="fixed bottom-4 right-4 flex h-12 w-12 items-center justify-center rounded-full shadow-lg transition-all hover:scale-110 active:scale-95"
	style:background-color={enabled ? enabledColor : disabledColor}
	style:right={`calc(1rem + ${offsetLeft}px)`}
	onclick={() => (enabled = !enabled)}
	title={enabled ? 'Disable render scanning' : 'Enable render scanning'}
>
	<div class="transition-transform" style:transform={enabled ? 'rotate(360deg)' : 'rotate(0deg)'}>
		<Eye color="white" size={24} />
	</div>
</button>

<style>
	button {
		transition:
			transform 0.2s ease,
			background-color 0.3s ease;
		z-index: 100000;
	}

	div {
		transition: transform 0.5s ease;
	}
</style>
