<template>
  <q-card class="apod-card">
    <q-card-section>
      <q-form @submit.prevent="fetchData">
        <div class="q-gutter-md">
          <q-input v-model="startDate" label="Fecha Inicial" type="date" />
          <q-input v-model="endDate" label="Fecha Final" type="date" />
          <q-btn label="Consultar" type="submit" color="RED" />
        </div>
      </q-form>
    </q-card-section>
  </q-card>
</template>

<style>
.apod-card {
  width: 300px;
}
.apod-card q-card-section {
  height: 100px;
  overflow: hidden;
}
</style>

<script>
export default {
  name: "FilterAPOD",
  emits: ["fetchData"],
  data() {
    return {
      startDate: "",
      endDate: "",
    };
  },
  methods: {
    fetchData() {
      this.$emit("fetchData", {
        startDate: this.startDate,
        endDate: this.endDate,
      });
    },
  },
};
</script>
