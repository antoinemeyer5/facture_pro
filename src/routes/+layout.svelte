<script lang="ts">
	import "../app.css";
	import favicon from "$assets/favicon.svg";

	let { children } = $props();

	function print() {
        window.print();
    }
</script>

<svelte:head>
	<title>Facture Pro</title>
	<link rel="icon" href={favicon} />
</svelte:head>

<div class="flex width-65vw ml-mr mt-5" style="justify-content: space-between;">
	<h1 class="no_print">FacturePro</h1>
	<div>
		<a class="no_print" href="/devis_1"><button class="button">Devis (m²)</button></a>
		<a class="no_print" href="/facture_1"><button class="button">Facture (h)</button></a>
	</div>
	<button class="no_print button" onclick={print}> Téléchargez le PDF </button>
</div>

{@render children?.()}
