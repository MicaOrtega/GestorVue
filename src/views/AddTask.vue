<template>
    <div class="add-task-container">
        <h1>Añadir Tarea</h1>
        <div class="input-group">
            <input 
                v-model="newTask" 
                @keyup.enter="addTask" 
                placeholder="Añadir nueva tarea" 
                class="task-input"
            />
            <button @click="addTask" class="add-button">Añadir</button>
        </div>

        <div v-if="false" > 0" class="task-list">
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
    name: "AddTask",
    data() {
        return {
            newTask: "", // Campo de entrada para la nueva tarea
            tasks: [],   // Lista de tareas locales
        };
    },
    methods: {
        addTask() {
            if (this.newTask.trim() === "") return;

            const newTask = {
                todo: this.newTask,
                completed: false,
                id: Date.now(),
            };

            // Añadir la nueva tarea al inicio de la lista
            this.tasks.unshift(newTask);
            this.saveTasksToLocalStorage();
            this.newTask = ""; // Limpiar el campo de entrada después de agregar
        },

        // Elimina una tarea específica de la lista
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
            this.saveTasksToLocalStorage();
        },

        // Cambia el estado de la tarea entre completada y no completada
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
            this.saveTasksToLocalStorage();
        },

        saveTasksToLocalStorage(){
            localStorage.setItem("tasks", JSON.stringify(this.tasks));
        },

        loadTasksFromLocalStorage() {
            const storedTasks = localStorage.getItem("tasks");
            if (storedTasks) {
                this.tasks = JSON.parse(storedTasks);
            }
        },

    },

    created(){
        this.loadTasksFromLocalStorage();
    }
};
</script>

<style scoped>
.add-task-container {
    padding: 20px;
    max-width: 400px;
    margin: 0 auto;
    text-align: center;
    border-radius: 8px;
    background-color: #f8f9fa;
}

.input-group {
    display: flex;
    align-items: center;
    margin-bottom: 15px;
}

.task-input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.add-button {
    padding: 10px 15px;
    border: none;
    margin-left: 5px;
    border-radius: 4px;
    background-color: #28a745 !important; /* Verde */
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.add-button:hover {
    background-color: #218838 !important; /* Un tono más oscuro de verde para el hover */
}

.task-list {
    margin-top: 20px;
}

.task-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    background-color: #ffffff;
    border: 1px solid #ddd;
    border-radius: 6px;
    margin-bottom: 10px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.completed {
    text-decoration: line-through;
    color: gray;
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

</style>