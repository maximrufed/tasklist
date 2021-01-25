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
				<!-- <span on:click={onAdd}>
				<Icon name="arrow-right" />
			</span> -->
			</Col>
		</Row>
	</Container>
</Card>
<!-- 
<div class="frame">
	<input bind:value={bufer} />
</div>

<div class="frame">
	<button on:click={onAdd}>add</button>
</div> -->
<!--<style>
	.frame {
		/*border-width: 1px;*/
		/*border-style: solid;*/
		margin: 10px 10px 10px 10px;
	}
</style>-->
