<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<svelte:head>
	<title>Svelte Render Scan</title>
	<meta name="title" content="svelte-render-scan" />
	<meta
		name="description"
		content="Visual debugging tool for Svelte applications. Watch your components update in real-time with beautiful overlays."
	/>

	<!-- Open Graph / Facebook -->
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://svelte-render-scan.com/" />
	<meta property="og:title" content="Svelte Render Scan" />
	<meta
		property="og:description"
		content="Visual debugging tool for Svelte applications. Watch your components update in real-time with beautiful overlays."
	/>
	<meta property="og:image" content="https://svelte-render-scan.com/og-image.png" />

	<!-- Twitter -->
	<meta property="twitter:card" content="summary_large_image" />
	<meta property="twitter:url" content="https://svelte-render-scan.com/" />
	<meta property="twitter:title" content="Svelte Render Scan" />
	<meta
		property="twitter:description"
		content="Visual debugging tool for Svelte applications. Watch your components update in real-time with beautiful overlays."
	/>
	<meta property="twitter:image" content="https://svelte-render-scan.com/og-image.png" />
</svelte:head>

{@render children()}
