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
                  label="Fecha de Inicio"
                  type="date"
                />
                <q-input v-model="endDate" label="Fecha Final" type="date" />
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
            class="my-card"
            flat
            bordered
          >
            <template v-if="item.media_type === 'image'">
              <q-img :src="item.url" class="media-item">
                <div class="absolute-bottom text-subtitle2 text-center">
                  {{ item.title }}
                </div>
              </q-img>
            </template>
            <template v-else>
              <q-video :src="item.url" class="media-item" />
            </template>
            <q-card-section>
              <div class="text-h6">{{ item.title }}</div>
              <div class="text-subtitle2">Fecha: {{ item.date }}</div>
            </q-card-section>
            <q-card-actions>
              <q-space />
              <q-btn
                color="grey"
                round
                flat
                dense
                :icon="
                  expanded[item.date]
                    ? 'keyboard_arrow_up'
                    : 'keyboard_arrow_down'
                "
                @click="toggleExpand(item.date)"
              />
            </q-card-actions>
            <q-slide-transition>
              <div v-show="expanded[item.date]">
                <q-separator />
                <q-card-section class="text-subtitle2">
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
  name: "ItemPage",
  setup() {
    const startDate = ref("");
    const endDate = ref("");
    const searchTerm = ref("");
    const data = ref([]);
    const expanded = ref({});

    const fetchData = async () => {
      console.log("Fetching data...");
      console.log(`Start Date: ${startDate.value}`);
      console.log(`End Date: ${endDate.value}`);

      try {
        const response = await api.get("/apod", {
          params: {
            api_key: "Gwz5SE2UAGmgoXqmsgVOeaVSTRv8BiHAGjEhazmn",
            start_date: startDate.value,
            end_date: endDate.value,
          },
        });
        console.log("Response received:", response.data);
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

<style scoped>
.bg-page {
  background: url("/src/components/Picture/Fondo2.jpg");
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
  top: 100px;
  left: 20px;
  z-index: 1;
}

.list {
  margin-left: 260px;
  width: calc(100% - 280px);
  overflow-y: auto;
  height: 100vh;
  padding-top: 20px;
}

.my-card {
  width: 100%;
  max-width: 300px;
  margin-bottom: 20px;
}

.media-item {
  max-height: 200px;
  object-fit: cover;
  width: 100%;
}
</style>
