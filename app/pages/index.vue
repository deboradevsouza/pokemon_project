<template>
  <div
  class="page-all" 
  >
  <main>
    <div class="page-container">
        <img 
          src="../assets/titulo-pokelift.png"
          alt="Titulo PokeLift"
          class="w-[50%] max-w-2xl md:max-w-md lg:max-w-lg mb-4"
        />

        <!-- SVG container -->
      <div class="svg-container">
        <svg viewBox="0 0 500 100">
          <!-- This path starts and ends with equal margins -->
          <path id="curve" d="M50,50 Q250,0 450,50" fill="transparent" />
          <text class="curved-text">
            <textPath href="#curve" startOffset="50%" text-anchor="middle">
              Gotta lift 'em all!
            </textPath>
          </text>
        </svg>
      </div>

      <img 
      class="pokelift-logo"
      src="../assets/Pokelift.png" alt="pokelift-logo"
      >
    </div>
  </main>

    <div
        class="link-pokemons"
        >
          <NuxtLink 
            to="/pokemon"
            class="link-text"
            >Ver Pokémons
            <i
            class="bi bi-arrow-right-circle"
            ></i>
          </NuxtLink>
    </div>

  <footer>
      <h3 class="footer-text">
        2025 | Desenvolvido por 
        <strong><a 
        href="https://github.com/deboradevsouza"
        target="_blank"
        rel="noopener noreferrer"
        >@deboradevsouza</a></strong> inspirado no projeto do 
        <strong><a 
        href="https://www.instagram.com/pikachudamaromba/"
        target="_blank"
        rel="noopener noreferrer"
        >@pikachudamaromba</a></strong>
      </h3>
    </footer>
    </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Tilt+Warp&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

.page-all {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 100vh;
  gap: 2rem;
  background-color: #77BEF0;
}

main {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  gap: 2rem;
  margin-top: 3rem;
}

.page-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100%;
}

.pokelift-logo {
  display: flex;
  min-height: 40vh;
  min-width: 300px;
}

.link-pokemons {
  display: flex;
  justify-content: flex-end;
  margin-right: 2rem;
}

.link-text {
  font-family: "Tilt Warp", sans-serif;
  font-weight: 400;
}

footer {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #FFCC03;
  padding: 1rem;
  border-top: 3px solid black;
}

.footer-text {
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  text-align: center;
}

/* Center the SVG container */
.svg-container {
  width: 100%;
  max-width: 600px;
  display: flex;
  justify-content: center;
}

.curved-text {
    font-family: "Tilt Warp", sans-serif;
    font-size: 25px;
    letter-spacing: 0.4rem;
    fill: white;
    text-shadow: 
    3px 3px 0 #386ABB,
    -3px 3px 0 #386ABB,
    3px -3px 0 #386ABB,
    -3px -3px 0 #386ABB,
    0 3px 0 #386ABB,
    0 -3px 0 #386ABB,
    3px 0 0 #386ABB,
    -3px 0 0 #386ABB;
}
</style>