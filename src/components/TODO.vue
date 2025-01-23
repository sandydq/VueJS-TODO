<script setup>

import { ref, computed } from 'vue'

const todoValue = ref('')
const todoList = ref(JSON.parse(localStorage.getItem('toDoList')) || [])

function addToDo(params) {
    todoList.value.push({
        done: false,
        value: params
    })
    todoValue.value = ""
}

function removeToDo(index) {
    todoList.value.splice(index, 1)
}

function completeToDo(todo) {
    todo.done = !todo.done
}

function saveIntoStorage() {
    localStorage.setItem('toDoList', JSON.stringify(todoList.value))
}

function deleteStorage() {
    localStorage.setItem('toDoList', null)
}

const isToDoEmpty = computed(() => {
    return todoList.value.length > 0 ? false : true
})
</script>

<template>
    <div class="container">
        <h1> TODO APP</h1>
        <form @submit.prevent="addToDo(todoValue)">
            <label>New ToDo</label>
            <input v-model="todoValue" autocomplete="off" name="newToDo">
            <button>Add Todo</button>
        </form>
        <h2>ToDo List</h2>
        <ul>
            <li v-for="(todo, index) in todoList" :key="index">
                <span :class="{ done: todo.done }" @click="completeToDo(todo)">{{ todo.value }}</span>
                <button @click="removeToDo(index)">Remove</button>
            </li>
        </ul>
        <span v-if="isToDoEmpty">TODO is Empty!</span>
        <div>
            <button class="save" @click="saveIntoStorage">Save</button>
            <button class="delete" @click="deleteStorage" >Delete</button>
        </div>
    </div>

</template>

<style lang="scss">
$border: 2px solid rgba($color: white,
        $alpha: 0.35,
    );
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #131212;
$textColor: white;
$primaryColor: hsl(170, 68%, 31%);
$secondTextColor: #e5eeee;

.delete {
    margin-left: 82%;
}

.container {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    margin: 0;
    padding: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background-color: $backgroundColor;
    color: $textColor;

}

h1 {
    font-weight: bold;
    font-size: 28px;
    text-align: center;
}

h2 {
    font-size: 22px;
    border-bottom: $border;
    padding-bottom: $size1;
}

.done {
    text-decoration: line-through;
}

label {
    font-size: 14px;
    font-weight: bold;
}

form {
    display: flex;
    flex-direction: column;
    width: 100%;

}

form>button,
input {
    height: $size5;
    box-shadow: none;
    outline: none;
    padding-left: $size2;
    padding-right: $size2;
    border-radius: $size1;
    font-size: 18px;
    margin-top: $size1;
    margin-bottom: $size2;
}

button {
    cursor: pointer;
    background-color: $primaryColor;
    border: 1px solid $primaryColor;
    color: $secondTextColor;
    font-weight: bold;
    outline: none;
    border-radius: $size1;
}

input {
    background-color: transparent;
    border: $border;
    color: inherit;
}

ul {
    padding: 10px;

    li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $size2 $size4;
        border-radius: $size1;
        margin-bottom: $size2;

        span {
            cursor: pointer;
        }

        .done {
            text-decoration: line-through;
        }

        button {
            font-size: $size2;
            padding: $size1;
        }
    }
}
</style>