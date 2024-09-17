<script>
	import * as myc from "@apple/mycelium";
	import "@apple/mycelium/dist/style.css";
	import { onMount } from "svelte";

	const [a, b] = ["a", "b"];

	// given node id produces a node
	function createNode(nodeId) {
		return new myc.ui.Node(
			nodeId,
			new myc.ui.VStack(
				new myc.ui.Text("Node").with({ fontWeight: 600 }),
				new myc.ui.Text(nodeId)
			)
		);
	}

	function getRandomInt(min, max) {
		min = Math.ceil(min);
		max = Math.floor(max);
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	function randomGraph(network, n, maxCons = 10) {
		// create the nodes
		for (let i = 0; i < n; i++) {
			network.setNode(i, createNode(i));
		}

		// random connect the nodes
		for (let i = 0; i < maxCons; i++) {
			const idA = getRandomInt(0, n - 1);
			const idB = getRandomInt(0, n - 1);
			network.setEdge(idA, idB);
		}
	}

	let el;
	let viewer;
	onMount(() => {
		const network = new myc.Network();
		randomGraph(network, 200, 300);

		viewer = myc.NetworkViewer.create(network, el, {
			showBreadcrumbs: false,
			minimap: true,
		});
	});
</script>

<main>
	<button
		on:click={() => {
			const network = new myc.Network();
			randomGraph(network, 200, 300);
			viewer.setNetwork(network);
		}}>Randomize</button
	>
	<div bind:this={el} style="width: 100vw; height: 100vh;" />
</main>

<style>
</style>
