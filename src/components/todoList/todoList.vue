<template>
    <div class="todoList">
        <div class="headerContainer">

            <h2 class="header"
                title="click to Apply/Remove sorting"
                @click="applySort"> 
                    My Todo List
            </h2>

            <div class="icon1"
                :class="{up: sortBy==='asce', down: sortBy==='desc'}"
                @click="deleteAll">
                    <Arrow />
            </div>

            <div title="Delete All" 
                class="icon" 
                @click="deleteAll">
                    <Delete />
            </div>

        </div>
        
        <AddTask v-on:add-task="addTask"></AddTask>
        <hr/>
        <div class="tasksContainer">
            <div v-for="task in tasks" :key="task.taskID">
                <Task :task="task"
                    v-on:edit-task="editTask"
                    v-on:delete-task="deleteTask"/>
            </div>
        </div>
        
    </div>    
</template>

<script>

import Task from '../task/task'
import AddTask from '../addTask/addTask'
import Delete from '../../assets/delete'
import Arrow from '../../assets/arrow'

export default {
    name: 'TodoList',
    components: {
        Task,
        AddTask,
        Delete,
        Arrow
    },
    data() {
        return {
            tasks: [
                {
                    taskID: 1,
                    title: 'Go workout',
                    completed: false
                },
                {
                    taskID: 2,
                    title: 'Do laundry',
                    completed: false
                },
                {
                    taskID: 3,
                    title: 'Cook food',
                    completed: false
                },
                {
                    taskID: 4,
                    title: 'Clean up room',
                    completed: false
                },
                {
                    taskID: 5,
                    title: 'Finish work',
                    completed: false
                }
            ],
            maxID: 5,
            sortBy: ''
        }
    },
    methods: {
        addTask(newTaskObj){
            newTaskObj.taskID = this.maxID+1;
            this.tasks = [...this.tasks,newTaskObj];
            this.maxID += 1;
            this.reOrderTasks();
        },
        editTask(modifiedTaskObj){
            this.tasks.forEach(task => {
                if(task.taskID === modifiedTaskObj.taskID)
                    task.completed = modifiedTaskObj.completed
            });
            this.reOrderTasks();
        },
        deleteTask(taskID){
            this.tasks = this.tasks.filter(task => task.taskID !== taskID);
        },
        deleteAll(){
            this.tasks = [];
        },
        applySort(){
            
            if(this.sortBy==='')this.sortBy = 'asce'
            else if(this.sortBy==='asce')this.sortBy = 'desc'
            else this.sortBy = ''

            this.reOrderTasks();
        },
        reOrderTasks(){

            let t1 = this.tasks.filter(task => !task.completed);
            let t2 = this.tasks.filter(task => task.completed);

            if(this.sortBy === 'desc'){
                t1 = t1.sort( (e1, e2) => e2.title.toUpperCase().localeCompare(e1.title.toUpperCase()) )
                t2 = t2.sort( (e1, e2) => e2.title.toUpperCase().localeCompare(e1.title.toUpperCase()) )
            }else if(this.sortBy === 'asce'){
                t1 = t1.sort( (e1, e2) => e1.title.toUpperCase().localeCompare(e2.title.toUpperCase()) )
                t2 = t2.sort( (e1, e2) => e1.title.toUpperCase().localeCompare(e2.title.toUpperCase()) )
            }
            this.tasks = [...t1,...t2];
        }
    }
}
</script>

<style scoped>

.todoList {
    width: 450px;
    margin-left: auto;
    margin-right: auto;
}

hr {
    margin: 0;
    width: 430px;
    margin-top: 5px;
    border-left: 0;
    border-right: 0;
    border-top: 0.5px solid #000;
    border-bottom: 0.5px solid #000;
}

.headerContainer {
    display: flex;
    margin-bottom: 20px;
}

.header {
    margin: 0px;
}

.tasksContainer{
    width: 450px;
    max-height: 400px;
    overflow-y: auto;
}

.icon {
    display: inline-block;
    width: 24px;
    height: 24px;
    margin-right: 20px;
    margin-left: auto;
}

.icon1 {
    display: none;
    width: 24px;
    height: 24px;
}

.up {
    display: inline;
    transform: rotate(-90deg);
}

.down {
    display: inline;
    transform: rotate(90deg);
}

</style>