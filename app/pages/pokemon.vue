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
      class="rounded-full p-2 flex items-center"
      type="text"
      placeholder="Buscar Pokémon">
      <button>
        <i
        class="bi bi-search">
        </i>
      </button>
    </div>
  </header>

  <div class="background">
    <div 
    class="flex flex-col gap-2"
    v-if="pokemons && pokemons.length">
      <Card 
        v-for="poke in pokemons" 
        :key="poke.id"
        :type="poke.primaryType"
      >
        <!-- Main content (always visible) -->
        <div class="ficha-pokemon">
          
          <div class="nome-pokemon-info">
            <h3 class="nome-poke capitalize text-center">
              # 0{{ poke.id }}</h3>
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