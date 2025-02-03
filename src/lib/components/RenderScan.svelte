<script lang="ts">
	import { onMount } from 'svelte';
	import renderScan from '$lib/render-scan';

	onMount(() => {
		renderScan();
	});
</script>
