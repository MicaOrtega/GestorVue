<template>
    <div>
        <h1>Lista de Tareas Completa</h1>
        <!-- Esta seccion es una combinación de las dos vistas anteriores -->
        <div v-if="allTasks.length > 0" class="task-list">
            <div v-for="task in allTasks" :key="task.id" class="task-item">
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
    name: "TaskCombined",
    data() {
        return {
            localTasks: [],
            apiTasks: [],       // Lista de tareas locales
        };
    },
    
    computed:{
        allTasks(){
            return[...this.localTasks, ...this.apiTasks];
        }
    },

    methods: {

        // Cambiar estado de tarea
        toggleTaskCompletion(task) {
            task.completed= !task.completed;

            if(this.localTasks.includes(task)){ 
                this.saveLocalTasks()
            }else if(this.apiTasks.includes(task)){
                this.saveApiTasks();
            }
        },

        saveTasksToLocalStorage() {
            localStorage.setItem("usertasks", JSON.stringify(this.localTasks));
        },


        deleteTask(task) {
            if (this.localTasks.includes(task)) {
                this.localTasks = this.localTasks.filter((t) => t.id !== task.id);
                this.saveLocalTasks();
            }else if (this.apiTasks.includes(task)) {
                this.apiTasks = this.apiTasks.filter((t) => t.id !== task.id);
            }
        },
        
        loadLocalTasks() {
            const storedTasks = localStorage.getItem("userTasks");
            if (storedTasks) {
                this.localTasks = JSON.parse(storedTasks);
            }
        },

        loadApiTasks(){
            const storedApiTasks = localStorage.getItem("apiTasks");
            if(storedApiTasks){
                this.apiTasks=JSON.parse(storedApiTasks);
            }
        },
        
        saveLocalTasks(){
            localStorage.setItem("userTasks", JSON.stringify(this.localTasks));
        },

        saveApiTasks(){
            localStorage.setItem("apiTasks", JSON.stringify(this.apiTasks))
        },

    },
    
    created() {
        this.loadLocalTasks();
        this.loadApiTasks();
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