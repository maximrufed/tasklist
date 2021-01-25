<script>
	import List from "./List.svelte";
	import Task from "./Task.svelte";

	let tasks = {};

	async function get_tasks() {
		let a = await fetch("/go/api/tasks");
		let b = await a.text();
		tasks = JSON.parse(b);
	}

	get_tasks();

	// Dispatcher

	function update() {
		get_tasks();
	}

	// Dispatcher - end

</script>

<h1>List</h1>

<List tasks={ tasks } on:update={ update }/>
