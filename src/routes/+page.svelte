<script>
// @ts-nocheck
	import toast from 'svelte-french-toast';
	import axios from 'axios';
	let baseUrl = 'http://localhost:8080/'
	let todos = [];

	const todo = {task: "", status: "INSERT", taskStatus: "PENDING"}

	const getTodos = async () => {
		try {
            const res = await axios.get(baseUrl + "todo/");
            todos = res.data;
			console.log(res.data);
        } catch (e) {
            console.log(e);
        }
	}

	getTodos()

	const completeTodo = async (item) => {
		try {
            const res = await axios.put(baseUrl + "todo/" + item.ID, {...item, taskStatus: "COMPLETED"});
			getTodos()
            toast.success("Todo Updated successfully!")
        } catch (e) {
            console.log(e);
        }
	}

	const deleteTodo = async (id) => {
		try {
            const res = await axios.delete(baseUrl + "todo/" + id);
			getTodos()
            toast.success("Todo Deleted successfully!")
        } catch (e) {
            console.log(e);
        }
	}

	const createTodo = async () => {
		const forms = document.forms
		const form = forms.todoForm
		const task = form.task.value
		console.log(task);
		if(task === '' || task === undefined || task === null) {
			toast.error("Task field is empty!")
			return
		}
		try {
            const res = await axios.post(baseUrl + "todo/", {...todo, Task: task});
            toast.success("Todo added successfully!")
			form.reset()
			getTodos()
        } catch (e) {
            console.log(e);
        }
	}

</script>

<svelte:head>
	<title>Todo Application</title>
	<meta name="description" content="A simple todo application" />
</svelte:head>

<section>
	<form class="flex items-center" name="todoForm">   
		<div class="relative w-full">
			<div class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none">
				<svg class="w-4 h-4 text-gray-500 " aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 20">
					<path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5v10M3 5a2 2 0 1 0 0-4 2 2 0 0 0 0 4Zm0 10a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm12 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm0 0V6a3 3 0 0 0-3-3H9m1.5-2-2 2 2 2"/>
				</svg>
			</div>
			<input type="text" id="task" name="task" class="w-full sm:w-[500px] bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block ps-10 p-2.5" placeholder="Enter Task ..." required>
		</div>
		<button type="button" on:click={createTodo} class="p-2.5 ms-2 text-sm font-medium text-white bg-blue-700 rounded-lg border border-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300">
			Add
		</button>
	</form>
	<div class="w-full sm:w-[560px] border mt-12 shadow-xl rounded-lg">
		{#each todos as item}
			<div class="flex justify-between items-center py-2 px-2">
				<div class="text-xl font-bold">{item.task}</div>
				<div class="flex items-center gap-x-2">
					<button type="button" disabled={item.taskStatus === "COMPLETED"} on:click={() => completeTodo(item)} class={`${item.taskStatus === "COMPLETED" ? 'cursor-not-allowed bg-gray-600' : 'bg-green-700 hover:bg-green-800'} focus:outline-none text-white  focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 `}>
						Completed
					</button>
					<button type="button" on:click={() => deleteTodo(item.ID)} class="focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">
						Delete
					</button>
				</div>
			</div>
			<hr>
		{/each}
	</div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	
</style>
