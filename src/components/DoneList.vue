<template>
    <div class="task-list">
        <h1>Завершенные задачи</h1>
        <ul>
            <li v-if="dones.length === 0" class="empty"><p>Тeкущих задач нет</p></li>
            <li v-for="done in dones" :done="done" :key="done.id" class="task passed">
                <span style="width: 110"></span>
                <span class="name">{{done.taskName}}</span>
                <span class="end-time">{{done.doneTime}}</span>
                <span class="end-date">{{done.doneDate}}</span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'DoneList',
    props: {
        dones: Array,
    }
}
</script>

<style>
    .task-list {
        border-bottom: 5px solid #fff;
    }
    .task {
        margin:5px;
        padding: 10px;
        font-size: 18px;
        text-align: center;
        border-radius: 10px;
    }
    .empty {
        width: calc(100% - 20px);
    }
    .passed {
        width: calc(100% - 40px);
        padding: 15px 10px;
        background-color: lightgrey;
    }
    .end-time {
        width: 80px;
    }
    .end-date {
        width: 120px;
    }
</style>