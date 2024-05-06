<script lang="ts">
  import type { Task } from "../data/task";
  import {
    buildTaskTree,
    type TaskTree,
  } from "../data/transformations/relationships";
  import { sortTasks } from "../data/transformations/sorting";
  import TaskList from "./TaskList.svelte";
  import { getQuery } from "./contexts";

  export let tasks: Task[];
  let query = getQuery();

  $: taskTrees = getTaskTree(tasks);

  function getTaskTree(tasks: Task[]): TaskTree[] {
    if (query.showProjectNameOnly) {
      let projectLabelSet = new Set<string>();
      let arrangedTasks = []
      tasks.map(task => {
        if (!projectLabelSet.has(task.project?.id)) {
          projectLabelSet.add(task.project?.id);
          arrangedTasks.push(task);
        }
      })
      sortTasks(arrangedTasks, query.sorting);
      return buildTaskTree(arrangedTasks);
    }
    sortTasks(tasks, query.sorting);
    return buildTaskTree(tasks);
  }
</script>

<TaskList {taskTrees} />
