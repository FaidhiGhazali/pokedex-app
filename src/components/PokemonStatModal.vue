<template>
  <div
    v-if="show"
    class="fixed inset-0 backdrop-blur-sm flex items-center justify-center"
    @click.self="close"
  >
    <div class="bg-white rounded-xl shadow-lg max-w-4xl w-full flex relative z-50">
      <div class="flex flex-col items-center justify-center p-8 w-2/4 bg-white relative">
        <div class="absolute top-4 right-4 text-3xl">
          <button 
            @click.stop="$emit('toggle-favorite', pokemon.name)" 
            class="absolute top-2 right-2"
            >
            <HeartIcon 
              class="w-6 h-6"
              :class="isFavorite ? 'text-red-500' : 'text-gray-500'" 
            />
          </button>
        </div>

        <div class="absolute top-4 left-4">
          <button
            @click="playCry"
            class="absolute top-1 left-2"
          >
          <SoundIcon class="w-7 h-10" />
        </button>
        </div>

        <img
          :src="pokemon.image"
          :alt="pokemon.name"
          class="w-52 h-auto"
          loading="lazy"
        />

        <div class="text-center pb-1 jutify-center mt-4">
          <h2 class="text-3xl font-bold capitalize text-gray-800">{{ pokemon.name }}</h2>
          <h3 class="text-xl text-gray-400 font-semibold">{{ formattedId }}</h3>
        </div>

        <div class="flex space-x-4 text-center":class="{ 'justify-center': type.length === 1, 'justify-start': type.length > 1 }">
          <p class="text-center rounded dense opacity-100 px-2 py-1 capitalize":class="`bg-type-${type[0]?.type?.name}`"> {{ type[0]?.type?.name }}</p>
          <p v-if="type[1]?.type?.name"class="text-center rounded dense opacity-100 px-2 py-1 capitalize" :class="`bg-type-${type[1]?.type?.name}`"> {{ type[1]?.type?.name }} </p>
        </div>

      </div>

      <div class="w-3/4 bg-gray-200 p-8">
        <h3 class="text-2xl font-bold mb-4 text-gray-900">Stats</h3>
        <div
          v-for="(stat, key, index) in stats"
          :key="key"
        >
          <div class="grid grid-flow-row-dense grid-cols-3 grid-rows-1 gap-1 font-medium">
            <div class="col-span-2 mb-2 font-bold" :class="['items-center px-4 py-2 rounded-md',index % 2 === 0 ? 'bg-white' : 'bg-gray-100' ]">{{ stat.label }}</div>
            <div class="text-right mb-2 font-bold" :class="['items-center px-4 py-2 rounded-md',index % 2 === 0 ? 'bg-white' : 'bg-gray-100' ]">{{ pokemon[key] }}</div>
          </div>
        </div>
      </div>
      <button
        @click="close"
        class="absolute -top-4 -right-4 z-100 bg-yellow-400 text-black rounded-full w-10 h-10 flex items-center justify-center text-2xl hover:bg-yellow-500"
      >
        <img src="../assets/xmark-solid.svg" alt="Close" class="w-6 h-6" />
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import HeartIcon from '../assets/heart-solid.svg'
import SoundIcon from '../assets/sound.svg'

const props = defineProps({
  show: Boolean,
  pokemon: Object,
  type: Array,
  cries: String,
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

const playCry = () => {
  const cryUrl = props.cries
  const audio = new Audio(cryUrl);
  audio.play().catch((err) => {
    console.error('Failed to play sound:', err);
  });
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
svg {
  fill: currentColor;
}
</style>