<template>
  <div
    v-if="show"
    class="fixed inset-0 backdrop-blur-sm flex items-center justify-center"
    @click.self="close"
  >
    <div class="bg-white rounded-xl shadow-lg max-w-4xl w-full flex relative overflow-hidden">
      <div class="flex flex-col items-center justify-center p-8 w-2/4 bg-white relative">
        <div class="absolute top-4 right-4 text-red-500 text-3xl">
          <button 
            @click.stop="$emit('toggle-favorite', pokemon.name)" 
            class="absolute top-2 right-2"
            >
            <svg 
              xmlns="http://www.w3.org/2000/svg" 
              viewBox="0 0 24 24" 
              fill="currentColor" 
              class="w-6 h-6"
              :style="{ color: isFavorite ? '#EF4444' : '#6B7280' }">
              <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
            </svg>
          </button>
        </div>

        <img
          :src="pokemon.image"
          :alt="pokemon.name"
          class="w-52 h-auto"
        />

        <div class="text-center mt-4">
          <h2 class="text-3xl font-bold capitalize text-gray-800">{{ pokemon.name }}</h2>
          <h3 class="text-xl text-gray-400 font-semibold">{{ formattedId }}</h3>
        </div>
      </div>

      <div class="w-3/4 bg-gray-100 p-8">
        <h3 class="text-2xl font-bold mb-4 text-gray-900">Stats</h3>
        <div
          v-for="(stat, key, index) in stats"
          :key="key"
        >
          <div class="grid grid-flow-row-dense grid-cols-3 grid-rows-1 gap-1 font-medium">
            <div class="col-span-2 mb-2" :class="['items-center px-4 py-2 rounded-md',index % 2 === 0 ? 'bg-white' : 'bg-gray-200' ]">{{ stat.label }}</div>
            <div class="text-right mb-2" :class="['items-center px-4 py-2 rounded-md',index % 2 === 0 ? 'bg-white' : 'bg-gray-200' ]">{{ pokemon[key] }}</div>
          </div>
        </div>
      </div>
        <button
          @click="close"
          class="absolute top-4 right-4 bg-yellow-400 text-black rounded-full w-10 h-10 flex items-center justify-center text-2xl hover:bg-yellow-500"
        >
          <img src="../assets/xmark-solid.svg" alt="Close" class="w-6 h-6" />
        </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  show: Boolean,
  pokemon: Object,
  isFavorite: Boolean,
});

const emit = defineEmits(['close', 'toggle-favorite']);

const stats = {
  height: { label: 'Height' },
  weight: { label: 'Weight' },
  hp: { label: 'HP' },
  attack: { label: 'Attack' },
  defense: { label: 'Defense' },
  specialAttack: { label: 'Special Attack' },
  specialDefense: { label: 'Special Defense' },
  speed: { label: 'Speed' }
};

const formattedId = computed(() => {
  return String(props.pokemon.id).padStart(4, '0');
});

const close = () => emit('close');
</script>

<style scoped>
.capitalize {
  text-transform: capitalize;
}
</style>