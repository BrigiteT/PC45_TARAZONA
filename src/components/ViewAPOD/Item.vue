<template>
  <q-card class="media-card" flat bordered>
    <template v-if="media.type === 'image'">
      <q-img :src="media.url" class="media-content">
        <div class="text-overlay text-center">
          {{ media.title }}
        </div>
      </q-img>
    </template>
    <template v-else>
      <q-video :src="media.url" class="media-content" />
    </template>
    <q-card-section>
      <div class="title">{{ media.title }}</div>
      <div class="date">Date: {{ media.date }}</div>
    </q-card-section>
    <q-card-actions>
      <q-space />
      <q-btn
        color="blue"
        round
        flat
        dense
        :icon="isExpanded ? 'expand_less' : 'expand_more'"
        @click="$emit('toggleExpand', media.date)"
      />
    </q-card-actions>
    <q-slide-transition>
      <div v-show="isExpanded">
        <q-separator />
        <q-card-section class="description">
          {{ media.description }}
        </q-card-section>
      </div>
    </q-slide-transition>
  </q-card>
</template>

<script>
export default {
  name: "MediaItem",
  props: {
    media: Object,
    isExpanded: Boolean,
  },
};
</script>

<style scoped>
.media-card {
  width: 100%;
  max-width: 360px;
}

.media-content {
  width: 100%;
  height: 220px;
  object-fit: cover;
}

.text-overlay {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.6);
  color: #e92323;
  padding: 10px;
}

.title {
  font-size: 1.2rem;
  font-weight: bold;
}

.date {
  font-size: 1rem;
  color: #666;
}

.description {
  font-size: 0.9rem;
}
</style>
