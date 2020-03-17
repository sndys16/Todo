<template>
    <div>
        <form @submit.prevent="addTask" class="formContainer">
            <input type="text" v-model="title" name="title">
            <button type="submit">Add</button>
        </form>
    </div>
</template>

<script>

export default {
    name: "AddTask",
    data() {
        return {
            title: ''
        }
    },
    methods: {
        addTask() {
            const newTaskObj = {
                title: this.title,
                completed: false
            }
            this.$emit('add-task', newTaskObj);
            this.title = '';
        }
    }
}

</script>

<style scoped>

.formContainer {
    display: flex;
}

input {
    height: 24px;
    width: 350px;
    font-size: 20px;
    font-family: times new roman;
}

button {
    margin-left: 2px;
    height: 30px;
    width: 78px;
    border: 0;
    background-color: rgb(0, 123, 255);
    color: white;
    border-radius: 4px;
}

</style>