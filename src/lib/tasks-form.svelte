<script lang="ts">
	// track state on newTask var
	let newTask = $state('');

	// accept, destructure, and type the addTask "callback prop" passed from +page.svelte
    // I think you do this to keep state-modifying code scoped to the component
    // where its state is defined...so you call addTask, which is defined in the
    // main app page, but you pass newTask, which is a state variable defined
    // in this component...addTask then modifies the tasks state in the main app page.
	let {
		addTask
	}: {
		addTask: (newTask: string) => void;
	} = $props();

	// handles the submit event from the form, calls addTask (callback prop),
	// and resets newTask, blanking out the form
	function formSubmitted(e: SubmitEvent) {
		e.preventDefault();
		addTask(newTask);
		newTask = '';
	}
</script>

<form onsubmit={formSubmitted}>
	<label>
		Task
		<input name="newTask" bind:value={newTask} />
	</label>
	<div class="button-container">
		<button>Add</button>
	</div>
</form>

<style>
	form {
		margin-bottom: 1rem;
	}
	.button-container {
		display: flex;
		justify-content: end;
	}
</style>
