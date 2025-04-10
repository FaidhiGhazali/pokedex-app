<template>
    <div 
      style="background-color: #FFFFFF"
      class="relative rounded-lg p-4 text-center shadow-md hover:shadow-lg transition-shadow"
    >
      <img 
        :src="pokemon.image" 
        alt="Pokemon image" 
        class="w-50 h-50 mx-auto mb-2" 
      />
      <h2 class="text-lg font-bold capitalize">{{ pokemon.name }}</h2>
      <h3 class="text-sm text-gray-500">{{ pokemon.id }}</h3>
  
      <button 
       @click.stop="$emit('toggle-favorite', pokemon.name)" 
        class="absolute top-2 right-2">
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
  </template>
  
  <script setup>
  defineProps({
    pokemon: Object,
    isFavorite: Boolean
  });
  defineEmits(['toggle-favorite']);
  </script>
  