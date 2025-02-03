<script lang="ts">
	import renderScan from '$lib/render-scan';
	import { onMount } from 'svelte';
	import pkg from '../../package.json';
	//import 'prismjs';
	//import 'prism-svelte';
	import RenderScan from '$lib/components/RenderScan.svelte';

	onMount(() => {
		mounted = true;
	});

	// Interactive demo state
	let highlightColor = $state('#2E8B57'); // Changed to Sea Green
	let jokeLength = $state(50);
	let mounted = $state(false);

	// Joke generation with 5 levels, 3 jokes each
	let currentJoke = $state(
		'Why did the Svelte component go to therapy? It had too many reactive issues!'
	);
	const jokes = {
		1: [
			// Shortest
			'What is Svelte? Magic!',
			'Svelte? Simply sweet!',
			'Reactive? Effective!'
		],
		2: [
			'Why use Svelte? Less code!',
			'Svelte: Write less, do more!',
			'Components love Svelte life!'
		],
		3: [
			'What does a Svelte dev order? State management!',
			'How do Svelte apps run? Reactively fast!',
			'Svelte developers code less and smile more!'
		],
		4: [
			'Why was the Svelte app feeling lonely? It had no props to talk to!',
			'What does a Svelte dev say at a restaurant? "Can I get that state to go?"',
			'How do Svelte components stay in shape? They do reactive training!'
		],
		5: [
			// Longest
			'Why did the Svelte component go to therapy? It had too many reactive issues!',
			'What happened when the Svelte component lost its state? It had an identity crisis!',
			'How many Svelte developers does it take to change a lightbulb? None, it reactively updates itself!'
		]
	};

	function generateJoke() {
		const level = Math.ceil((jokeLength / 100) * 5);
		const levelJokes = jokes[level as keyof typeof jokes];
		const randomJoke = levelJokes[Math.floor(Math.random() * levelJokes.length)];
		currentJoke = randomJoke;
	}

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
				style="background-color: {highlightColor}"
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

				<!-- Joke Section -->
				<div class="space-y-4">
					<div class="flex justify-center">
						<button
							on:click={generateJoke}
							class="flex items-center space-x-2 rounded-xl border-2 border-amber-400 bg-amber-300 px-5 py-2 text-lg font-bold shadow"
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
							<span>Generate joke</span>
						</button>
					</div>
					<p class="italic text-gray-700">"{currentJoke}"</p>
					<div class="space-y-2">
						<label class="font-medium">Joke length: Level {Math.ceil((jokeLength / 100) * 5)}</label
						>
						<input
							type="range"
							bind:value={jokeLength}
							min="0"
							max="100"
							step="1"
							class="w-full"
							style="--thumb-color: {highlightColor}"
						/>
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
						<span style="background-color: {i.name === installer ? highlightColor : ''}"
							>{i.name}</span
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
		@apply rounded-full px-3 py-1 text-white;
	}

	input[type='range'] {
		@apply h-2 appearance-none rounded-lg bg-gray-200;
	}

	input[type='range']::-webkit-slider-thumb {
		@apply h-4 w-4 cursor-pointer appearance-none rounded-full;
		background-color: var(--thumb-color, #2e8b57);
	}

	input[type='color'] {
		@apply cursor-pointer rounded border border-gray-200;
	}
</style>
