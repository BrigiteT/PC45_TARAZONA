<template>
  <div class="home-page">
    <!-- Fondo con imagen de fondo -->
    <div class="background-image"></div>

    <!-- Contenedor par a el contenido centrado -->
    <div class="content-container">
      <!-- Texto en el medio -->
      <div class="top-text">
        <h2>Welcome to my page</h2>
      </div>
      <!-- Botón para redirigir a la segunda página -->
      <div class="bottom-button">
        <q-btn-toggle
          class="q-pa-md"
          v-model="model"
          toggle-color="black"
          :options="[{ label: 'FIND TO APOD', value: 'one' }]"
          @click="handleToggle"
        />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "HomePage",
  data() {
    return {
      model: "one", // Valor inicial del toggle
    };
  },
  methods: {
    handleToggle() {
      // Lógica para manejar el cambio de estado del toggle
      if (this.model === "one") {
        this.$router.push("/apod_nasa");
      }
    },
  },
};
</script>
<style scoped>
/* Estilos para la página principal */
.home-page {
  height: 100vh; /* Establece la altura al 100% del viewport */
  display: flex;
  flex-direction: column; /* Alinear elementos verticalmente */
  justify-content: flex-start; /* Alinear elementos al inicio */
  align-items: center; /* Centrar horizontalmente */
  position: relative; /* Posición relativa para que los elementos absolutos se ajusten */
}

.background-image {
  /* Ajusta aquí la imagen de fondo */
  background-image: url("/src/components/Picture/nasa.webp");
  background-size: cover;
  background-position: center;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  opacity: 1; /* Opacidad opcional para que la imagen sea visible */
  z-index: -1; /* Coloca la imagen detrás del contenido */
  width: 100%;
  height: 100%;
}

.content-container {
  z-index: 1; /* Coloca el contenido sobre la imagen de fondo */
  padding: 20px;
  display: flex;
  flex-direction: column; /* Alinear elementos verticalmente */
  align-items: center; /* Centrar horizontalmente */
}

.top-text {
  text-align: center; /* Alineación centrada si es necesario */
  margin-top: 0cap; /* Espacio superior entre la parte superior */
  color: rgb(245, 245, 245);
}

.bottom-button {
  margin-top: 200px; /* Espacio entre el texto y el botón */
  text-align: center; /* Alineación centrada si es necesario */
}

/* Estilos para q-btn-toggle */
.q-btn-toggle {
  width: 200px; /* Ancho del toggle */
  padding: 30px; /* Espaciado interno */
}
</style>
