<template>
  <div>
    <LoadingIndicator v-if="isLoading" text="Loading comics..." />
    <div v-if="data && !isLoading">
      <div class="grid grid-flow-row grid-cols-1 gap-4 md:grid-cols-2 lg:grid-">
        <div :key="comic.id" v-for="comic in data">
          {{  comic.title }}
        </div>
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">
import {ref, onMounted} from 'vue'
import type {Ref} from 'vue'

import { useComics } from '@/composables/marvelApi'
import type { Comic } from '@/types/marvel';
import LoadingIndicator from './LoadingIndicator.vue'

const isLoading: Ref<boolean> = ref(false);
const data: Ref<Comic[] | undefined> = ref()

const getComics = async () => {
  isLoading.value = true;
  const comics = await useComics();

  data.value = comics.results;
  isLoading.value = false;
}

onMounted(async () => {
  await getComics()
})
</script>

<style scoped>

</style>