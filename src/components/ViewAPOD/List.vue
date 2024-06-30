<template>
  <q-page padding class="bg-page">
    <div class="container">
      <div class="filter">
        <q-card flat bordered>
          <q-card-section>
            <q-form @submit.prevent="fetchData">
              <div class="q-gutter-md">
                <q-input
                  v-model="startDate"
                  label="Fecha de Inicio (YYYY-MM-DD)"
                  type="date"
                />
                <q-input
                  v-model="endDate"
                  label="Fecha de Fin (YYYY-MM-DD)"
                  type="date"
                />
                <q-input
                  v-model="searchTerm"
                  label="Buscar en Descripción"
                  type="text"
                />
                <q-btn label="Consultar" type="submit" color="primary" />
              </div>
            </q-form>
          </q-card-section>
        </q-card>
      </div>
      <div class="list">
        <div class="q-pa-md row items-start q-gutter-md">
          <q-card
            v-for="item in filteredData"
            :key="item.date"
            class="custom-card"
            flat
            bordered
          >
            <template v-if="item.media_type === 'image'">
              <q-img :src="item.url" class="custom-media">
                <div class="text-overlay text-center">
                  {{ item.title }}
                </div>
              </q-img>
            </template>
            <template v-else>
              <q-video :src="item.url" class="custom-media" />
            </template>
            <q-card-section>
              <div class="custom-title">{{ item.title }}</div>
              <div class="custom-date">Fecha: {{ item.date }}</div>
            </q-card-section>
            <q-card-actions>
              <q-space />
              <q-btn
                color="grey"
                round
                flat
                dense
                :icon="expanded[item.date] ? 'expand_less' : 'expand_more'"
                @click="toggleExpand(item.date)"
              />
            </q-card-actions>
            <q-slide-transition>
              <div v-show="expanded[item.date]">
                <q-separator />
                <q-card-section class="custom-description">
                  {{ item.explanation }}
                </q-card-section>
              </div>
            </q-slide-transition>
          </q-card>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { ref, computed } from "vue";
import { api } from "boot/axios";

export default {
  name: "ListPage",
  setup() {
    const startDate = ref("");
    const endDate = ref("");
    const searchTerm = ref("");
    const data = ref([]);
    const expanded = ref({});

    const fetchData = async () => {
      try {
        const response = await api.get("/data", {
          params: {
            start_date: startDate.value,
            end_date: endDate.value,
          },
        });
        data.value = response.data;
        expanded.value = {};
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    const toggleExpand = (date) => {
      expanded.value[date] = !expanded.value[date];
    };

    const filteredData = computed(() => {
      if (!searchTerm.value) {
        return data.value;
      }
      return data.value.filter((item) =>
        item.explanation.toLowerCase().includes(searchTerm.value.toLowerCase())
      );
    });

    return {
      startDate,
      endDate,
      searchTerm,
      data,
      fetchData,
      filteredData,
      expanded,
      toggleExpand,
    };
  },
};
</script>

<style lang="scss" scoped>
.bg-page {
  background: url("https://i.pinimg.com/originals/9f/ea/c1/9feac1a4b56f22bdb2ca6926ff8c80db.jpg")
    no-repeat center center fixed;
  background-size: cover;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

.container {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  height: 100%;
  width: 100%;
}

.filter {
  position: fixed;
  top: 60px;
  left: 20px;
  z-index: 1000;
}

.list {
  margin-left: 260px;
  width: calc(100% - 280px);
  overflow-y: auto;
  height: 100vh;
  padding-top: 20px;
}

.custom-card {
  width: 100%;
  max-width: 360px;
  margin-bottom: 20px;
}

.custom-media {
  width: 100%;
  height: 220px;
  object-fit: cover;
}

.text-overlay {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.6);
  color: #b31515;
  padding: 10px;
}

.custom-title {
  font-size: 1.2rem;
  font-weight: bold;
}

.custom-date {
  font-size: 1rem;
  color: #666;
}

.custom-description {
  font-size: 0.9rem;
}
</style>
