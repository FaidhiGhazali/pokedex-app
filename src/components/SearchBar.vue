<template>
    <div class="relative w-80">
      <input 
        v-model="inputValue"
        type="text" 
        placeholder="Name or ID number" 
        style="background-color: #FFFFFF"
        class="w-full px-4 py-2 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 pr-10"
      />
      <div v-if="!hasSearched" >
        <button 
          
          @click="$emit('search')" 
          class="absolute inset-y-0 right-10 flex items-center p-2">
          <img 
            src="../assets/magnifying-glass-solid.svg" 
            alt="search icon" 
            class="w-5 h-5 text-gray-500 hover:text-blue-500"
          />
        </button>
      </div>
      <div v-else >
        <button 
          @click="$emit('clear')" 
          class="absolute inset-y-0 right-0 flex items-center p-2">
          <img 
            src="../assets/xmark-solid.svg" 
            alt="clear icon" 
            class="w-5 h-5 text-gray-500 hover:text-red-500"
          />
        </button>
      </div>
      
    </div>
  </template>
  
  <script setup>
  import {computed} from 'vue'
  const props = defineProps({
    modelValue: String,
    hasSearched: Boolean
  });
  const emit = defineEmits(['update:modelValue', 'search', 'clear']);
  
  const inputValue = computed({
    get: () => props.modelValue,
    set: (value) => emit('update:modelValue', value)
  });
  </script>
  