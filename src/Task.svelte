<script>
	// Dispatcher

	import { createEventDispatcher } from "svelte";
	import {
		Card,
		Icon,
		Input,
		CustomInput,
		Container,
		Row,
		Col,
	} from "sveltestrap";

	$: {
		fetch("/go/api/tasks/" + task.id.toString(), {
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
			// upd();
		}
	}

	async function onCheck() {
		// let a = await
	}
</script>

<Card body>
	<Container>
		<Row>
			<Col xs="10">
				<Input type="checkbox" bind:checked={task.done} inline />
				{#if edited}
					<Input id="taskText" bind:value={bufer} inline />
					<!-- <textarea class="text" bind:value={bufer} /> -->
				{:else}
					<!-- <Input bind:value={bufer} disabled="true" /> -->
					{task.text}
				{/if}
			</Col>
			<Col xs="1">
				<span on:click={onEdit}>
					<Icon name="pencil-square" />
				</span>
			</Col>
			<Col xs="1">
				<span on:click={onDelete}>
					<Icon name="trash" />
				</span>
			</Col>
		</Row>
	</Container>
	<!-- {task.done} -->

	<!-- <button on:click={onDelete}>x</button> -->
</Card>
