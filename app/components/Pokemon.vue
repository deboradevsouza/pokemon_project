<template>
  <div>
    <div v-if="data && data.results">
      <div v-for="currentPokemon in data.results" :key="currentPokemon.name">
        <p>{{ currentPokemon.name }}</p>
        <img
          :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getPokemonId(currentPokemon.url)}.png`"
          :alt="currentPokemon.name"
        >
      </div>
    </div>
    <div v-else>Carregando...</div>
  </div>
</template>

<script setup>
const { data, error, pending } = await useFetch('https://pokeapi.co/api/v2/pokemon?limit=100')

function getPokemonId(url) {
  //Extrai o id do pokemon para usar na url da imagem
  return url.split('/').filter(Boolean).pop()
}

if (error.value) {
  console.error('Erro ao buscar Pokémon:', error.value)
}
</script>
