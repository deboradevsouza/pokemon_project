<template>
    <div
        :class="cardClasses"
        class="rounded-lg flex gap-4 flex-row p-4 w-full max-w-[294px]"
        @click="toggleInfo">

        <slot></slot>
        <div class="poke-detalhes" v-show="aberto">
            <hr class="my-3 border-white/30" />
            <slot name="expanded"></slot>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  type: {
    type: String,
    default: 'normal'
  }
})

const aberto = ref(false)

function toggleInfo() {
  aberto.value = !aberto.value
}

// Map Pokemon types to Tailwind classes
const typeColors = {
  normal: 'bg-gray-200',
  fire: 'bg-red-500 text-white',
  water: 'bg-blue-500 text-white',
  grass: 'bg-green-500 text-white',
  electric: 'bg-yellow-400',
  ice: 'bg-cyan-200',
  fighting: 'bg-red-700 text-white',
  poison: 'bg-purple-500 text-white',
  ground: 'bg-yellow-700 text-white',
  flying: 'bg-indigo-300',
  psychic: 'bg-pink-500 text-white',
  bug: 'bg-lime-500',
  rock: 'bg-yellow-800 text-white',
  ghost: 'bg-purple-800 text-white',
  dark: 'bg-gray-800 text-white',
  dragon: 'bg-indigo-700 text-white',
  steel: 'bg-gray-400',
  fairy: 'bg-pink-300'
}

// Dynamic class based on type
const cardClasses = computed(() => {
  return typeColors[props.type] || typeColors.normal
})
</script>