<template>
    <div>
        <h1>Lista de Tareas</h1>
        <button v-if="!tasksLoaded" @click="fetchTasks" class="button">Cargar Tareas</button>
        <div v-if="tasks.length > 0" class="task-list">
            <div v-for="task in tasks" :key="task.id" class="task-item">
                <span :class="{ completed: task.completed }">{{ task.todo }}</span>
                <div>
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
            tasksLoaded: false,
        };
    },
    methods: {
        // Llamada para obtener las tareas desde la API externa
        fetchTasks() {
            // Aquí deberían realizar la solicitud a la API usando axios o fetch.
            // La URL que usaremos es: https://dummyjson.com/todos
            fetch("https://dummyjson.com/todos")
                .then((response) => response.json())
                .then((data) => {
                    this.tasks = data.todos.map((task) => ({
                        id: task.id,
                        todo: task.todo,
                        completed: task.completed,
                    }));
                    this.tasksLoaded = true;
                    this.saveApiTasksToLocalStorage();
                })
                .catch((error) =>
                    console.error("Error al cargar tareas desde la API:", error)
                );
            // Sugerencia: Intentar implementarlo con axios o fetch
        },

        // Cambiar el estado de una tarea (completada/no completada)
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
            this.saveApiTasksToLocalStorage();
        },

        loadApiTasksToLocalStorage(){
            const storedTasks = localStorage.getItem("apiTasks");
            if(storedTasks){
                this.tasks = JSON.parse(storedTasks);
                this.tasksLoaded=true;
            }
        },

        // Eliminar la tarea seleccionada
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
            this.saveApiTasksToLocalStorage();
        },

        saveApiTasksToLocalStorage(){
            localStorage.setItem("apiTasks", JSON.stringify(this.tasks));
        },

    },

    created(){
        this.loadApiTasksToLocalStorage();
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