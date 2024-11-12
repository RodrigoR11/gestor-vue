<template>
    <div>
        <h1 class="title">Lista de Tareas</h1>
        <button @click="fetchTasks" class="add-button">Cargar Tareas</button>
        <div v-if="tasks.length > 0" class="task-list">
            <div v-for="task in tasks" :key="task.id" class="task-item">
                <div class="task-content">
                    <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
                    <span class="task-status" :class="{ completed: task.completed }">
                        {{ task.completed ? 'Completada' : 'Pendiente' }}
                    </span>
                </div>
                <!-- Botones pequeños centrados debajo de cada tarea -->
                <div class="task-buttons">
                    <button @click="toggleTaskCompletion(task)" class="small-button complete-button">
                        ✔️
                    </button>
                    <button @click="deleteTask(task)" class="small-button delete-button">
                        🗑️
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "TaskList",
    data() {
        return {
            tasks: [],
        };
    },
    methods: {
        fetchTasks() {
            axios.get('https://dummyjson.com/todos')
                .then(response => {
                    this.tasks = response.data.todos;
                })
                .catch(error => {
                    console.error("Error al obtener las tareas:", error);
                });
        },
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
    },
};
</script>

<style scoped>
.title {
    font-size: 2rem;
    font-weight: bold;
    color: #154189;
    margin-bottom: 20px;
}

.add-button {
    padding: 8px 16px;
    background-color: #20c997;
    color: white;
    border: none;
    border-radius: 20px;
    font-size: 0.9rem;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
    box-shadow: 0 0 5px rgba(32, 201, 151, 0.5);
    display: block;
    margin: 0 auto 20px auto;
}

.add-button:hover {
    background-color: #17a2b8;
}

.task-list {
    margin-top: 20px;
}

.task-item {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 10px;
    margin-bottom: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    text-align: center;
}

.task-content {
    margin-bottom: 10px;
}

h5 {
    font-size: 1.1em;
    margin: 5px 0;
}

.task-status {
    font-size: 0.8rem;
    font-weight: bold;
    color: #fff;
    background-color: #f0ad4e;
    padding: 3px 8px;
    border-radius: 5px;
}

.task-status.completed {
    background-color: #5cb85c;
}

.task-buttons {
    display: flex;
    justify-content: center;
    gap: 5px;
    margin-top: 5px;
}

.small-button {
    padding: 4px 8px;
    font-size: 0.8rem;
    border-radius: 5px;
    cursor: pointer;
}

.complete-button {
    color: #20c997;
    border: 1px solid #20c997;
}

.complete-button:hover {
    background-color: #17a2b8;
}

.delete-button {
    color: #d9534f;
    border: 1px solid #d9534f;
}

.delete-button:hover {
    background-color: #c82333;
}
</style>
