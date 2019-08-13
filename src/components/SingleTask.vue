<template>
    <li class="task" :class="task.priority + (task.failed ? ' failed' : '')" @click="expand = !expand">
        <button class="done" :disabled="task.failed" @click="taskDone">Готово</button>
        <span class="name">{{ task.taskName }}</span>
        <span class="end-time">{{ task.endTime }}</span>
        <span class="end-date">{{ ddmmyyyy }}</span>
        <span class="description" v-if="expand">
            {{ task.description ?  task.description : 'Нет описания' }}
        </span>
        <button class="modify" v-if="expand" @click="taskModify">Редактировать</button>
        <button class="delete" v-if="expand" @click="taskDelete">Удалить</button>
    </li>
    
</template>

<script>
import EventBus from "../event-bus.js";
export default {
    name: 'SingleTask',
    data() {
        return {
            expand: false
        }
    },
    props: {
        task: Object,
    },
    computed: {
        ddmmyyyy: function() { 
            const eDate = this.task.endDate
            return eDate ? eDate[8]+eDate[9]+'-'+eDate[5]+eDate[6]+'-'+eDate[0]+eDate[1]+eDate[2]+eDate[3] : null}
    },
    
    methods: {
        taskDone() {
            EventBus.$emit("task-done", {
                taskName: this.task.taskName,
                doneTime: new Date().toLocaleTimeString().slice(0,5),
                doneDate: new Date().toLocaleDateString(),
                id: this.task.id,
            });   
        },
        taskModify() {
            EventBus.$emit("task-modify", {
                taskName: this.task.taskName,
                taskDescription: this.task.description,
                endTime: this.task.endTime,
                endDate: this.task.endDate,
                priority: this.task.priority,
                id: this.task.id
            });
        },
        taskDelete() {
            EventBus.$emit("task-delete", {
                id: this.task.id
            });
        }
    },
}
</script>

<style>
    .task {
        margin:5px;
        padding: 10px;
        font-size: 18px;
        text-align: center;
        border-radius: 10px;
    }
    .done {
        width: 120px;
    }
    .modify,
    .delete {
        width: calc(50% - 10px);
        min-width: 200px;
    }
    .name {
        width: calc(100% - 110px - 130px - 90px);
        min-width: 200px;
    }
    .name:first-letter {
        text-transform: capitalize;
    }
    .end-time {
        width: 80px;
    }
    .end-date {
        width: 120px;
    }
    .description {
        width: 100%;
    }
    .low {
        background-color:#C0E5A5;
    }
    .med {
        background-color: #EFEF83;
    }
    .high {
        background-color: #E57472;
    }
    .failed {
        background: repeating-linear-gradient(
            45deg,
            #E57472,
            #E57472 10px,
            #fff 10px,
            #fff 20px
        );
    }
</style>