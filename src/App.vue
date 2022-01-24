<template>
    <div id="app">
        <header>
            <h1>Список задач на 2022</h1>
            <form v-on:submit.prevent="addTask" class="new-task-form">
                <input class="new-task-input" type="text" placeholder="Что вы хотите запланировать?" v-model="text" />
                <input class="new-task-submit" type="submit" value="Добавить задачу" />
            </form>
        </header>
        <main>
            <section class="task-list">
                <h2>Задачи</h2>
                <div id="task" :key="item.id" v-for="(item, id) in list">
                    <div class="task">
                        <div class="content">
                            <input class="text" type="text" v-model="item.text" :readonly="read ? true : false" />
                        </div>
                        <div class="actions">
                            <button class="edit" v-if="read" @click="editTask(id)">Изменить</button>
                            <button class="edit" v-else @click="editTask(id)">Применить</button>

                            <button class="delete" @click="removeTask(id)">Удалить</button>
                        </div>
                    </div>
                </div>
            </section>
        </main>
    </div>
</template>

<script>
import "boxicons";
export default {
    components: {},
    data() {
        return {
            read: true,
            text: "",
            inputText: "",
            nextTaskId: 1,
            list: [],
        };
    },
    mounted() {
        if (localStorage.getItem("list")) {
            try {
                this.list = JSON.parse(localStorage.getItem("list"));
            } catch (e) {
                localStorage.removeItem("list");
            }
        }
    },
    methods: {
        removeTask(id) {
            this.list.splice(id, 1);
            this.saveList();
        },

        editTask() {
            this.read = !this.read;
            this.saveList();
        },

        addTask() {
            if (this.text.trim() != "") {
                const newTask = {
                    id: this.nextTaskId,
                    text: this.text,
                    completed: false,
                };
                this.list.unshift(newTask);
                this.text = "";
                this.nextTaskId++;
                this.saveList();
            }
        },
        saveList() {
            const parsed = JSON.stringify(this.list);
            localStorage.setItem("list", parsed);
        },
    },
};
</script>

<style>
:root {
    --dark: #374151;
    --darker: #1f2937;
    --darkest: #111827;
    --gray: #6b7280;
    --light: #eee;
    --pink: #ec4899;
    --purple: #8b5cf6;
}

* {
    box-sizing: border-box;
    margin: 0;
    font-family: "Fira sans", sans-serif;
}

body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background: var(--dark);
    color: #fff;
}

header {
    padding: 2rem;
    min-width: 800px;
    width: 100%;
    margin: 0 auto;
}

header h1 {
    color: var(--gray);
    font-size: 2.5rem;
    font-weight: 300;
    margin-bottom: 1rem;
}

.new-task-form {
    display: flex;
}
.new-task-input {
    flex: 1 1 0%;
    background-color: var(--darker);
    padding: 1rem;
    margin-right: 1rem;
    border-radius: 1rem;
    color: var(--light);
    font-size: 1.25rem;
}

.new-task-input::placeholder {
    color: var(--gray);
}

.new-task-submit {
    color: var(--pink);
    font-size: 1.25rem;
    font-weight: 700;
    background-image: linear-gradient(to right, var(--pink), var(--purple));
    background-clip: text;
    -webkit-text-fill-color: transparent;
    cursor: pointer;
}

.new-task-submit:hover {
    opacity: 0.8;
}

.new-task-submit:active {
    opacity: 0.6;
}

input,
button {
    appearance: none;
    border: none;
    outline: none;
    background: none;
}

main {
    flex: 1 1 0%;
    padding: 2rem 1rem;
    min-width: 800px;
    width: 100%;
    margin: 0 auto;
}

.task-list {
    padding: 1rem;
}

.task-list h2 {
    font-size: 1.5rem;
    font-weight: 300;
    color: var(--gray);
    margin-bottom: 1rem;
}

#task .task {
    display: flex;
    justify-content: space-between;
    background-color: var(--darkest);
    padding: 1rem;
    border-radius: 1rem;
    margin-bottom: 1rem;
}

#task .task .content .text {
    color: var(--light);
    font-size: 1.125rem;
    width: 100%;
    display: block;
    transition: 0.4s;
}

#task .task .content .text:not(:read-only) {
    color: var(--pink);
}

#task .task .actions {
    display: flex;
    margin: 0 -0.5rem;
}

.delete,
.edit {
    display: none;
}

#task:hover .edit {
    display: block;
    cursor: pointer;
    margin: 0 0.5rem;
    font-size: 1.125rem;
    font-weight: 700;
    text-transform: uppercase;
    transition: 0.4s;
}

#task:hover .delete {
    display: block;
    cursor: pointer;
    margin: 0 0.5rem;
    font-size: 1.125rem;
    font-weight: 700;
    text-transform: uppercase;
    transition: 0.4s;
    color: crimson;
}

.task .actions button:hover {
    opacity: 0.8;
}

.task .actions button:active {
    opacity: 0.6;
}

.task .actions .edit {
    background-image: linear-gradient(to right, var(--pink), var(--purple));
    background-clip: text;
    -webkit-text-fill-color: transparent;
}

.task .actions .delete {
    color: crimson;
}
</style>
