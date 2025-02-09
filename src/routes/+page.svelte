<script lang="ts">
	import { onMount } from 'svelte';
	import pkg from '../../package.json';
	import RenderScan from '$lib/components/RenderScan.svelte';

	onMount(() => {
		mounted = true;
	});

	// Interactive demo state
	let highlightColor = $state('#2189b5'); // Changed to Sea Green
	let mounted = $state(false);
	let boxes = $state<number[]>([]);
	let maxBoxes = 3;
	let isAnimating = $state(false);

	// Calculate text color based on background brightness
	function calculateBrightness(hexColor: string): number {
		// Remove # if present
		const color = hexColor.replace('#', '');
		const r = parseInt(color.substring(0, 2), 16);
		const g = parseInt(color.substring(2, 4), 16);
		const b = parseInt(color.substring(4, 6), 16);
		// Calculate perceived brightness using relative luminance
		return (r * 299 + g * 587 + b * 114) / 1000;
	}

	let textColor = $derived.by(() => {
		const brightness = calculateBrightness(highlightColor);
		return brightness > 128 ? '#000000' : '#FFFFFF';
	});

	async function demonstrateRendering() {
		if (isAnimating) return;
		isAnimating = true;
		boxes = [0]; // Start with first box immediately
		// Add remaining boxes one by one with a delay
		for (let i = 1; i < maxBoxes; i++) {
			await new Promise((resolve) => setTimeout(resolve, 800));
			boxes = [...boxes, i];
		}
		isAnimating = false;
	}

	// Installation options
	const installers = [
		{ name: 'NPM', cmd: 'npm install -D svelte-render-scan' },
		{ name: 'PNPM', cmd: 'pnpm install -D svelte-render-scan' },
		{ name: 'Yarn', cmd: 'yarn add -D svelte-render-scan' },
		{ name: 'Bun', cmd: 'bun add svelte-render-scan -d' }
	];
	let installer = $state(installers[0].name);

	// Demo code snippet
	const demoCode = `<script>
  import { RenderScan } from 'svelte-render-scan';
<\/script>

<RenderScan />
`;
</script>

{#if mounted}
	<RenderScan />
{/if}

<div class="border-b-4 bg-[#faf6f4] py-24">
	<div class="container mx-auto flex max-w-xl flex-col items-center text-center">
		<div
			class="mb-10 flex flex-col items-center space-y-3 text-white md:flex-row md:space-x-3 md:space-y-0"
		>
			<div
				class="-rotate-2 rounded-xl bg-[#322f35] p-4 text-xl font-extrabold uppercase tracking-widest shadow-lg md:text-2xl"
			>
				Svelte
			</div>
			<div
				class="rotate-3 rounded-xl p-4 text-xl font-extrabold uppercase tracking-widest shadow-lg md:text-2xl"
				style="background-color: {highlightColor}; color: {textColor}"
			>
				Render
			</div>
			<div
				class="-rotate-2 rounded-xl bg-[#322f35] p-4 text-xl font-extrabold uppercase tracking-widest shadow-lg md:text-2xl"
			>
				Scan
			</div>
		</div>
		<h1 class="text-3xl font-bold md:text-5xl">Visual debugging for Svelte apps</h1>
		<p class="mt-10 max-w-prose text-lg md:text-xl">
			Watch your components update in real-time with beautiful overlays.<br />
			Perfect for debugging reactivity and performance issues.
		</p>

		<!-- Interactive Demo Section -->
		<div class="mt-10 w-full max-w-md rounded-xl bg-white p-6 shadow-lg">
			<h2 class="mb-4 text-lg font-bold">Customize Your Experience</h2>
			<p class="mb-6 italic text-gray-600">And see how the page reacts</p>

			<div class="space-y-6">
				<!-- Color Picker -->
				<div class="flex items-center justify-between">
					<label class="font-medium">Highlight Color</label>
					<input type="color" bind:value={highlightColor} class="h-8 w-12" />
				</div>

				<!-- Demo Section -->
				<div class="space-y-4">
					<div class="flex justify-center">
						<button
							onclick={demonstrateRendering}
							disabled={isAnimating}
							class="flex items-center space-x-2 rounded-xl border-2 border-amber-400 bg-amber-300 px-5 py-2 text-lg font-bold shadow transition-opacity disabled:cursor-not-allowed disabled:opacity-50"
						>
							<svg
								xmlns="http://www.w3.org/2000/svg"
								class="h-5 w-5"
								viewBox="0 0 20 20"
								fill="currentColor"
							>
								<path
									fill-rule="evenodd"
									d="M5 2a1 1 0 011 1v1h1a1 1 0 010 2H6v1a1 1 0 01-2 0V6H3a1 1 0 010-2h1V3a1 1 0 011-1zm0 10a1 1 0 011 1v1h1a1 1 0 110 2H6v1a1 1 0 11-2 0v-1H3a1 1 0 110-2h1v-1a1 1 0 011-1zM12 2a1 1 0 01.967.744L14.146 7.2 17.5 9.134a1 1 0 010 1.732l-3.354 1.935-1.18 4.455a1 1 0 01-1.933 0L9.854 12.8 6.5 10.866a1 1 0 010-1.732l3.354-1.935 1.18-4.455A1 1 0 0112 2z"
									clip-rule="evenodd"
								/>
							</svg>
							<span>See it in action</span>
						</button>
					</div>
					<!-- Demo Boxes -->
					<div class="flex justify-center space-x-4">
						{#each boxes as box (box)}
							<div
								class="h-16 w-16 rounded-lg transition-all duration-300"
								style="background-color: {highlightColor}"
							></div>
						{/each}
					</div>
				</div>
			</div>
		</div>

		<div class="mt-10 grid grid-cols-2 gap-4 self-stretch font-medium md:grid-cols-3">
			{#each ['Real-time Updates', 'Customizable Colors', 'Fine-grained Control', 'Zero Config', 'Lightweight', 'TypeScript Support'] as feature}
				<div class="flex space-x-2 text-left">
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
						stroke={highlightColor}
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"><polyline points="20 6 9 17 4 12" /></svg
					>
					<p>{feature}</p>
				</div>
			{/each}
		</div>
	</div>
</div>

<div class="container mx-auto max-w-2xl py-10">
	<section>
		<div class="mb-4 flex flex-col justify-between gap-4 sm:flex-row sm:items-center">
			<p class="text-xl font-bold">1. Install</p>
			<div class="space-x-1">
				{#each installers as i}
					<label for={i.name} class:checked={i.name === installer}>
						<input
							type="radio"
							id={i.name}
							name="installers"
							value={i.name}
							bind:group={installer}
						/>
						<span
							style="background-color: {i.name === installer
								? highlightColor
								: ''}; color: {i.name === installer ? textColor : ''}">{i.name}</span
						>
					</label>
				{/each}
			</div>
		</div>
		{#each installers as i}
			<div class:hidden={installer !== i.name}>
				<code class="language-shell">
					{i.cmd}
				</code>
			</div>
		{/each}
	</section>

	<section class="mt-10">
		<p class="mb-4 text-xl font-bold">2. Mount and use</p>
		<pre class="language-html"><code class="language-html">{demoCode}</code></pre>
	</section>

	<p class="mb-2 mt-24 text-center">
		<a href="https://github.com/khromov/svelte-render-scan" class="underline">GitHub</a>
	</p>
	<p class="text-center text-gray-500">
		© {new Date().getFullYear()} svelte-render-scan · Version {pkg.version}
	</p>
</div>

<style lang="postcss">
	:not(pre) > code,
	pre {
		@apply block rounded-lg bg-gray-100 p-5;
	}

	input[type='radio'] {
		@apply hidden appearance-none;
	}

	label {
		cursor: pointer;
		@apply rounded-full px-3 py-1;
	}

	label.checked {
		@apply font-bold;
	}

	label.checked span {
		@apply rounded-full px-3 py-1;
	}

	input[type='color'] {
		@apply cursor-pointer rounded border border-gray-200;
	}
</style>
