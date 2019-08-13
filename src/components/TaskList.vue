<template>
<div class="task-list">
  
  <h2>Текущие задачи</h2>
  <ul>
      <li v-if="tasks.length === 0"><p>Текущих задач нет</p></li>
      <single-task v-for="task in tasks" :task="task" :key="task.id"></single-task>
  </ul>
</div> 
</template>

<script>

import SingleTask from "./SingleTask.vue";

export default {
    name: 'TaskList',
    props: {
        tasks: Array,
    },
    components: {
        "single-task": SingleTask,
    }
}
</script>

<style>
.task-list {
    border-bottom: 5px solid #fff;
}

</style>