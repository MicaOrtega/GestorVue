<template>
  <div id="app">
    <!-- Barra de navegación con enlaces a diferentes rutas -->
    <nav>
      <ul class="nav justify-content-center">
        <li class="nav-item">
          <router-link to="/">Home</router-link> |
        </li>
        <li class="nav-item">
          <router-link to="/addtask">Agregar Tarea</router-link> |
        </li>
        <li class="nav-item">
          <router-link to="/tasklist">Lista de Tareas</router-link> |
        </li>
        <li class="nav-item">
          <router-link to="/combinedview">Vista Combinada</router-link>
        </li>
      </ul>
    </nav>
    <!-- Aquí se cargará el componente correspondiente a la ruta seleccionada -->
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>