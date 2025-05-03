<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import {
  Paintbrush,
  Wrench,
  Home,
  Construction,
  Sprout,
  ShieldCheck,
  Hammer,
  PaintBucket,
  Lightbulb,
  Droplet,
  LayoutGrid,
  ScrollText
} from "lucide-vue-next";
import AnimateOnScroll from './AnimateOnScroll.vue';

// Definiendo un tipo para las claves de iconos permitidas
type IconKey = 'paintbrush' | 'wrench' | 'home' | 'construction' | 'sprout' | 'shield-check' | 'hammer' | 'paint-bucket' | 'lightbulb' | 'droplet' | 'layout-grid' | 'scroll-text';

interface ServiceProps {
  icon: IconKey;
  title: string;
  subdescription: string;
  hoverDescription: string;
  image: string;
}

const services: ServiceProps[] = [
  {
    icon: "paintbrush",
    title: "Pintura Profesional",
    subdescription: "Pinturas decorativas, satinadas, látex, oleomate, esmalte, epóxicas y laqueados.",
    hoverDescription: "Realizamos acabados de alta calidad con técnicas modernas para renovar y proteger sus espacios.",
    image: "pintura.jpg"
  },
  {
    icon: "lightbulb",
    title: "Sistemas Eléctricos",
    subdescription: "Sistemas eléctricos en general, tableros eléctricos, UPS, pozos a tierra, cámaras de vigilancia, sistemas de data, puertas eléctricas.",
    hoverDescription: "Soluciones completas e integrales en energía, tecnología y seguridad eléctrica, con mantenimiento especializado.",
    image: "sistemas_electricos.jpg"
  },
  {
    icon: "construction",
    title: "Albañilería General",
    subdescription: "Remodelaciones en general, encofrados, enchapados en general, remodelación de casas, edificios, locales, terrajeo, llenado de techos.",
    hoverDescription: "Remodelamos espacios con materiales duraderos y acabados de alta calidad para hogares, locales y edificaciones.",
    image: "albañileria.jpg"
  },
  {
    icon: "droplet",
    title: "Gasfitería",
    subdescription: "Instalaciones de sanitarios en general, desatoro en general, reparación de sistemas de ducto. Trabajos rápidos y efectivos.",
    hoverDescription: "Brindamos soluciones eficaces en instalaciones sanitarias, desatoros y mantenimiento con atención inmediata.",
    image: "gafiteria.jpg"
  },
  {
    icon: "hammer",
    title: "Carpintería",
    subdescription: "Puertas, mesas, escritorios, sistemas de melamina, armarios, divisiones transitorias, andamios, plataformas.",
    hoverDescription: "Fabricamos muebles y estructuras de madera a medida con precisión técnica y excelente acabado.",
    image: "carpinteria.jpg"
  },
  {
    icon: "layout-grid",
    title: "Drywall",
    subdescription: "Remodelaciones, divisiones de ambientes, falso techo y cielo raso.",
    hoverDescription: "Implementamos soluciones versátiles y rápidas para espacios modernos y bien organizados con sistemas de drywall.",
    image: "drywall.jpg"
  },
  {
    icon: "sprout",
    title: "Limpieza de Pozos Sépticos",
    subdescription: "Limpieza de pozos sépticos, desinfección, alquiler de bombas y sumergibles.",
    hoverDescription: "Servicio confiable y eficiente para el mantenimiento y limpieza profunda de sistemas sanitarios.",
    image: "pozos septicos.jpg"
  },
  {
    icon: "scroll-text",
    title: "Instalación de Mamparas",
    subdescription: "Suministro e instalación de puertas de vidrio, divisiones de vidrio, mantenimiento del sistema de frenos, sistema SEBA.",
    hoverDescription: "Instalación profesional de mamparas y divisiones de vidrio con sistemas seguros y funcionales.",
    image: "mamparas.jpg"
  },
  {
    icon: "shield-check",
    title: "Estructuras Metálicas",
    subdescription: "Fabricación de estructuras metálicas, techos aligerados, puertas canaladas, rejillas metálicas, barandas y escaleras de todo tipo.",
    hoverDescription: "Diseñamos y fabricamos estructuras metálicas resistentes y personalizadas para cada necesidad.",
    image: "estructuras_metalicas.jpg"
  },
];

// Definiendo el mapa de iconos con el tipo correcto
const iconMap: Record<IconKey, any> = {
  paintbrush: Paintbrush,
  wrench: Wrench,
  home: Home,
  construction: Construction,
  sprout: Sprout,
  "shield-check": ShieldCheck,
  hammer: Hammer,
  "paint-bucket": PaintBucket,
  lightbulb: Lightbulb,
  droplet: Droplet,
  "layout-grid": LayoutGrid,
  "scroll-text": ScrollText
};

// Para las microinteracciones en hover
const hoverStates = ref(Array(services.length).fill(false));

// Para efecto de borde animado
const borderAnimationStates = ref(Array(services.length).fill(false));

// Para el efecto de rotación de íconos
function rotateIcon(index: number): void {
  hoverStates.value[index] = true;
}

function resetIconRotation(index: number): void {
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
  
  // Observar la sección de servicios
  const section = document.querySelector('#servicios');
  if (section) {
    observer.observe(section);
  }
});
</script>

<template>
  <section
    id="servicios"
    class="w-full py-24 sm:py-32 relative overflow-hidden"
  >
    <!-- Elementos decorativos de fondo con parallax -->
    <div class="absolute -top-20 -left-20 w-64 h-64 bg-primary/5 rounded-full blur-3xl"></div>
    <div class="absolute -bottom-40 -right-20 w-80 h-80 bg-secondary/5 rounded-full blur-3xl"></div>
    
    <!-- Header de la sección con efectos -->
    <div :class="{'scale-in-center': sectionInView}" class="text-center mb-12 relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider glow-text russo-font">Servicios</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary russo-font">
          Nuestras <span class="creative-highlight">Especialidades</span>
        </h2>
        <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
          Ofrecemos soluciones integrales en construcción, mantenimiento y remodelación para proyectos residenciales y comerciales.
        </p>
      </AnimateOnScroll>
    </div>

    <!-- Grid de servicios con microinteracciones -->
    <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-8 w-full px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll
        v-for="(service, index) in services"
        :key="service.title"
        :delay="50 * index"
        :distance="30"
        :direction="index % 2 === 0 ? 'up' : 'down'"
        class="isolated-card" 
      >
        <div 
          class="h-full service-wrapper"
        >
          <Card
            class="service-card border bg-background/90 backdrop-blur-sm transition-all duration-300 h-full relative overflow-hidden dark:bg-card/90"
            :class="{
              'border-primary/50': borderAnimationStates[index]
            }"
            @mouseenter="rotateIcon(index); startBorderAnimation(index)" 
            @mouseleave="resetIconRotation(index); stopBorderAnimation(index)"
          >
            <!-- Imagen de fondo con overlay -->
            <div class="absolute inset-0 w-full h-full z-0">
              <img :src="`/${service.image}`" :alt="service.title" class="w-full h-full object-cover opacity-30 dark:opacity-20" />
              <div class="absolute inset-0 bg-background/70 dark:bg-background/80"></div>
            </div>
            
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
            
            <CardHeader class="relative z-10">
              <div class="icon-container mb-4">
                <component
                  :is="iconMap[service.icon]"
                  class="size-8 icon-transition text-primary transition-all duration-300"
                  :class="{
                    'rotate-12 scale-125': hoverStates[index]
                  }"
                />
              </div>
              <CardTitle class="text-lg font-bold relative z-10 text-secondary russo-font">{{ service.title }}</CardTitle>
            </CardHeader>
            <CardContent class="text-muted-foreground relative z-10 pb-16">
              <!-- Subdescripción siempre visible -->
              <p class="mb-4">{{ service.subdescription }}</p>
              
              <!-- Descripción hover con animación -->
              <div 
                class="hover-description-container absolute bottom-0 left-0 right-0 p-4 overflow-hidden opacity-0 transform translate-y-4 transition-all duration-300 mb-4 text-primary/90 z-20"
                :class="{ 'opacity-100 translate-y-0': hoverStates[index] }"
              >
                <p class="p-3 bg-primary/5 rounded-lg border border-primary/10">{{ service.hoverDescription }}</p>
              </div>
            </CardContent>
            
            <!-- Indicador de flecha en hover -->
            <div 
              class="absolute bottom-4 right-4 opacity-0 transform translate-y-2 transition-all duration-300 text-primary z-20"
              :class="{ 'opacity-100 translate-y-0': hoverStates[index] }"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M7 17l9.2-9.2M17 17V7H7"></path>
              </svg>
            </div>
          </Card>
        </div>
      </AnimateOnScroll>
    </div>

    <!-- CTA con efecto de pulso -->
    <AnimateOnScroll :delay="200">
      <div class="mt-12 text-center">
        <a href="#contacto">
          <button class="btn-pulse mt-6 bg-primary hover:bg-primary/90 text-black px-6 py-3 rounded-md font-medium transition-all duration-300 hover:-translate-y-1 hover:shadow-lg shadow-md russo-font">
            <span class="relative z-10">Solicitar un presupuesto</span>
          </button>
        </a>
      </div>
    </AnimateOnScroll>
  </section>
</template>

<style scoped>
/* Animación para las tarjetas de servicios */
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

.service-wrapper {
  isolation: isolate;
}

.service-card {
  isolation: isolate;
  will-change: auto; /* Optimize rendering */
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

.service-card:hover .radial-gradient {
  transform: scale(1.2);
}

/* Contenedor de icono con efecto */
.icon-container {
  position: relative;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  background-color: rgba(255, 204, 1, 0.08); /* Amarillo corporativo */
  transition: all 0.3s ease;
  padding: 8px;
  z-index: 10;
}

.service-card:hover .icon-container {
  background-color: rgba(255, 204, 1, 0.15); /* Amarillo corporativo */
  transform: scale(1.05);
}

/* Botón con efecto de pulso */
.btn-pulse {
  position: relative;
  overflow: hidden;
}

.btn-pulse::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.4), transparent 70%);
  transform: scale(0);
  transition: transform 0.5s cubic-bezier(0.19, 1, 0.22, 1);
  z-index: 1;
}

.btn-pulse:hover::before {
  transform: scale(2);
}

.dark .service-card {
  border-color: #1e1e1e; /* Borde en modo oscuro */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.dark .service-card:hover {
  box-shadow: 0 4px 20px rgba(255, 204, 1, 0.1); /* Amarillo corporativo */
}

/* Fuente Russo One */
.russo-font {
  font-family: 'Russo One', sans-serif;
}
</style>
