<template>
  <main class="min-h-screen bg-[#77BEF0]">
    <div class="flex flex-col min-h-screen items-center p-4">
      <!-- Make image responsive but with max size -->
      <img 
        src="../assets/titulo-pokelift.png" 
        alt="Titulo PokeLift"
        class="w-[60%] max-w-4xl md:max-w-5xl lg:max-w-6xl"
      />

      <h2 class="font-tilt-warp">Gotta lift 'em all!</h2>
    </div>
  </main>
</template>

<script setup>

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Tilt+Warp&display=swap');

h2 {
    font-family: "Tilt Warp", sans-serif;
    font-size: 20px;
    color: white;
    text-shadow: 
        3px 3px 0 #000,
    -3px 3px 0 #000,
    3px -3px 0 #000,
    -3px -3px 0 #000,
    0 3px 0 #000,
    0 -3px 0 #000,
    3px 0 0 #000,
    -3px 0 0 #000;
}


</style>