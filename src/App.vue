<template>
  <div class="min-h-screen p-6" style="background-color: #FDE68A;">
    <div class="mb-6">
      <p class="text-6xl font-bold capitalize">Pokédex</p>
    </div>
    
    <div class="mb-6">
      <p class="text-xl font-bold">Search for a Pokémon by name or id number</p>
    </div>
    
    <div class="mb-6 flex items-center">
      <SearchBar
        v-model="searchQuery"
        :hasSearched="hasSearched"
        @search="searchPokemon"
        @clear="clearSearch"
      />
    </div>

    <PokemonGrid 
      :list="filteredPokemonList" 
      :favorites="favorites"
      :hasSearched="hasSearched"
      :loading="isLoading"
      @toggle-favorite="toggleFavorite"
      @load-more="loadMore"
      @card-click="openModal"
    />

    <PokemonModal 
      :show="isModalOpen" 
      :pokemon="selectedPokemon"
      :type="typePokemon"
      :cries="criesPokemon"
      @close="closeModal"
      :isFavorite="selectedPokemon ? favorites.includes(selectedPokemon.name) : false"
      @toggle-favorite="toggleFavorite"
    />
  </div>

  <div 
    v-if="showErrorModal"
    class="fixed inset-0 backdrop-blur-sm flex items-center justify-center"
  >
    <div class="bg-white p-6 rounded shadow-md max-w-sm w-full text-center relative">
      <p class="text-xl font-semibold text-red-600 mb-4">{{ errorMessage }}</p>
      <button
        @click="showErrorModal = false"
        class="absolute -top-4 -right-4 z-10 bg-yellow-400 text-black rounded-full w-10 h-10 flex items-center justify-center text-2xl hover:bg-yellow-500"
      >
        <img src="../src/assets/xmark-solid.svg" alt="Close" class="w-6 h-6" />
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';
import SearchBar from './components/SearchBar.vue';
import PokemonGrid from './components/PokemonGrid.vue';
import PokemonModal from './components/PokemonStatModal.vue';

const pokemonList = ref([]);
const favorites = ref(JSON.parse(localStorage.getItem("favorites")) || []);
const limit = ref(10); 
const offset = ref(0);
const searchQuery = ref("");
const searchResult = ref(null); 
const hasSearched = ref(false);
const showErrorModal = ref(false)
const errorMessage = ref("");
const selectedPokemon = ref(null);
const isModalOpen = ref(false);
const typePokemon = ref([])
const criesPokemon = ref("")
const isLoading = ref(false);

const fetchPokemon = async () => {
  if (isLoading.value) return;
  isLoading.value = true;

  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${limit.value}&offset=${offset.value}`);
    const pokemonResults = response.data.results;

    const detailedPokemonList = await Promise.all(
      pokemonResults.map(async (pokemon) => {
        const details = await axios.get(pokemon.url);
        return {
          id: details.data.id.toString().padStart(4, "0"),
          name: pokemon.name,
          image: details.data.sprites.other["official-artwork"].front_default,
          url: pokemon.url
        };
      })
    );

    pokemonList.value = [...pokemonList.value, ...detailedPokemonList];
    offset.value += limit.value;
  } catch (error) {
    console.error("Error loading Pokémon:", error);
  } finally {
    isLoading.value = false;
  }
};


const searchPokemon = async () => {
  const query = searchQuery.value.trim().toLowerCase().replace(/^0+/, "");
  if (query) {
    try {
      const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${query}`);
      const data = response.data;
      searchResult.value = {
        id: data.id.toString().padStart(4, "0"),
        name: data.name,
        image: data.sprites.other["official-artwork"].front_default,
        url: `https://pokeapi.co/api/v2/pokemon/${data.id}`
      };
      hasSearched.value = true;
    } catch (error) {
      searchResult.value = null;
      hasSearched.value = false;
      showErrorModal.value = true;
      errorMessage.value = error.response.data;
      searchQuery.value = ""
    }
  }
};

const clearSearch = () => {
  searchQuery.value = "";
  searchResult.value = null;
  pokemonList.value = [];
  offset.value = 0;
  fetchPokemon();
  hasSearched.value = false;
}

const filteredPokemonList = computed(() => {
  return searchResult.value ? [searchResult.value] : pokemonList.value;
});

const loadMore = () => {
  fetchPokemon();
}

const toggleFavorite = (name) => {
  if (favorites.value.includes(name)) {
    favorites.value = favorites.value.filter(item => item !== name);
  } else {
    favorites.value.push(name);
  }
  localStorage.setItem("favorites", JSON.stringify(favorites.value));
};

const openModal = async (pokemon) => {
  try {
    const response = await axios.get(pokemon.url);
    selectedPokemon.value = {
      ...pokemon,
      height: response.data.height,
      weight: response.data.weight,
      hp: response.data.stats[0].base_stat,
      attack: response.data.stats[1].base_stat,
      defense: response.data.stats[2].base_stat,
      specialAttack: response.data.stats[3].base_stat,
      specialDefense: response.data.stats[4].base_stat,
      speed: response.data.stats[5].base_stat,
    };
    isModalOpen.value = true;
    typePokemon.value = response.data.types
    criesPokemon.value = response.data.cries.latest
  } catch (error) {
    console.error('Error fetching Pokémon stats:', error);
  }
};

const closeModal = () => {
  isModalOpen.value = false;
};

onMounted(() => {
  fetchPokemon();
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');

body {
  font-family: 'Lato', sans-serif;
}
</style>
