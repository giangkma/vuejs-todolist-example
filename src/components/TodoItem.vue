<template>
    <li>
        <div class="list__left">
            <input
                class="input_checkbox"
                type="checkbox"
                :checked="todoProp.done"
                @change="onToggleDone"
            />
            <p :class="[todoProp.done ? 'underline' : '']">
                {{ todoProp.name }}
            </p>
        </div>
        <div>
            <svg
                @click="onEditTask"
                class="pen-icon"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"
                ></path>
            </svg>
            <svg
                @click="onRemoveTask"
                class="trash-icon"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
            >
                <path
                    fill-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    clip-rule="evenodd"
                ></path>
            </svg>
        </div>
    </li>
</template>

<script>
export default {
    name: 'TodoItem',
    props: ['todoProp'],
    setup(props, context) {
        const onEditTask = () => {
            context.emit('onEditTask', props.todoProp.id);
        };
        const onRemoveTask = () => {
            context.emit('onRemoveTask', props.todoProp.id);
        };
        const onToggleDone = () => {
            context.emit('onToggleDone', props.todoProp.id);
        };
        return {
            onEditTask,
            onRemoveTask,
            onToggleDone
        };
    }
};
</script>

<style scope>
ul li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: rgb(163, 163, 163);
    padding: 1rem;
    border-radius: 0.5rem;
    margin-bottom: 0.5rem;
}
.input_checkbox {
    transform: scale(2);
}
li .list__left {
    display: flex;
    gap: 2rem;
    align-items: center;
}

li p {
    font-size: 1.4rem;
}

li svg {
    width: 40px;
    cursor: pointer;
}

li input {
    transform: scale(1.4);
}
li .pen-icon {
    color: rgb(66, 66, 66);
}
li .trash-icon {
    color: rgb(206, 14, 14);
}
</style>
