<script>
	// Dispatcher

	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();

	function update() {
		dispatch("update");
	}

	// Dispatcher - end

	export let task;

	let edited = 0;
	let bufer = "";

	async function onDelete() {
		await fetch("/go/api/tasks/" + task.id.toString(), {
			method: "DELETE",
		});
		update();
	}

	async function upd() {
		let a = await fetch("/go/api/tasks/" + task.id.toString(), {
			method: "PUT",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify({
				done: task.done,
				text: task.text,
			}),
		});
	}

	function onEdit() {
		if (!edited) {
			edited = 1;
			bufer = task.text;
		} else {
			edited = 0;
			task.text = bufer;
			upd();
		}
	}

	async function onCheck() {
		let a = await fetch("/go/api/tasks/" + task.id.toString(), {
			method: "PUT",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify({
				done: !task.done,
				text: task.text,
			}),
		});
	}
</script>

<div class="frame">
	<!-- {task.done} -->
	<div class="checkbox">
		<input type="checkbox" bind:checked={task.done} on:click={onCheck} />
	</div>

	<div class="text">
		{#if edited}
			<textarea class="text" bind:value={bufer} />
		{:else}
			{task.text}
		{/if}
	</div>

	<div class="edit_button">
		<button on:click={onDelete}>x</button>
	</div>

	<div class="edit_button">
		<button on:click={onEdit}>edit</button>
	</div>
	<div class="clear" />
</div>
<!--<style>
	.frame {
		border-width: 1px;
		border-style: solid;
		margin: 10px 10px 10px 10px;
	}

	.checkbox {
		float: left;
		position: relative;
		margin-top: 10px;
		margin-left: 10px;
		margin-right: 10px;
		width: 20px;
	}

	.text {
		float: left;
		width: 150px;
		word-wrap: break-word;
	}

	.edit_button {
		float: left;
		margin-top: 10px;
		margin-left: 5px;
		/*margin-right: 10px;*/
	}

	.clear {
		clear: both;
	}
</style>-->
