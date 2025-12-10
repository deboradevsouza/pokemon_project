<template>
    <div
        class="rounded-lg flex flex-col p-4 w-full max-w-[294px] cursor-pointer bg-[#EEEEEE] border-8 border-[#FFCC03] shadow-xl outline outline-2 outline-black outline-offset-[-8px] transition-all duration-300"
        @click="toggleInfo">
        
        <!-- Top row with content and icon -->
        <div class="flex flex-row justify-between items-center">
            <!-- This is the slot content that's always visible -->
            <div class="flex-1">
                <slot></slot>
            </div>
            
            <!-- Expand icon -->
            <div class="flex justify-center ml-2">
                <i 
                    class="bi bi-chevron-down transition-transform duration-300"
                    :class="{ 'rotate-180': aberto }"
                ></i>
            </div>
        </div>
        
        <!-- Expanded content -->
        <div class="poke-detalhes w-full" v-show="aberto">
            <hr class="my-3 border-white/30" />
            <slot name="expanded"></slot>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'

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

// Dynamic class based on type
const cardClasses = computed(() => {
  return typeColors[props.type] || typeColors.normal
})
</script>

<style scoped>
.poke-detalhes {
  display: flex;
}

.rotate-180 {
  transform: rotate(180deg);
}
</style>