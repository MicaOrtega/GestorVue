<template>
    <div>
        <h1>Lista de Tareas</h1>
        <button @click="fetchTasks" class="button">Cargar Tareas</button>
        <div v-if="tasks.length > 0" class="task-list">
            <div v-for="task in tasks" :key="task.id" class="task-item">
                <div>
                    <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
                    <span>{{ task.completed ? 'Completada' : 'Pendiente' }}</span>
                    <button @click="toggleTaskCompletion(task)">
                        {{ task.completed ? 'Desmarcar' : 'Completar' }}
                    </button>
                    <button @click="deleteTask(task)">Eliminar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TaskList",
    data() {
        return {
            tasks: [], // Almacenamiento local de las tareas traídas de la API
        };
    },
    methods: {
        // Llamada para obtener las tareas desde la API externa
        fetchTasks() {
            // Aquí deberían realizar la solicitud a la API usando axios o fetch.
            // La URL que usaremos es: https://dummyjson.com/todos
            fetch("https://dummyjson.com/todos")
                .then(response => response.json())
                .then(data => {
                    this.tasks = data.todos;
                    this.saveTasksToLocalStorage();
                })
                .catch(error => console.error("Error al cargar tareas:", error));
            // Sugerencia: Intentar implementarlo con axios o fetch
        },

        // Cambiar el estado de una tarea (completada/no completada)
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
            this.saveTasksToLocalStorage();
        },

        // Eliminar la tarea seleccionada
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
            this.saveTasksToLocalStorage()
        },

        saveTasksToLocalStorage(){
            localStorage.setItem("tasks", JSON.stringify(this.tasks));
        },

        loadTasksFromLocalStorage(){
            const storedTasks= localStorage.getItem("tasks");
            if(storedTasks){
                this.tasks= JSON.parse(storedTasks);
            }
        },
    },

    created(){
        
    },
};
</script>

<style scoped>
/* Aquí pueden experimentar con estilos de tu preferencia */

.task-list {
    padding: 20px;
    max-width: 400px;
    margin: 0 auto;
    text-align: center;
    border-radius: 8px;
    background-color: #f8f9fa;
}

.task-item {
    padding: 10px;
    border-bottom: 1px solid #ccc;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

button {
    padding: 6px 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: #28a745;
    color: white;
}

button:last-child {
    background-color: #dc3545;
    color: white;
}

button:last-child:hover {
    background-color: #c82333;
}

.completed {
    text-decoration: line-through;
    color: gray;
}
</style>