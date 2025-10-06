<template>
  <div>

    <div class="background" v-if="data && data.results">
      
      <Card v-for="currentPokemon in data.results" :key="currentPokemon.name">

        <p>{{ currentPokemon.name }}</p>
        <p>Weight:{{ currentPokemon.weight }}</p>

        <img
          :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getPokemonId(currentPokemon.url)}.png`"
          :alt="currentPokemon.name"
        >
      </Card>

    </div>
    <div v-else>Carregando...</div>

  </div>
  

</template>

<style>
img {
  max-height: 200px;
  max-width: 200px;
}

.background {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #f6bd20;
}





</style>

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
