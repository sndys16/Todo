<template>
    <div class="task">

        <div v-show="!task.completed" 
            title="Mark Complete"
            class="icon" 
            @click="updateTask">
                <RadioButton />
        </div>
        <div v-show="task.completed" 
            title="Mark Incomplete" 
            class="icon" 
            @click="updateTask">
                <Done />
        </div>

        <div v-if="!editMode" class="taskContainer">
            <p class="taskTitle"> {{ task.title }} </p>
            <div title="Edit details"
                class="icon" 
                @click="enableEdit">
                    <Edit />
            </div>
            <div title="Delete" 
                class="icon" 
                @click="deleteTask">
                    <Delete />
            </div>
        </div>

        <div v-else class="taskEditBox">
            <form @submit.prevent="updateTaskTitle">
                <input v-model="task.title" 
                    class="taskDetails"
                    id="task.taskID"
                    autofocus
                    @blur="updateTaskTitle"/>
            </form>
        </div>

    </div>
</template>

<script>

import Done from '../../assets/done';
import RadioButton from '../../assets/radioButton'
import Edit from '../../assets/edit';
import Delete from '../../assets/delete';

export default {
    name: 'Task',
    props: {
        task: {
            required: true
        }
    },
    components: {
        Done,
        RadioButton,
        Edit,
        Delete
    },
    data() {
        return {
            editMode: false
        }
    },
    methods:{
        updateTask() {
            let modifiedTaskObj = {}
            modifiedTaskObj.title = this.task.title
            modifiedTaskObj.taskID = this.task.taskID
            modifiedTaskObj.completed = !this.task.completed
            this.$emit('edit-task', modifiedTaskObj);
        },
        updateTaskTitle(e) {
            this.editMode = false
            let modifiedTaskObj = {}
            modifiedTaskObj.title = e.target.value
            modifiedTaskObj.taskID = this.task.taskID
            modifiedTaskObj.completed = this.task.completed
            this.$emit('edit-task', modifiedTaskObj);
        },
        enableEdit() {
            this.editMode = true;
            // document.getElementById(this.task.taskID).focus();
        },
        deleteTask() {
            this.$emit('delete-task',this.task.taskID);
        }
    }
}
</script>

<style scoped>

.task{
    width: 430px;
    min-height: 40px;
    height: auto;
    border-bottom: 0.5px solid black;
    display: flex;
    align-items: center;
}

.icon {
    width: 24px;
    height: 24px;
}

.task:hover {
    margin-bottom: 5px;
    box-shadow: 0 1px 2px 0 rgba(60,64,67,0.302), 0 2px 6px 2px rgba(60,64,67,0.149);
    outline: 0;
    z-index: 2000;
}

.taskContainer{
    width: 406px;
    display: inline-flex;
    justify-content: flex-end;
}

.taskEditBox {
    width: 406px;
}

.taskTitle {
    margin: 0;
    margin-left: 5px;
    height: auto;
    width: 358px;
    font-size: 20px;
    display: inline-block;
    word-wrap: break-word;
}

.taskDetails{
    height: 24px;
    font-size: 20px;
    font-family: times new roman;
    border: 0;
    margin-left: 5px;
    outline: none;
    width: 400px;
}
</style>