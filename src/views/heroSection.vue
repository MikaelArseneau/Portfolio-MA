<template>
  <section class="hero-ctn" id="Home">
    <h1 class="title-hero" @click="changeTitle">
      {{ titleText }}
    </h1>
    <div class="propos">
      Salut ! Bienvenue sur mon
      <span :style="{ color: colors[0] }">portfolio</span>. Je suis
      <span :style="{ color: colors[1] }">développeur web</span>
      et <span :style="{ color: colors[2] }">monteur vidéo</span>, deux passions
      qui se complètent super bien. J'adore créer des interfaces clean et
      animées avec Vue.js, tout en gardant cet œil créatif que le montage vidéo
      m'a apporté.
      <br />
      <br />
      Que ce soit pour coder un site stylé ou monter une vidéo percutante, je
      mets toute mon énergie dans mes projets. Tu as une idée en tête ? Un
      projet qui te tient à cœur ? N'hésite surtout pas à me
      <span class="contact" :style="{ color: colors[3] }" @click="openContact"
        >contacter</span
      >, ça me ferait plaisir d'en discuter autour d'une bonne bière (virtuelle
      ou réelle) !
    </div>
  </section>

  <!-- MODAL CONTACT -->
  <Transition name="slide-up">
    <ContactHeader v-if="showContact" @close="showContact = false" />
  </Transition>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import ContactHeader from "../components/specific/contactHeader.vue";

const showContact = ref(false);
const currentIndex = ref(0);
const texts = ["Développeur Front-End", "Montage Vidéo"];
let interval = null;

// Palette de couleurs au choix
const colorPalette = [
  "#646cff",
  "#ee6055",
  "#60d394",
  "#ff6b9d",
  "#ffa41b",
  "#5f27cd",
  "#00d2d3",
  "#ff6348",
  "#1e90ff",
  "#c44569",
];

// Fonction pour obtenir une couleur aléatoire
const getRandomColor = () => {
  return colorPalette[Math.floor(Math.random() * colorPalette.length)];
};

// Générer 4 couleurs aléatoires (une pour chaque span)
const colors = ref([
  getRandomColor(),
  getRandomColor(),
  getRandomColor(),
  getRandomColor(),
]);

function openContact() {
  showContact.value = true;
}

onMounted(() => {
  interval = setInterval(() => {
    currentIndex.value = (currentIndex.value + 1) % texts.length;
  }, 3000);
});

onUnmounted(() => {
  if (interval) {
    clearInterval(interval);
  }
});
const title = document.querySelector(".title-hero");

const titleText = ref("[ Mikael Arseneau .]");

function changeTitle() {
  titleText.value =
    titleText.value === "Arrête de frapper sa fais mal :("
      ? "[ Mikael Arseneau .]"
      : "Arrête de frapper sa fais mal :(";
}
</script>

<style scoped>
.hero-ctn {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: sticky;
  top: 0;
  overflow: hidden;
  z-index: 0;
  background-color: #f0f0f0;
  gap: 4rem;
  scroll-margin-top: 0;
}

.propos {
  font-family: "Archivo", sans-serif;
  font-size: 0.9rem;
  max-width: 600px;
  text-align: center;
  color: #333;
  line-height: 1.8;
}

.propos span {
  font-weight: 600;
  transition: color 0.3s ease;
}

.contact {
  cursor: pointer;
  position: relative;
  display: inline-block;
  z-index: 1;
  padding: 0 0.2em;
  text-decoration: underline;
  transition: color 0.3s ease;
}

/* Animation du fond coloré sur hover (identique à l'annexe) */
.contact::before {
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
.contact:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}

/* Le texte devient blanc au hover */
.contact:hover {
  color: white !important;
}

.title-hero {
  position: absolute;
  top: 2rem;
  left: 2rem;
  font-family: "Archivo", sans-serif;
  font-size: 1.2rem;
  margin: 0;
  color: #1a1a1a;
  cursor: pointer;
}

.titlte-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.subtitle-container {
  position: relative;
  height: 2.5em;
  overflow: hidden;
}

.subtitle-hero {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%) translateY(20px);
  font-size: 3em;
  font-weight: 400;
  margin: 0;
  font-family: "Archivo", sans-serif;
  color: #1a1a1a;
  opacity: 0;
  transition:
    opacity 0.8s ease-in-out,
    transform 0.8s ease-in-out;
  white-space: nowrap;
}

.subtitle-hero.active {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.scroll-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  animation: bounce 2s infinite;
}

.scroll-text {
  font-family: "Archivo", sans-serif;
  font-size: 0.9em;
  color: #666;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.arrow-down {
  width: 24px;
  height: 24px;
  border-right: 2px solid #666;
  border-bottom: 2px solid #666;
  transform: rotate(45deg);
}

@keyframes bounce {
  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
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

/* Responsive */
@media (max-width: 768px) {
  .title-hero {
    font-size: 1rem;
    top: 1.5rem;
    left: 1.5rem;
  }

  .propos {
    font-size: 0.8rem;
    max-width: 90%;
    padding: 0 1rem;
  }

  .subtitle-hero {
    font-size: 2em;
  }

  .subtitle-container {
    height: 2em;
  }
}

@media (max-width: 480px) {
  .title-hero {
    font-size: 0.9rem;
    top: 1rem;
    left: 1rem;
  }

  .propos {
    font-size: 0.75rem;
  }

  .subtitle-hero {
    font-size: 1.5em;
  }
}
</style>
