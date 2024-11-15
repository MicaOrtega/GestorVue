<template>
    <div>
        <h1>Lista de Tareas</h1>
        <!-- Esta seccion es una combinación de las dos vistas anteriores -->
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
   // Esta sección debe permitir agregar tareas nuevas a la vez que extraer las tareas anteriores de la API
    export default {
    name: "TaskManager",
    data() {
        return {
            newTask: "",     // Campo de entrada para la nueva tarea
            tasks: [],       // Lista de tareas locales
        };
    },
    methods: {
        // Método para agregar nueva tarea
        addTask() {
            if (this.newTask.trim() === "") return;

            const newTask = {
                todo: this.newTask,
                completed: false,
                id: Date.now(),
            };

            // Añadir nueva tarea al principio de la lista
            this.tasks.unshift(newTask);
            this.saveTasksToLocalStorage();
            this.newTask = ""; // Limpiar campo de entrada
        },

        // Método para eliminar tarea
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
            this.saveTasksToLocalStorage();
        },

        // Cambiar estado de tarea
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
            this.saveTasksToLocalStorage();
        },

        // Guardar tareas en localStorage
        saveTasksToLocalStorage() {
            localStorage.setItem("tasks", JSON.stringify(this.tasks));
        },

        // Cargar tareas desde localStorage
        loadTasksFromLocalStorage() {
            const storedTasks = localStorage.getItem("tasks");
            if (storedTasks) {
                this.tasks = JSON.parse(storedTasks);
            }
        },

        // Cargar tareas desde API externa
        async fetchTasksFromAPI() {
            try {
                const response = await fetch("https://dummyjson.com/todos");
                const data = await response.json();
                // Extraer solo las primeras 5 tareas para no sobrecargar la lista
                this.tasks = [...data.todos.slice(0, 5), ...this.tasks];
                this.saveTasksToLocalStorage();
            } catch (error) {
                console.error("Error al cargar tareas desde la API:", error);
            }
        }
    },
    created() {
        this.loadTasksFromLocalStorage(); // Cargar tareas al iniciar el componente
    },
};
</script>

<style scoped>
/* Aquí pueden agregar estilos personalizados para el componente. */
.task-list{
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
    flex-direction: column; /* Cambia a columna para centrar el contenido verticalmente */
    align-items: center; /* Centra horizontalmente cada elemento */
    gap: 10px;
}

/* Estilo para marcar tareas completadas */
.completed {
    text-decoration: line-through;
    color: gray;
}

/* Estilos de botones dentro de cada tarea */
button {
    padding: 6px 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

/* Hover para botones en tareas */
button:hover {
    background-color: #28a745;
    color: white;
}

/* Botón de eliminación de tarea */
button:last-child {
    background-color: #dc3545;
    color: white;
}

button:last-child:hover {
    background-color: #c82333;
}

</style>