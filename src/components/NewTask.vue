<template>
<form class="new-task">
    <h1>Новая задача</h1>
    
    <input 
        type="text" 
        id="taskName" 
        placeholder="Название задачи" 
        autocomplete="off" 
        :value="taskName" 
        @input="$emit('input-name', $event.target.value)"
        class="task-name"
        :class="priority"
    />
    <input
        type="text" 
        id="task-description"
        placeholder="Описание задачи"
        autoComplete="off"
        :value="taskDescription"
        @input="$emit('input-description', $event.target.value)"
        class="task-description"
        :class="priority"
      />
    <label class="task-time" :class="priority">
        Время окончания
        <input 
            class="clock"
            id="clock"
            type="time"
            :value="endTime"
            @change="$emit('input-time', $event.target.value)"  
        />
    </label>

    <label class="task-date" :class="priority">
        Дата окончания
        <input 
            class="calendar"
            id="calendar"
            type="date"
            min= "2019-08-01"
            max="2100-12-31"
            :value="endDate"
             @change="$emit('input-date', $event.target.value)" 
        />
    </label>

    <label class="task-radio low"><input
        type="radio"
        name="priority"
        value="low"
        :checked="priority === 'low'"
        @change="$emit('priority-select', $event.target.value)"
      />Обычная</label>
      <label class="task-radio med"><input
        type="radio"
        name="priority"
        value="med"
        :checked="priority === 'med'"
        @change="$emit('priority-select', $event.target.value)"
      />Важная</label>
      <label class="task-radio high"><input
        type="radio"
        name="priority"
        value="high"
        :checked="priority === 'high'"
        @change="$emit('priority-select', $event.target.value)"
      />Очень важная</label>
    <button 
    v-on:click="$emit('task-add')"
    class="add"
    :disabled="taskName.trim() ? false : true"
    >Добавить</button>
</form> 
</template>

<script>
export default {
    name: 'NewTask',
    props: {
        taskName: String,
        taskDescription: String,
        endTime: String,
        endDate: String,
        priority: String,
    }
}
</script>

<style>
    .new-task {
        padding: 10px;
        font-size: 1em;
        text-align: center;
        border-top: 5px solid #fff;
        border-bottom: 5px solid #fff;
    }
    .task-name, .task-description {
        width: calc(100% - 20px);
        margin: 5px auto;
        padding: 10px;
        font-family: "Comic Sans MS", sans-serif;
        text-align: center;
        font-size: 1.1em;
        border-radius: 10px;
        border: none;
    }
    .task-name:focus,
    .task-name:active, 
    .task-description:focus,
    .task-description:active {
        outline: none;
        font-weight: bold;
    }
    .task-time,    
    .task-date {
        display: inline-block;
        width: calc(50% - 25px);
        min-width: 250px;
        margin: 5px 0;
        padding: 10px;
        font-family: "Comic Sans MS", sans-serif;
        font-size: 1.1em;
        text-align: center; 
        border-radius: 10px;
    }
    .task-time {
        margin-right: 5px;
    }
    .task-date {
        margin-left: 5px;
    }
    .clock,
    .calendar {
        margin-left: 20px;
        padding: 0 10px;
        font-family: "Comic Sans MS", sans-serif;
        font-size: 1.0em;
        background-color: #fff;
        border-radius: 10px;
        border: none;
    }
        .clock:focus,
        .clock:active,
        .calendar:focus,
        .calendar:active {
        outline: none;
    }
    .task-radio {
        display:inline-block;
        padding: 10px 5px;
        margin: 5px;
        width: 23%;
        min-width: 200px;
        border-radius: 10px;
    }
    .task-radio {
        width: 30%;
    }
    .add {
        width: calc(100% - 20px)
    }
    .low {
        background-color: #C0E5A5;
    }
    .med {
        background-color: #EFEF83;
    }
    .high {
        background-color: #E57472;
    }
</style>