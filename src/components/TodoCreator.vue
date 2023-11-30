<script setup lang="ts">
import TodoButton from './TodoButton.vue'
import { reactive } from 'vue'

const emit = defineEmits<{
    (e: 'create-todo', todo: string): void
}>()

interface TodoState {
    todo: string
    invalid: null | boolean
    errMsg: string
}

const todoState: TodoState = reactive({
    todo: "",
    invalid: null,
    errMsg: "",
});

const createTodo = () => {
    todoState.invalid = null;
    if (todoState.todo !== '') {
        emit("create-todo", todoState.todo);
        todoState.todo = '';
        return
    };
    todoState.invalid = true;
    todoState.errMsg = 'Todo value cannot be empty';
}

</script>

<template>
    <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
        <input type="text" v-model="todoState.todo" />
        <TodoButton @click="createTodo()">Create</TodoButton>
    </div>
    <!-- v-show instead of v-if because the error message is going to be toggled more often -->
    <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
    display: flex;
    transition: 250ms ease;
    border: 2px solid #41b080;

    &.input-err {
        border-color: red;
    }

    &:focus-within {
        box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
            0 -2px 4px -2px rgb(0 0 0 / 0.1);
    }

    input {
        width: 100%;
        padding: 8px 6px;
        border: none;

        &:focus {
            outline: none;
        }
    }
}

.err-msg {
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>