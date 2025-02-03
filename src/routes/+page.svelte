<script lang="ts">
	import renderScan from '$lib/render-scan';
	import { onMount } from 'svelte';
	import pkg from '../../package.json';
	import 'prismjs';
	import 'prism-svelte';

	// Interactive demo state
	let overlayOpacity = $state(0.05);
	let highlightColor = $state('#aa00ff');
	let fadeOutDelay = $state(1000);

	// Initialize render-scan for the demo
	onMount(() => {
		renderScan();
	});

	// Installation options
	const installers = [
		{ name: 'NPM', cmd: 'npm install svelte-render-scan' },
		{ name: 'PNPM', cmd: 'pnpm install svelte-render-scan' },
		{ name: 'Yarn', cmd: 'yarn add svelte-render-scan' },
		{ name: 'Bun', cmd: 'bun add svelte-render-scan' }
	];
	let installer = $state(installers[0].name);

	// Demo code snippet
	const demoCode = `<script>
  import renderScan from 'svelte-render-scan';
  import { onMount } from 'svelte';

  onMount(() => {
    renderScan();
  });
<\/script>

<!-- Your app code here -->
`;
</script>

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
				class="rotate-3 rounded-xl bg-[#aa00ff] p-4 text-xl font-extrabold uppercase tracking-widest shadow-lg md:text-2xl"
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
			<h2 class="mb-4 text-lg font-bold">Try the controls below to see updates</h2>

			<div class="space-y-6">
				<!-- Opacity Slider -->
				<div class="space-y-2">
					<label class="font-medium">Overlay Opacity: {overlayOpacity}</label>
					<input
						type="range"
						bind:value={overlayOpacity}
						min="0"
						max="0.2"
						step="0.01"
						class="w-full"
					/>
				</div>

				<!-- Color Picker -->
				<div class="flex items-center justify-between">
					<label class="font-medium">Highlight Color</label>
					<input type="color" bind:value={highlightColor} class="h-8 w-12" />
				</div>

				<!-- Fade Out Delay -->
				<div class="space-y-2">
					<label class="font-medium">Fade Out Delay: {fadeOutDelay}ms</label>
					<input
						type="range"
						bind:value={fadeOutDelay}
						min="0"
						max="3000"
						step="100"
						class="w-full"
					/>
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
						stroke="#aa00ff"
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
						{i.name}
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
		<a href="https://github.com/yourusername/svelte-render-scan" class="underline">GitHub</a>
	</p>
	<p class="text-center text-gray-500">
		© 2024 svelte-render-scan · Version {pkg.version}
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
		@apply bg-gray-200 font-bold;
	}

	input[type='range'] {
		@apply h-2 appearance-none rounded-lg bg-gray-200;
	}

	input[type='range']::-webkit-slider-thumb {
		@apply h-4 w-4 cursor-pointer appearance-none rounded-full bg-[#aa00ff];
	}

	input[type='color'] {
		@apply cursor-pointer rounded border border-gray-200;
	}
</style>
