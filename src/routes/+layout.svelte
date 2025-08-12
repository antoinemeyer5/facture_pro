<script lang="ts">
	import favicon from "$assets/favicon.svg";

	let { children } = $props();
</script>

<svelte:head>
	<title>FacturePro</title>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children?.()}
