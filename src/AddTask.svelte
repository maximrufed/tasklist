<script>
	// Dispatcher

	import { createEventDispatcher } from "svelte";
	import {
		Container,
		Row,
		Col,
		Input,
		Icon,
		Button,
		Card,
	} from "sveltestrap";
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

<Card body>
	<Container>
		<Row>
			<Col xs="auto">
				<Input id="taskText" bind:value={bufer} />
			</Col>
			<Col xs="auto">
				<Button on:click={onAdd}>add</Button>
			</Col>
		</Row>
	</Container>
</Card>
