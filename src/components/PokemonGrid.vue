<template>
    <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-6 max-w-fit">
      <PokemonCard 
        v-for="pokemon in list"
        :key="pokemon.name"
        :pokemon="pokemon"
        :isFavorite="favorites.includes(pokemon.name)"
        class="cursor-pointer"
        @toggle-favorite="emit('toggle-favorite', $event)"
        @click="emit('card-click', pokemon)"
      />
  
      <div v-if="!hasSearched" class="rounded-lg p-4 text-center flex items-center justify-center">
        <button 
          @click="emit('load-more')" 
          :disabled="loading"
          class="px-4 py-2 bg-yellow-400 font-bold rounded hover:bg-yellow-500 disabled:opacity-50 disabled:cursor-not-allowed"
        >
          {{ loading ? 'Loading...' : 'Load More Pokemon' }}
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import PokemonCard from './PokemonCard.vue'
  
  defineProps({
    list: Array,
    favorites: Array,
    hasSearched: Boolean,
    loading: Boolean
  });

  const emit = defineEmits(['toggle-favorite', 'load-more', 'card-click']);
  </script>
  