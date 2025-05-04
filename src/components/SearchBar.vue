<template>
    <div class="relative w-80">
      <input 
        v-model="inputValue"
        type="text" 
        placeholder="Name or ID number" 
        @focus="$event.target.select()"
        @keyup.enter="$emit('search')"
        style="background-color: #FFFFFF"
        class="w-full px-4 py-2 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 pr-10"
      />
      <!-- <button 
        @click="handleAction" 
        class="absolute inset-y-0 right-10 flex items-center p-2">
        <img 
          :src="hasSearched ? '../src/assets/xmark-solid.svg' : '../src/assets/magnifying-glass-solid.svg'" 
          alt="icon"
          class="w-5 h-5 text-gray-500 hover:text-blue-500"
        />
      </button> -->
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

  const handleAction = () => {
  if (props.hasSearched) {
    emit('update:modelValue', '');
    emit('clear');
  } else {
    emit('search');
  }
};
  </script>
  