<template>
  <section class="annexe">
    <div class="bg">
      <div class="ContenuDisplay">
        <a class="GitHub" href="#Home">Accueil</a>
        <a class="GitHub" @click.prevent="openContact">Contact</a>
        <a class="GitHub" href="#projets">Projets</a>
        <a class="GitHub" href="#Competence">Compétences</a>
      </div>
      <div class="ContenuDisplay">
        <a
          class="GitHub"
          href="https://github.com/MikaelArseneau"
          target="_blank"
          >Github</a
        >
        <a
          class="GitHub"
          href="https://www.linkedin.com/in/mikael-arseneau/

"
          target="_blank"
          >linkedin</a
        >
      </div>
    </div>
  </section>

  <footer>
    <div>{{ timeDisplay }}</div>
    <div class="nom_footer">
      <div class="nom">Mikael Arseneau</div>
      <div class="nom">Développeur Front-End</div>
    </div>
  </footer>

  <!-- MODAL CONTACT -->
  <Transition name="slide-up">
    <ContactHeader v-if="showContact" @close="showContact = false" />
  </Transition>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import ContactHeader from "../components/specific/contactHeader.vue";

const showContact = ref(false);
const timeDisplay = ref("00:00:00");
let intervalId = null;

function openContact() {
  showContact.value = true;
}

function updateTime() {
  const now = new Date();
  const hours = now.getHours().toString().padStart(2, "0");
  const minutes = now.getMinutes().toString().padStart(2, "0");
  const seconds = now.getSeconds().toString().padStart(2, "0");
  timeDisplay.value = `${hours}:${minutes}:${seconds}`;
}

onMounted(() => {
  updateTime();
  intervalId = setInterval(updateTime, 1000);
});

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId);
  }
});
</script>

<style scoped>
.nom_footer {
  display: flex;
  flex-direction: row;
  align-items: center;
  font-family: "Archivo", sans-serif;
  text-transform: uppercase;
  font-size: 1.6em;
  gap: 1em;
}
.annexe {
  width: 100%;
  background-color: #f0f0f0;
  height: auto;
  position: relative;
  z-index: 10;
  min-height: 30vh; /* Pour couvrir toute la hauteur si nécessaire */
}
.bg {
  padding-top: 2em;
  padding-left: 0.8em;
  display: flex;
  flex-direction: row;
  width: 50%;
  position: relative;
  left: 0;
  justify-content: space-evenly;
  margin-bottom: 4em;
  z-index: 2;
  background-color: #f0f0f0;
  height: auto;
}

.ContenuDisplay {
  display: flex;
  flex-direction: column;
  gap: 0.5em;
}

.plateformeDisplay {
  display: flex;
  flex-direction: column;
  gap: 0.5em;
}

.GitHub {
  text-decoration: none;
  color: #0a0908;
  cursor: pointer;
  position: relative;
  display: inline-block;
  transition: color 0.3s ease;
  z-index: 1;
  text-transform: capitalize;
  font-size: 1.1em;
  padding-right: 1em;
}

/* Animation du fond coloré sur hover (avant l'élément) */
.GitHub::before {
  content: "";
  position: absolute;
  inset: 0;
  background-color: black;
  z-index: -1;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease-in-out;
}

/* Quand on passe la souris → animation d'apparition du fond */
.GitHub:hover::before,
.GitHub:focus::before {
  transform: scaleX(1);
  transform-origin: left;
}

/* Le texte devient blanc au hover */
.GitHub:hover,
.GitHub:focus {
  color: white;
}

footer {
  display: flex;
  width: 100vw;
  justify-content: space-between;
  gap: 16px;
  font-family: "Archivo", sans-serif;
  text-transform: uppercase;
  padding-right: 20px;
  color: #0a0908;
  position: relative;
  z-index: 2;
  overflow-x: hidden;
}

/* ANIMATION SLIDE DU BAS */
.slide-up-enter-from {
  transform: translateY(100%);
  opacity: 1;
}

.slide-up-enter-active {
  transition: transform 0.7s cubic-bezier(0.77, 0, 0.175, 1);
}

.slide-up-enter-to {
  transform: translateY(0%);
}

/* SORTIE */
.slide-up-leave-from {
  transform: translateY(0%);
}

.slide-up-leave-active {
  transition: transform 0.6s cubic-bezier(0.77, 0, 0.175, 1);
}

.slide-up-leave-to {
  transform: translateY(100%);
}
</style>
