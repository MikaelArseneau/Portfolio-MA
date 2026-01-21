<template>
  <div v-if="showIntro" class="animationPage">
    <div class="numero">
      <div class="numero1">98 / 97 *</div>
      <div class="numero1">64 / 97 *</div>
      <div class="numero1">42 / 97 *</div>
      <div class="numero1">0 / 97 *</div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, defineEmits } from "vue";
import gsap from "gsap";

const showIntro = ref(true);
const emit = defineEmits(["finish"]);

onMounted(() => {
  document.body.style.overflow = "hidden";

  const numeros = document.querySelectorAll(".numero1");

  gsap
    .timeline()
    .fromTo(
      numeros,
      { opacity: 0, y: 50 },
      {
        opacity: 1,
        y: 0,
        stagger: {
          each: 0.4,
          from: "end",
          onStart: function () {
            const index = Array.from(numeros).indexOf(this.targets()[0]);
            if (index < numeros.length - 1) {
              gsap.to(numeros[index + 1], { opacity: 0, duration: 0.2 });
            }
          },
        },
        duration: 0.5,
        ease: "power2.out",
      },
    )
    .to({}, { duration: 0.5 })
    .to(".animationPage", {
      opacity: 0,
      duration: 0.5,
      onComplete: () => {
        showIntro.value = false;
        document.body.style.overflow = "auto";
        emit("finish"); // <-- important
      },
    });
});
</script>

<style scoped>
.animationPage {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background-color: black;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  z-index: 9999;
}

.numero {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 80%;
  align-self: flex-start;
  padding: 2rem;
}

.numero1 {
  font-size: 5em;
  font-weight: bold;
  opacity: 0;
}
</style>
