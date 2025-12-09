<template>
  <div>
    <div class="background" v-if="pokemons && pokemons.length">
      <Card 
        v-for="poke in pokemons" 
        :key="poke.id"
        :type="poke.primaryType"
      >
        <!-- Main content (always visible) -->
        <div class="ficha-pokemon">
          <img
            :src="poke.image"
            :alt="poke.name"
            class="max-h-32 mx-auto"
          />
          
          <div class="nome-pokemon-info">
            <h3 class="text-lg font-bold capitalize text-center">{{ poke.name }}</h3>
          </div>
        </div>

        <!-- Expanded content -->
        <template #expanded>
          <div class="space-y-2">
            <p><span class="font-medium">Peso:</span> {{ poke.weight }} kg</p>
            <p><span class="font-medium">ID:</span> {{ poke.id }}</p>
            <p><span class="font-medium">Tipo:</span> {{ poke.primaryType }}</p>
            
            <div v-if="poke.baseExperience">
              <p><span class="font-medium">Experiência base:</span> {{ poke.baseExperience }}</p>
            </div>
            <div v-if="poke.height">
              <p><span class="font-medium">Altura:</span> {{ poke.height }} m</p>
            </div>
          </div>
        </template>
      </Card>
    </div>

    <div v-else>Carregando...</div>
  </div>
</template>

<script setup>
// Usa useAsyncData com cache automático do Nuxt
const { data: pokemons, error, pending } = await useAsyncData('pokemons', async () => {
  //Busca a lista principal
  const response = await $fetch('https://pokeapi.co/api/v2/pokemon?limit=20')

  //Busca os detalhes (peso, imagem etc.)
  const results = await Promise.all(
    response.results.map(async (poke) => {
      const detail = await $fetch(poke.url)
      const id = poke.url.split('/').filter(Boolean).pop()

      return {
        id,
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

//Trata erros se houver
if (error.value) {
  console.error('Erro ao buscar Pokémon:', error.value)
}
</script>

<style>
img {
  max-height: 200px;
  max-width: 200px;
}

.background {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 1rem;
}

.ficha-pokemon {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.nome-pokemon-info {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 0.5rem;
  width: 100%;
}
</style>