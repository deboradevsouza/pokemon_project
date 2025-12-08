<template>
  <div>
    <div class="background" v-if="pokemons && pokemons.length">
      <Card v-for="poke in pokemons" :key="poke.id"
      :type="poke.primaryType"
      >
        <p>{{ poke.name }}</p>
        <p>Peso: {{ poke.weight }}kg</p>

        <img
          :src="poke.image"
          :alt="poke.name"
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
}
</style>

<script setup>
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
        id,
        name: poke.name,
        weight: detail.weight/10,
        image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`,
        primaryType: detail.types[0]?.type?.name || 'normal'
      }
    })
  )

  return results
})

//Trata erros se houver
if (error.value) {
  console.error('Erro ao buscar Pokémon:', error.value)
}
</script>
