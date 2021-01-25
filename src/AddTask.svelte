<script>
	// Dispatcher

	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();

	function update() {
		dispatch("update");
	}

	// Dispatcher - end

	let bufer = "";

	async function onAdd() {
		if (bufer == "") return;
		let a = await fetch("/go/api/tasks", {
			method: "POST",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify({
				done: false,
				text: bufer,
			}),
		});
		bufer = "";
		update();
	}
</script>

<div class="frame">
	<input bind:value={bufer} />
</div>

<div class="frame">
	<button on:click={onAdd}>add</button>
</div>
<!--<style>
	.frame {
		/*border-width: 1px;*/
		/*border-style: solid;*/
		margin: 10px 10px 10px 10px;
	}
</style>-->
