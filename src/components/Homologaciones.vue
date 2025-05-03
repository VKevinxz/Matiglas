<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Card, CardContent } from "@/components/ui/card";
import AnimateOnScroll from './AnimateOnScroll.vue';

interface CertificationProps {
  logo: string;
  title: string;
  alt: string;
}

const certifications: CertificationProps[] = [
  {
    logo: "homolgado-ccl.jpg",
    title: "Empresa Homologada por la Cámara de Comercio de Lima",
    alt: "Homologación Cámara de Comercio de Lima"
  },
  {
    logo: "Hodelpe_Homogado.jpg",
    title: "Proveedor Homologado por HODELPE",
    alt: "Homologación HODELPE"
  }
];

// Para las microinteracciones en hover
const hoverStates = ref(Array(certifications.length).fill(false));

// Para el efecto de borde animado
const borderAnimationStates = ref(Array(certifications.length).fill(false));

// Para efectos de hover
function startHoverEffect(index: number): void {
  hoverStates.value[index] = true;
}

function stopHoverEffect(index: number): void {
  hoverStates.value[index] = false;
}

// Para el efecto de borde animado
function startBorderAnimation(index: number): void {
  borderAnimationStates.value[index] = true;
}

function stopBorderAnimation(index: number): void {
  borderAnimationStates.value[index] = false;
}

// Variable para controlar la escala de los elementos cuando se hace scroll
const sectionInView = ref(false);

onMounted(() => {
  // Configurar un observer para detectar cuando la sección está en la vista
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        sectionInView.value = true;
      } else {
        sectionInView.value = false;
      }
    });
  }, { threshold: 0.2 });
  
  // Observar la sección de homologaciones
  const section = document.querySelector('#homologaciones');
  if (section) {
    observer.observe(section);
  }
});
</script>

<template>
  <section
    id="homologaciones"
    class="w-full py-16 sm:py-24 relative overflow-hidden bg-[#043856]"
  >
    <!-- Elementos decorativos de fondo con parallax -->
    <div class="absolute -top-20 -left-20 w-64 h-64 bg-primary/5 rounded-full blur-3xl"></div>
    <div class="absolute -bottom-40 -right-20 w-80 h-80 bg-primary/5 rounded-full blur-3xl"></div>
    
    <!-- Header de la sección con efectos -->
    <div :class="{'scale-in-center': sectionInView}" class="text-center mb-12 relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider glow-text russo-font">Homologaciones</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-white russo-font">
          Confianza <span class="creative-highlight">Reconocida</span>
        </h2>
        <p class="md:w-3/4 mx-auto text-xl text-white/80">
          Contamos con certificaciones que avalan nuestra calidad y seriedad.
        </p>
      </AnimateOnScroll>
    </div>

    <!-- Grid de certificaciones con microinteracciones -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 w-full px-4 md:px-8 lg:px-12 max-w-[900px] mx-auto">
      <AnimateOnScroll
        v-for="(certification, index) in certifications"
        :key="certification.title"
        :delay="50 * index"
        :distance="30"
        :direction="index % 2 === 0 ? 'left' : 'right'"
        class="isolated-card" 
      >
        <div 
          class="h-full certification-wrapper"
        >
          <Card
            class="certification-card bg-white shadow-md transition-all duration-300 h-full relative overflow-hidden border-0 hover:scale-105 hover:shadow-xl"
            :class="{
              'border-primary/50': borderAnimationStates[index]
            }"
            @mouseenter="startHoverEffect(index); startBorderAnimation(index)" 
            @mouseleave="stopHoverEffect(index); stopBorderAnimation(index)"
          >
            <!-- Efecto de borde animado solo al elemento actual -->
            <div 
              class="border-animation"
              :class="{ 'animate-border': borderAnimationStates[index] }"
            ></div>
            
            <!-- Efecto de resplandor en hover solo al elemento actual -->
            <div
              class="absolute inset-0 radial-gradient opacity-0 transition-opacity duration-300"
              :class="{ 'opacity-100': hoverStates[index] }"
            ></div>
            
            <CardContent class="flex flex-col items-center p-6">
              <div class="mb-6 transform transition-all duration-300 hover:scale-105">
                <img 
                  :src="`/${certification.logo}`" 
                  :alt="certification.alt"
                  class="w-[150px] h-auto mx-auto object-contain"
                />
              </div>
              <h3 class="text-lg font-semibold text-center text-gray-800">
                {{ certification.title }}
              </h3>
            </CardContent>
            
            <!-- Indicador de brillo en hover -->
            <div 
              class="absolute inset-0 bg-primary/5 opacity-0 transition-opacity duration-300"
              :class="{ 'opacity-100': hoverStates[index] }"
            ></div>
          </Card>
        </div>
      </AnimateOnScroll>
    </div>
  </section>
</template>

<style scoped>
/* Animación para las tarjetas de certificaciones */
.scale-in-center {
  animation: scale-in-center 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
}

@keyframes scale-in-center {
  0% {
    transform: scale(0.9);
    opacity: 0;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

/* Aislar los efectos de hover para evitar afectar a elementos adyacentes */
.isolated-card {
  isolation: isolate;
}

.certification-wrapper {
  isolation: isolate;
}

.certification-card {
  isolation: isolate;
  will-change: transform; /* Optimize rendering */
}

/* Texto con brillo */
.glow-text {
  text-shadow: 0 0 5px rgba(255, 204, 1, 0.5); /* Amarillo corporativo */
}

/* Destacado creativo para texto */
.creative-highlight {
  position: relative;
  display: inline-block;
  color: #FFCC01; /* Amarillo corporativo */
}

.creative-highlight::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: -5%;
  right: -5%;
  height: 30%;
  background-color: rgba(255, 204, 1, 0.2); /* Amarillo corporativo */
  z-index: -1;
  transform: rotate(-2deg) skewX(-15deg);
}

/* Efecto de borde animado */
.border-animation {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  z-index: 0;
}

@keyframes border-glow {
  0%, 100% {
    box-shadow: 0 0 0 2px rgba(255, 204, 1, 0.2) inset; /* Amarillo corporativo */
  }
  50% {
    box-shadow: 0 0 0 2px rgba(255, 204, 1, 0.6) inset; /* Amarillo corporativo */
  }
}

.animate-border {
  animation: border-glow 2s ease-in-out infinite;
}

/* Gradiente radial para el efecto hover */
.radial-gradient {
  background: radial-gradient(circle at center, rgba(255, 204, 1, 0.05) 0%, transparent 70%); /* Amarillo corporativo */
  transform: scale(0.9);
  transition: transform 0.5s ease-out, opacity 0.3s ease-out;
}

.certification-card:hover .radial-gradient {
  transform: scale(1.2);
}

/* Fuente Russo One */
.russo-font {
  font-family: 'Russo One', sans-serif;
}
</style>