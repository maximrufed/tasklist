<script>
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

	const dispatch = createEventDispatcher();

	function update() {
		dispatch("update");
	}

	export let task;
	let edited = 0;
	let bufer = "";

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

	async function onDelete() {
		await fetch("/go/api/tasks/" + task.id.toString(), {
			method: "DELETE",
		});
		update();
	}

	function onEdit() {
		if (!edited) {
			edited = 1;
			bufer = task.text;
		} else {
			edited = 0;
			task.text = bufer;
		}
	}
</script>

<Card body>
	<Container>
		<Row>
			<Col xs="10">
				<Input type="checkbox" bind:checked={task.done} />
				{#if edited}
					<Input id="taskText" bind:value={bufer} />
				{:else}
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
</Card>
