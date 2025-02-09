<script lang="ts">
	import '../app.css';
	let { children } = $props();

	const SITE_URL = 'https://khromov.github.io/svelte-render-scan';
</script>

<svelte:head>
	<title>Svelte Render Scan</title>
	<meta name="title" content="Svelte Render Scan" />
	<meta
		name="description"
		content="Visual debugging for Svelte apps. Watch your components update in real-time with beautiful overlays. Perfect for debugging reactivity and performance issues."
	/>

	<!-- Open Graph / Facebook -->
	<meta property="og:type" content="website" />
	<meta property="og:url" content={SITE_URL} />
	<meta property="og:title" content="Svelte Render Scan" />
	<meta
		property="og:description"
		content="Visual debugging for Svelte apps. Watch your components update in real-time with beautiful overlays. Perfect for debugging reactivity and performance issues."
	/>
	<meta property="og:image" content={`${SITE_URL}/og-image.png`} />

	<!-- Twitter -->
	<meta property="twitter:card" content="summary_large_image" />
	<meta property="twitter:url" content={SITE_URL} />
	<meta property="twitter:title" content="Svelte Render Scan" />
	<meta
		property="twitter:description"
		content="Visual debugging for Svelte apps. Watch your components update in real-time with beautiful overlays. Perfect for debugging reactivity and performance issues."
	/>
	<meta property="twitter:image" content={`${SITE_URL}/og-image.png`} />
</svelte:head>

{@render children()}
