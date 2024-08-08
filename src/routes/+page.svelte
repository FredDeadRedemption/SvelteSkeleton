<script>
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	let frame;
	let numCells = 2240;

	onMount(() => {
		if (!browser) return;

		for (let i = 0; i < numCells; i++) {
			let div = html.createElement('div');
			div.cla;

			frame.appendChild();
		}

		frame.appendChild();
	});
</script>

<div id="wrap">
	<!--<a href="/Matas-gavekort.pdf" target="_blank" download>Download</a>-->
	<div bind:this={frame} id="frame"></div>
</div>

<!-- 2240 cells -->

<style lang="scss">
	#wrap {
		height: 100vh;
		width: 100vw;
		background-image: url('/bg.jpg');
		background-size: cover;
		background-repeat: no-repeat;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		gap: 20px;
	}
	#frame {
		border: 1px solid $primary;
		width: 68%;
		height: 75%;
		display: grid;
		grid-template-columns: repeat(56, 1fr);
		grid-template-rows: repeat(40, 1fr);
	}
	.cell {
		border: 1px solid $primary;
	}
</style>
