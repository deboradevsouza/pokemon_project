<template>
  <header 
  class="flex flex-row justify-between items-center bg-[#FFCC03] p-4 gap-4"
  >
    <div class="pag-inicial">
      <NuxtLink
      to="/"
      >Início</NuxtLink>
    </div>
    <div class="buscar-pokemon flex flex-row items-center gap-2">
      <input 
      type="text"
      v-model="searchQuery"
      placeholder="Buscar Pokémon"
      class="rounded-full p-2 flex items-center border-2 border-solid border-black"
      @keyup.enter="searchPokemon"
      />
      <button 
      @click="searchPokemon">
        <i
        class="bi bi-search">
        </i>
      </button>
    </div>
  </header>

  <div class="background">
    <div 
    class="flex flex-col gap-2"
    v-if="filteredPokemons && filteredPokemons.length">
      <Card 
        v-for="poke in filteredPokemons" 
        :key="poke.id"
        :type="poke.primaryType"
      >
        <!-- Main content (always visible) -->
        <div class="ficha-pokemon">
          
          <div class="nome-pokemon-info">
            <h3 class="nome-poke capitalize text-center">
              # {{ poke.id.toString().padStart(3, '0') }}</h3>
            <h3 class="nome-poke capitalize text-center">{{ poke.name }}</h3>
          </div>
          <div>
            <img
              :src="poke.image"
              :alt="poke.name"
              class="max-h-32 mx-auto"
            />
          </div>
        </div>

        <!-- Expanded content -->
        <template #expanded>
          <div class="space-y-2">
            <p><span class="font-medium">Peso:</span> {{ poke.weight }} kg</p>
          </div>
        </template>
      </Card>
    </div>

    <div v-else-if="pokemons && pokemons.length === 0">
      Nenhum Pokémon encontrado para "{{ searchQuery }}"
    </div>

    <div v-else>Carregando...</div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Search query ref
const searchQuery = ref('')

// Usa useAsyncData com cache automático do Nuxt
const { data: pokemons, error, pending } = await useAsyncData('pokemons', async () => {
  //Busca a lista principal
  const response = await $fetch('https://pokeapi.co/api/v2/pokemon?limit=151')

  //Busca os detalhes (peso, imagem etc.)
  const results = await Promise.all(
    response.results.map(async (poke) => {
      const detail = await $fetch(poke.url)
      const id = poke.url.split('/').filter(Boolean).pop()

      return {
        id: parseInt(id),
        name: poke.name,
        weight: (detail.weight / 10).toFixed(1),
        image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`,
        primaryType: detail.types[0]?.type?.name || 'normal',
        baseExperience: detail.base_experience,
        height: (detail.height / 10).toFixed(1)
      }
    })
  )

  return results
})

// Computed property for filtered pokemons
const filteredPokemons = computed(() => {
  if (!pokemons.value) return []
  
  const query = searchQuery.value.toLowerCase().trim()
  
  if (!query) return pokemons.value
  
  return pokemons.value.filter(pokemon => {
    // Search by name
    if (pokemon.name.toLowerCase().includes(query)) return true
    
    // Search by ID (converted to string)
    if (pokemon.id.toString().includes(query)) return true
    
    // Search by type
    if (pokemon.primaryType.toLowerCase().includes(query)) return true
    
    return false
  })
})

// Search function for button/enter key
const searchPokemon = () => {
  // The computed property already handles the filtering
  // This function just ensures reactivity
  console.log('Searching for:', searchQuery.value)
}

//Trata erros se houver
if (error.value) {
  console.error('Erro ao buscar Pokémon:', error.value)
}
</script>

<style>
/* Your existing styles remain the same */
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

header {
  border-bottom: 3px solid black;
}

.pag-inicial {
  font-family: "Tilt Warp", sans-serif;
  font-weight: 400;
}

.buscar-pokemon {
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  font-size: 14px;
}

.nome-poke {
  font-family: "Montserrat", sans-serif;
  font-weight: 600;
}

img {
  max-height: 100px;
  max-width: 100px;
}

.background {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  background-color: #77BEF0;
  min-height: 100vh;
  padding: 1rem;
  gap: 0.5rem;
}

.ficha-pokemon {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.5rem;
}

.nome-pokemon-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
  gap: 0.5rem;
  width: 100%;
}
</style>