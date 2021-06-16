<template>
    <div class="container">
        <h1>Todo List</h1>
        <AddTodo
            :taskEdit="taskEdit"
            @onUpdateTask="onUpdateTask"
            @onAddTask="onAddTask"
        />
        <ul class="list__task">
            <TodoItem
                v-for="todo in todos"
                :key="todo.id"
                :todoProp="todo"
                @onToggleDone="onToggleDone"
                @onEditTask="onEditTask"
                @onRemoveTask="onRemoveTask"
            />
            <p>{{ name }}</p>
        </ul>
    </div>
</template>

<script>
import { ref } from 'vue';
import AddTodo from './components/AddTodo.vue';
import TodoItem from './components/TodoItem.vue';

export default {
    name: 'App',
    components: {
        AddTodo,
        TodoItem
    },
    setup() {
        const getTasks = () => {
            return JSON.parse(localStorage.getItem('tasks'));
        };

        const saveTasks = tasks => {
            localStorage.setItem('tasks', JSON.stringify(tasks));
        };

        const todos = ref(getTasks() ?? []);

        const taskEdit = ref('');

        const onAddTask = name => {
            todos.value.push({
                id: todos.value.length + 1,
                name: name,
                done: false
            });
            saveTasks(todos.value);
        };

        const onUpdateTask = name => {
            todos.value = todos.value.map(item => {
                if (item.id === taskEdit.value.id) {
                    return {
                        ...taskEdit.value,
                        name: name,
                        done: false
                    };
                }
                return item;
            });
            taskEdit.value = '';
            saveTasks(todos.value);
        };

        const onToggleDone = id => {
            todos.value = todos.value.map(item => {
                if (item.id === id) {
                    item.done = !item.done;
                }
                return item;
            });
            saveTasks(todos.value);
        };

        const onEditTask = id => {
            taskEdit.value = todos.value.find(item => item.id === id);
            saveTasks(todos.value);
        };

        const onRemoveTask = id => {
            todos.value = todos.value.filter(item => item.id !== id);
            saveTasks(todos.value);
        };

        return {
            todos,
            taskEdit,
            onAddTask,
            onToggleDone,
            onEditTask,
            onRemoveTask,
            onUpdateTask
        };
    }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');
* {
    padding: 0;
    margin: 0;
}

body {
    font-size: 62.5%;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
    font-family: 'Quicksand', sans-serif;
}
.container {
    width: 30rem;
    background: rgb(207, 207, 207);
    padding: 2rem;
    border-radius: 2rem;
    overflow: hidden;
}
.list__task {
    max-height: 40rem;
    overflow: auto;
}
h1 {
    font-size: 2.4rem;
}
ul {
    list-style: none;
}
.underline {
    text-decoration: line-through;
}
</style>
