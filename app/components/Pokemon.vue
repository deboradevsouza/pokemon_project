<template>
  <div>
    <div class="background" v-if="pokemons && pokemons.length">
      <Card 
        v-for="poke in pokemons" 
        :key="poke.id"
        :type="poke.primaryType"
      >
        <div class="ficha-pokemon">
          <img
            :src="poke.image"
            :alt="poke.name"
            class="max-h-32 mx-auto"
          />
          
          <div class="nome-pokemon-info" @click="toggleInfo(poke.id)">
            <h3 class="text-lg font-bold capitalize text-center">{{ poke.name }}</h3>
            
            <i 
              class="bi bi-chevron-down transition-transform duration-300 ml-2"
              :class="{ 'rotate-180': aberto[poke.id] }"
            ></i>
          </div>
        </div>

        <div class="gato-detalhes" v-show="aberto[poke.id]">
          <hr class="my-3 border-white/30" />
          <p>Peso: {{ poke.weight }} kg</p>
          <p>ID: {{ poke.id }}</p>
          <p>Tipo: {{ poke.primaryType }}</p>
        </div>
      </Card>
    </div>

    <div v-else>Carregando...</div>
  </div>
</template>

<script setup>
// ... same data fetching code ...

// Expand/collapse state
const aberto = ref({})

function toggleInfo(id) {
  aberto.value[id] = !aberto.value[id]
}
</script>