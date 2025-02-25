<script lang="ts">
    import { fade } from "svelte/transition";
	import type { Task } from '$lib/types';

	let {
		tasks,
        toggleDone,
        removeTask,
	}: {
		tasks: Task[];
        toggleDone: (task: Task) => void;
        removeTask: (id: string) => void;
	} = $props();
</script>

<section>
	{#each tasks as task}
		<article class="task" transition:fade>
			<label>
				<input checked={task.done} onchange={() => toggleDone(task)} type="checkbox" />
				<span class:done={task.done}>{task.title}</span>
			</label>
            <!-- remove task, like tasks and toggledone, is a prop -->
            <button
                onclick={() => removeTask(task.id)}
                class="outline">
                    Remove
            </button>
		</article>
	{/each}
</section>

<style>
    .done {
        /* crosses out the finished tasks */
        text-decoration: line-through;
    }
    .task {
        /* pushes the button to the right */
        display: flex;
        justify-content: space-between;
        /* vertical align */
        align-items: center;
    }
</style>
