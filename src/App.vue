<template>
  <div class="app">
    <h1>Список задач</h1>
    <div class="timer">
        <span>{{time.slice(0,5)}}</span>
        <span>{{date}}</span>
    </div>
    <NewTask 
      :taskName="taskName"
      @input-name="taskName = $event"
      
      :taskDescription="taskDescription"
      @input-description="taskDescription = $event"

      :endTime="endTime"
      @input-time="endTime = $event"

      :endDate="endDate"
      @input-date="endDate = $event"

      :priority="priority"
      @priority-select="priority = $event" 
       
      @task-add="addTask"
    ></NewTask>
    <TaskFilter
      :filter="filter"
      @filter-select="filter = $event"
    ></TaskFilter>
    <TaskList
      :tasks="currentTasks"
    ></TaskList>
    <DoneList 
      :dones="done"
    ></DoneList>
  </div>
</template>

<script>
import EventBus from "./event-bus.js"
//import tasks from './data/tasks.js';
import NewTask from './components/NewTask.vue';
import TaskFilter from './components/TaskFilter.vue';
import TaskList from './components/TaskList.vue';
import DoneList from './components/DoneList.vue';
import { setInterval, clearInterval } from 'timers';

export default {
  name: 'app',
  data() {
    return {
      tasks: [],
      done: [],
      taskName: "",
      taskDescription: "",
      endTime: "",
      endDate: "",
      priority: "med", 
      time: "",
      date: "",
      filter: "all"
    }
  },
  computed: {
    currentTasks: function() {
      let list = this.tasks
      if (!list) {
        return [];
      } 
      list = this.tasks.map((item) => {
        let elem = item
        if (!item.endDate){
          elem.failed = false
          return elem
        } else {
          const date = Date.parse(item.endDate +" "+ item.endTime)
          const curdate = Date.parse(new Date())
          if (date < curdate) {
            elem.failed = true
            return elem
          } else {  
            elem.failed = false
            return elem
          }
        }
      });  
      
      if (this.filter === 'all') {
        return list
      } else {
        return list.filter((item) => {
          return item.priority === this.filter
        })  
      }
    },  
  },
  components: {
    "NewTask": NewTask,
    "TaskFilter": TaskFilter,
    "TaskList": TaskList,
    "DoneList": DoneList
  },
  methods:{
    load(){
      this.time = (new Date().toLocaleTimeString());
      this.date = (new Date().toLocaleDateString());
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    addTask() {
      this.tasks.unshift({
        id: +new Date(),  
        taskName: this.taskName,
        taskDescription: this.taskDescription,
        endTime: this.endTime,
        endDate: this.endDate,
        priority: this.priority
      });

      this.taskName = "";
      this.taskDescription = "";
      this.endTime = "";
      this.endDate = "";
      this.priority = "med";

      this.saveTasks();
    },
	},
  mounted(){
      this.interval = setInterval(() => this.load(), 1000);
      this.tasks = JSON.parse(localStorage.getItem('tasks'));
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  created() {
    EventBus.$on("task-modify", evt => {
      this.selectedFilter = evt.filter;
      this.taskName = evt.taskName;
      this.taskDescription = evt.taskDescription;
      this.endTime = evt.endTime;
      this.endDate = evt.endDate;
      this.priority = evt.priority;

      const newList = this.tasks.filter((item)=>{
        return item.id !== evt.id;
      });
      this.tasks = newList;
      this.saveTasks()
    });

    EventBus.$on("task-delete", evt => {
      const newList = this.tasks.filter((item)=>{
        return item.id !== evt.id;
      });
      this.tasks = newList;
      this.saveTasks() 
    });
    
    EventBus.$on("task-done", evt => {
      this.done.unshift({
        taskName: evt.taskName,
        doneTime: evt.doneTime,
        doneDate: evt.doneDate,  
      })

      const newList = this.tasks.filter((item)=>{
        return item.id !== evt.id;
      });
      this.tasks = newList;
      this.saveTasks()
    })
  }
}
</script>

<style>
  html {
    overflow-y: scroll;
  }
  body {
    background: #F7F3D2;
    font-family: "Comic Sans MS", sans-serif;
    font-size: 16px;
    color: #0F0326;
    letter-spacing: 0.1em; 
    user-select: none;
  }
  h1, h2 {
    margin: 0 0 10px;
    text-align: center;
  }
  p { 
    font-size: 18px;
    text-align: center;
    font-style: italic;
  }
  ul {
    list-style: none;
    padding:0;
  }

  li {
    display: inline-block;
    width: calc(100% - 40px);
    font-size: 0;
  }
  span {
    display: inline-block;
    margin: 0;
    padding: 5px 0;
    font-size: 1em;
  }
  button {
    display: inline-block;
    margin: 5px;
    padding: 5px 10px;
    font-size: 18px;
    text-align: center;
    border: none;
    border-radius: 10px;
    background-color: #fff;
    font-family: "Comic Sans MS", sans-serif;
  }
  button:focus {
    outline: none;
  }
  
  .app {
    width: 60vw;
    min-width: 300px;
    margin: 0 auto;
    padding: 20px;
  }
  .timer {
    display: block;
    text-align: center;
  }
  .timer span {
    display: inline-block;
    margin: 0 10px;
  }
</style>
