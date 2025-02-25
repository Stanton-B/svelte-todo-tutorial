<script lang="ts">
	import type { Task } from '$lib/types';
	import TasksForm from '$lib/tasks-form.svelte';
	import TasksList from '$lib/tasks-list.svelte';

	let title = 'Tasks App';
    let currentFilter = $state<Filter>("all");

	// tasks is passed to the TasksList component as a prop
	let tasks = $state<Task[]>([]);
    // whenever tasks changes, this reruns as a derived rune.
    let totalDone = $derived(
        tasks.reduce(
            (total, task) => total + Number(task.done),
            0
        )
    )

    let filteredTasks = $derived.by(() => {
        switch(currentFilter) {
            case "all": {
                return tasks;
            }
            case "done": {
                return tasks.filter((task) => task.done)
            }
            case "todo": {
                return tasks.filter((task) => !task.done)
            }
        }
        // none of the filters match, then return all tasks
        return tasks;
    })

	// A "callback prop": passed to the component, the addTask callback prop
	// translates the newTask string to a Task object and
	// pushes the object to the tasks array.
	function addTask(newTask: string) {
		// deep state (detects an array.push via "state proxy")
		tasks.push({
			id: crypto.randomUUID(),
			title: newTask,
			done: false
		});
	}

    function toggleDone(task: Task) {
        task.done = !task.done;
    }

    function removeTask(id: string) {
        // findIndex is a js array method that returns the index of the 1st
        // element in the array that satisfies the test function provided.
        const index = tasks.findIndex(
            (task) => task.id === id
        );
        tasks.splice(index, 1);
    }
</script>

{#snippet filterButton(filter: Filter)}
    <button
        onclick={() => currentFilter = filter}
        class:contrast={currentFilter === filter}
        class="secondary filterButton">{filter}</button>
{/snippet}


<main>
	<h1>{title}</h1>
	<TasksForm {addTask} />
    <p>
        {#if tasks.length}
            {totalDone} / {tasks.length} tasks completed.
        {:else}
            Add a task to get started.
        {/if}
    </p>

        {#if tasks.length}
            <div class="button-container">
                {@render filterButton("all")}
                {@render filterButton("todo")}
                {@render filterButton("done")}
            </div>
        {/if}

	<TasksList
        tasks={filteredTasks}
        {toggleDone}
        {removeTask}
    />
</main>

<style>
	main {
		margin: 1rem auto;
		max-width: 800px;
	}

    .button-container {
        display: flex;
        justify-content: end;
        margin-bottom: 1rem;
        gap: 0.5rem;
    }

    .filterButton {
        text-transform: capitalize;
    }
</style>
