<template>
  <section class="projects-section">
    <!-- <h2 class="projects-title" id="projets">Mes Projets *</h2> -->

    <div
      class="display_project"
      :class="{ 'modal-open': selectedProject !== null }"
    >
      <div
        class="projects_ctn"
        v-for="(project, index) in projets"
        :key="index"
        @click="openModal(index)"
      >
        <div class="projet">
          <img :src="project.image" :alt="project.titre" class="image_projet" />
          <span class="titre_projet">{{ project.titre }}</span>
        </div>
      </div>
    </div>

    <!-- MODALE -->
    <Transition name="modal-fade">
      <div
        v-if="selectedProject !== null"
        class="modal-overlay"
        @click.self="closeModal"
      >
        <div class="modal-content" @click="checkClose">
          <h2 class="modal-header">
            {{ projets[selectedProject].titre }}
          </h2>

          <div class="modal-body">
            <h3 class="modal-description">
              {{ projets[selectedProject].description }}
            </h3>
            <div
              v-if="projets[selectedProject].collaboration"
              class="collaboration"
            >
              <h4>Collaboration :</h4>
              <ul>
                <h5>
                  <li
                    v-for="(collab, idx) in projets[selectedProject]
                      .collaboration"
                    :key="idx"
                  >
                    {{ collab }}
                  </li>
                </h5>
              </ul>
            </div>
            <div v-if="projets[selectedProject].resultat" class="collaboration">
              <h4>Résultat :</h4>
              <p>{{ projets[selectedProject].resultat }}</p>
            </div>

            <div v-if="projets[selectedProject].site" class="site-link">
              Voir le projet :
              <a :href="projets[selectedProject].site" target="_blank">
                {{ projets[selectedProject].titre }}</a
              >
            </div>
            <hr class="divider" />
            <div class="images-grid">
              <img
                v-for="i in projets[selectedProject].images || [
                  projets[selectedProject].image,
                ]"
                :key="i"
                :src="i"
                :alt="projets[selectedProject].titre"
                class="modal_image_in"
              />
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </section>
</template>

<script setup>
import { ref, watch } from "vue";

const selectedProject = ref(null);

const projets = [
  {
    titre: "Aureo.",
    description:
      "Plateforme dédiée à la créativité et à l'inspiration, pour découvrir, partager et admirer des œuvres artistiques variées — photographie, design graphique, architectures, illustrations et styles *",
    image: "./aureo.webp",
    collaboration: ["Alexandre Gendron", "Rafael Angon Dubé"],
    logiciel: ["Figma", "Vscode"],
    site: "https://mikaelarseneau.github.io/Aureo/",
    images: ["./aureo.webp", "./aureo3.webp", "./aureo1.webp", "./aureo2.webp"],
  },
  {
    titre: "Métrage.",
    description:
      "Site de critiques cinématographiques — films, séries et courts-métrages — rédigées avec soin et passion *",
    collaboration: ["Alexandre Gendron", "Rafael Angon Dubé"],
    logiciel: ["Figma", "Wordpress"],
    image: "./metrage.webp",
    images: [
      "./metrage.webp",
      "./metrage2.webp",
      "./metrage3.webp",
      "./metrage5.webp",
      "./metrage4.webp",
    ],
  },
  {
    titre: "Transforsmonstre.",
    collaboration: ["Alexandre Gendron", "Rafael Angon Dubé", "Mathieu Willet"],
    image: "./hero-bg.jpg",
    description:
      "Un court-métrage créé à partir de scènes de films d'Halloween pour donner vie à un tout nouveau monstre. Projeté sur la Place Bell le 31 octobre 2025 *",
    logiciel: ["Davinci"],
    site: "https://youtu.be/g_uvBUEz45w",
  },
  {
    titre: "Arbre en face.",
    image: "./arbre_en_face.webp",
    collaboration: [
      " Alexandre Gendron",
      "Rafael Angon Dubé",
      "Mathieu Willet",
    ],
    description:
      "Une expérience où les participants font apparaître des graines en forme de visages. Celles-ci tombent et s'enracinent, puis peuvent être arrosées grâce à un arrosoir virtuel. Selon la quantité d'eau reçue, elles deviennent une fleur, un buisson ou un arbre, toujours avec les visages des participants précédents intégrés dans les fleurs, fruits ou pommes *",
    logiciel: ["TouchDesigner"],
    resultat: "En développement",
    site: "https://mammouths.github.io/projet/#/",
  },
  {
    titre: "Terminus.",
    image: "./terminus.webp",
    collaboration: ["Alexandre Gendron", "Rafael Angon Dubé", "Mathieu Willet"],
    description:
      "Terminus est un court métrage que nous avons réalisé dans le cadre d'un projet de sensibilisation aux dangers de l'alcool au volant. Le film combine des prises de vue en caméra normale et en macro afin de créer une ambiance immersive et expressive *",
    logiciel: ["TouchDesigner"],
    site: "https://youtu.be/NqmFHKgiS5o",
    images: [
      "./terminus.webp",
      "./terminus2.webp",
      "./terminus3.webp",
      "./terminus4.webp",
      "./terminus5.webp",
    ],
  },
  {
    titre: "Coeur de la montagne.",
    image: "./montagne.webp",

    description:
      "Cœur de la montagne est une vidéo expérimentale mêlant animation 3D et sons créés avec des synthétiseurs. Le projet plonge le spectateur dans l'ascension d'une montagne à travers une expérience visuelle et sonore immersive *",
    logiciel: ["Maya"],
    site: "https://youtu.be/3KDPisDvK8E",
    images: [
      "./montagne.webp",
      "./montagne2.webp",
      "./montagne3.webp",
      "./montagne4.webp",
      "./montagne5.webp",
    ],
  },
  {
    titre: "Chasseur de crânes.",
    image: "./chasseur3.webp",

    description:
      "Jouer en tant qu'aventurier pris au piège dans un donjon. Il devra survivre aux nombreux ennemis qui cherchent à le vaincre et atteindre la sortie en vie. *",
    logiciel: ["Maya"],
    site: "https://github.com/AlexandreGendronCM/ProjetJeuVr_RealiteMixte",
    images: [
      "./chasseur3.webp",
      "./chasseur2.webp",
      "./chasseur1.webp",
      "./chasseur4.webp",
    ],
  },
];

function openModal(index) {
  selectedProject.value = index;
}

function closeModal() {
  selectedProject.value = null;
}

watch(selectedProject, (newValue) => {
  document.body.style.overflow = newValue !== null ? "hidden" : "";
});

function checkClose(event) {
  const tag = event.target.tagName.toLowerCase();
  if (
    tag !== "img" &&
    tag !== "h3" &&
    tag !== "a" &&
    tag !== "h2" &&
    tag !== "h5"
  ) {
    closeModal();
  }
}
</script>

<style scoped>
h2 {
  cursor: default;
}
/* ================== BASE ================== */
.collaboration {
  display: flex;
  flex-direction: row;
  gap: 0.5em;
  margin-bottom: 1em;
  flex-wrap: wrap;
}

li {
  margin-left: 1em;
  list-style-type: "-";
}

.divider {
  width: 100%;
  max-width: 400px;
  border: none;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  margin: 2rem 0;
}

/* ================== SECTION ================== */
.projects-section {
  position: relative;
  z-index: 2;
  background-color: #f7f7f7;
  min-height: 100vh;
  width: 100%;
  padding: 2rem;
}

/* ================== TITRE ================== */
.projects-title {
  padding: 2rem;
  font-size: 5rem;
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 3rem;
}

/* ================== PROJETS ================== */
.display_project {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 2rem;
  transition: opacity 0.4s ease;
  padding-top: 4em;
}

.projet {
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  gap: 0.8rem;
}

.image_projet {
  width: 100px;
  height: 100px;
  object-fit: cover;
  transition:
    width 0.6s ease,
    height 0.6s ease,
    opacity 0.4s ease;
}

.titre_projet {
  font-size: 0.8rem;
  font-weight: 600;
  text-transform: uppercase;
  text-align: center;
  transition: opacity 0.4s ease;
}

/* ✨ QUAND MODALE OUVERTE ✨ */
.display_project.modal-open .image_projet {
  width: 25px;
  height: 25px;
  opacity: 0.6;
}

.display_project.modal-open .titre_projet {
  opacity: 0;
}

.display_project.modal-open {
  pointer-events: none;
}

/* ================== MODALE ================== */
.modal-overlay {
  position: fixed;
  inset: 0;
  backdrop-filter: blur(5px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 1rem;
}

.modal-content {
  background: transparent;
  border-radius: 16px;
  max-width: 100%;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  padding: 2rem;
  box-sizing: border-box;
  cursor: pointer;
}

.modal-header {
  font-size: 2rem;
  font-weight: 600;
  margin-bottom: 1rem;
  text-align: center;
}

.modal-body {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  justify-content: center;
}

.modal-description {
  font-size: 0.8em;
  cursor: default;
  text-align: center;
  width: 100%;
  max-width: 400px;
  margin-bottom: 2rem;
}

.site-link {
  margin: 1rem 0;
  text-align: center;
}

.site-link a {
  color: #646cff;
  text-decoration: underline;
}

.images-grid {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
  align-items: center;
}

.modal_image_in {
  width: 100%;
  max-width: 400px;
  height: auto;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  cursor: default;
  border-radius: 8px;
}

/* ================== ANIMATION ================== */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.8s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

/* ================== RESPONSIVE ================== */

/* ================== RESPONSIVE ================== */

/* Tablettes (768px et moins) */
@media (max-width: 768px) {
  .projects-section {
    padding: 1rem;
  }

  .projects-title {
    font-size: 3rem;
    padding: 1rem;
    margin-bottom: 2rem;
  }

  .display_project {
    gap: 1.5rem;
    padding-top: 2em;
  }

  .image_projet {
    width: 80px;
    height: 80px;
  }

  .titre_projet {
    font-size: 0.7rem;
  }

  .modal-content {
    padding: 1.5rem;
  }

  .modal-header {
    font-size: 1.5rem;
  }

  .modal-description {
    font-size: 0.9em;
    max-width: 100%;
  }

  /* 👇 IMAGES RÉDUITES SUR TABLETTE */
  .modal_image_in {
    max-width: 300px;
  }

  .collaboration {
    flex-direction: column;
    gap: 0.3em;
  }
}

/* Mobiles (480px et moins) */
@media (max-width: 480px) {
  .projects-title {
    font-size: 2rem;
    padding: 0.5rem;
  }

  .display_project {
    gap: 1rem;
    padding-top: 1em;
    justify-content: center;
  }

  .image_projet {
    width: 60px;
    height: 60px;
  }

  .titre_projet {
    font-size: 0.6rem;
  }

  .modal-overlay {
    padding: 0.5rem;
  }

  .modal-content {
    padding: 1rem;
    max-height: 95vh;
  }

  .modal-header {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
  }

  .modal-description {
    font-size: 0.8em;
    margin-bottom: 1rem;
  }

  .images-grid {
    gap: 1rem;
  }

  /* 👇 IMAGES ENCORE PLUS PETITES SUR MOBILE */
  .modal_image_in {
    max-width: 250px;
  }

  .divider {
    margin: 1rem 0;
  }

  .collaboration h4 {
    font-size: 0.9rem;
  }

  .collaboration li {
    font-size: 0.8rem;
    margin-left: 0.5em;
  }

  .site-link {
    font-size: 0.9rem;
  }

  .display_project.modal-open .image_projet {
    width: 15px;
    height: 15px;
  }
}

/* Petits mobiles (360px et moins) */
@media (max-width: 360px) {
  .projects-title {
    font-size: 1.5rem;
  }

  .modal-header {
    font-size: 1rem;
  }

  .modal-description {
    font-size: 0.75em;
  }

  /* 👇 IMAGES TRÈS PETITES SUR PETITS ÉCRANS */
  .modal_image_in {
    max-width: 200px;
  }
}
</style>
