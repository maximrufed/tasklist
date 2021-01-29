<script>
	import Task from "./Task.svelte";
	import AddTask from "./AddTask.svelte";
	import { Container, Row, Col } from "sveltestrap";
	// Dispatcher

	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();

	function update() {
		dispatch("update");
	}

	// Dispatcher - end

	export let tasks;
</script>

<Container>
	{#each tasks as task}
		<Row>
			<Col>
				<Task bind:task on:update={update} />
			</Col>
		</Row>
	{/each}

	<Row>
		<Col>
			<AddTask on:update={update} />
		</Col>
	</Row>
</Container>

<!--<style>
	.frame {
		border-width: 1px;
		border-style: solid;
		width: 300px;
	}
</style>-->
