<template>
    <form @submit="onAddTask" class="form">
        <input
            class="form__input"
            :value="(name = taskEdit.name ?? name)"
            @keyup="name = $event.target.value"
            type="text"
        />
        <button
            type="submit"
            :class="[
                'form__button',
                taskEdit ? 'button__update' : 'button__add'
            ]"
        >
            {{ taskEdit ? 'Update' : 'Add' }}
        </button>
    </form>
</template>

<script>
import { ref } from 'vue';

export default {
    name: 'AddTodo',
    props: ['taskEdit'],
    setup(props, context) {
        const name = ref('');

        const onAddTask = e => {
            e.preventDefault();
            if (!name.value) {
                alert('Hãy nhập tên nhiệm vụ');
                return;
            }
            if (props.taskEdit) {
                context.emit('onUpdateTask', name.value);
            } else {
                context.emit('onAddTask', name.value);
            }
            name.value = '';
        };

        return {
            onAddTask,
            name
        };
    }
};
</script>

<style scope>
.form {
    height: 2.5rem;
    display: flex;
    gap: 2rem;
    margin: 1rem 0;
}

.form input {
    height: 100%;
    width: 100%;
    border: 1px solid rgb(144, 144, 144);
    border-radius: 0.4rem;
    outline: none;
    font-size: 1.4rem;
    padding-left: 1rem;
}

.form button {
    font-size: 1.4rem;
    border: none;
    color: #fff;
    padding: 0.2rem 0.4rem;
    border-radius: 0.4rem;
    cursor: pointer;
}
.form .button__add {
    background: rgb(3, 95, 180);
}
.form .button__update {
    background: rgb(1, 122, 21);
}
</style>
