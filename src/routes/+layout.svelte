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

<div class="flex mt-5 justify-content-space-between">
	<a class="no_print" href="/"
		><button class="buttonHeader"><h1>FacturePro</h1></button></a
	>
	<div>
		<a class="no_print" href="/otrta"
			><button class="buttonHeader">Facture/Devis sans TVA</button></a
		>

		<!--<a class="no_print" href="/facture_2"
			><button class="button" disabled>Facture (TVA)</button></a
		>-->
	</div>
	<button class="no_print buttonHeader" onclick={print}>
		Téléchargez le PDF
	</button>
</div>

{@render children?.()}
