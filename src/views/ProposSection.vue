<template>
  <section class="propos" id="about">
    <p class="propos__description">
      <span v-for="(word, index) in words" :key="index" class="word">
        {{ word }}
        <span class="overlay"></span>
      </span>
    </p>
  </section>
</template>

<script setup>
import { onMounted, ref } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const text =
  "Passionné par le développement front-end et le montage vidéo, je m'efforce de créer des expériences utilisateur engageantes et visuellement attrayantes.";
const words = ref(text.split(" "));

onMounted(() => {
  const overlays = gsap.utils.toArray(".overlay");

  overlays.forEach((overlay) => {
    gsap.to(overlay, {
      width: 0,
      duration: 1,
      ease: "power2.out",
      scrollTrigger: {
        trigger: overlay,
        start: "top 80%",
        toggleActions: "play none none none",
      },
    });
  });
});
</script>

<style scoped>
.propos {
  width: 100%;
  padding: clamp(1.5rem, 4vw, 2rem);
  background-color: #0a0908;
  box-sizing: border-box;
  min-height: clamp(250px, 40vh, 400px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.propos__description {
  font-family: "Archivo", sans-serif;
  font-size: clamp(1.2rem, 4vw, 2.5rem);
  line-height: 1.6;
  color: #ced3dc;
  font-weight: 800;
  max-width: 1200px;
  margin: 0 auto;
}

.word {
  position: relative;
  display: inline-block;
  margin-right: 0.3em;
  overflow: hidden;
}

.overlay {
  position: absolute;
  inset: 0;
  background-color: #0a0908;
  width: 100%;
  left: 0;
  top: 0;
}

/* Tablette */
@media (max-width: 1024px) {
  .propos__description {
    font-size: clamp(1.1rem, 3.5vw, 2rem);
  }
}

/* Mobile */
@media (max-width: 768px) {
  .propos {
    padding: clamp(1.5rem, 5vw, 2.5rem) clamp(1rem, 4vw, 1.5rem);
    min-height: auto;
  }

  .propos__description {
    font-size: clamp(1rem, 5vw, 1.5rem);
    line-height: 1.5;
  }

  .word {
    margin-right: 0.25em;
  }
}

/* Petit mobile */
@media (max-width: 480px) {
  .propos {
    padding: 1.5rem 1rem;
  }

  .propos__description {
    font-size: 1.1rem;
    line-height: 1.4;
  }
}

/* Très petit écran */
@media (max-width: 360px) {
  .propos {
    padding: 1.2rem 0.8rem;
  }

  .propos__description {
    font-size: 1rem;
  }
}
</style>
