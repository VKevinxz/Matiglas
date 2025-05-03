<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Card, CardContent, CardTitle } from "@/components/ui/card";
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

const hoverStates = ref(Array(services.length).fill(false));
const borderAnimationStates = ref(Array(services.length).fill(false));

function rotateIcon(index: number): void {
  hoverStates.value[index] = true;
}

function resetIconRotation(index: number): void {
  hoverStates.value[index] = false;
}

function startBorderAnimation(index: number): void {
  borderAnimationStates.value[index] = true;
}

function stopBorderAnimation(index: number): void {
  borderAnimationStates.value[index] = false;
}

const sectionInView = ref(false);

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        sectionInView.value = true;
      } else {
        sectionInView.value = false;
      }
    });
  }, { threshold: 0.2 });
  
  const section = document.querySelector('#servicios');
  if (section) {
    observer.observe(section);
  }
});

// Function to navigate to a section
const navigateTo = (hash: string) => {
  if (typeof window !== 'undefined') {
    window.location.href = hash;
  }
};
</script>

<template>
  <section
    id="servicios"
    class="w-full py-24 sm:py-32 relative overflow-hidden bg-gradient-to-b from-background via-muted/30 to-background"
  >
    <div class="absolute top-0 left-0 w-full h-16 md:h-24 bg-background -skew-y-2 z-0"></div>

    <div class="absolute -top-20 -left-20 w-64 h-64 bg-primary/5 rounded-full blur-3xl"></div>
    <div class="absolute -bottom-40 -right-20 w-80 h-80 bg-secondary/5 rounded-full blur-3xl"></div>
    
    <div class="text-center mb-12 relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto pt-8">
      <AnimateOnScroll animation="fade-up">
        <h2 class="text-lg text-primary mb-2 tracking-wider glow-text russo-font">Servicios</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary russo-font">
          Nuestras <span class="creative-highlight">Especialidades</span>
        </h2>
        <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
          Ofrecemos soluciones integrales en construcción, mantenimiento y remodelación para proyectos residenciales y comerciales.
        </p>
      </AnimateOnScroll>
    </div>

    <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-8 w-full px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto relative z-10">
      <AnimateOnScroll
        v-for="(service, index) in services"
        :key="service.title"
        :delay="50 * index"
        :distance="30"
        :animation="index % 2 === 0 ? 'fade-up' : 'fade-down'"
        class="isolated-card"
      >
        <div class="h-full service-wrapper group/service">
          <Card
            class="service-card border-0 shadow-md transition-all duration-500 h-full relative overflow-hidden dark:bg-card/90 group-hover/service:shadow-xl group-hover/service:shadow-primary/10 group-hover/service:border-primary/30 flex flex-col justify-center items-center text-center"
            @mouseenter="rotateIcon(index); startBorderAnimation(index)"
            @mouseleave="resetIconRotation(index); stopBorderAnimation(index)"
          >
            <div class="absolute inset-0 w-full h-full z-0">
              <img 
                :src="`/${service.image}`" 
                :alt="service.title" 
                class="w-full h-full object-cover opacity-100 group-hover/service:opacity-20 transition-opacity duration-500 group-hover/service:scale-105"
              />
            </div>
            
            <div
              class="absolute inset-0 bg-background/80 dark:bg-background/80 opacity-0 transition-opacity duration-300 group-hover/service:opacity-100"
            ></div>
            
            <div class="relative z-10 p-4 flex flex-col items-center justify-center h-full w-full">
              <div 
                class="icon-container mb-4 opacity-0 transition-all duration-300 group-hover/service:opacity-100 group-hover/service:transform group-hover/service:translate-y-0 translate-y-10"
              >
                <component
                  :is="iconMap[service.icon]"
                  class="size-8 icon-transition text-primary transition-all duration-300 group-hover/service:rotate-12"
                />
              </div>

              <div class="bg-black/40 px-3 py-2 rounded-md backdrop-blur-sm group-hover/service:bg-transparent group-hover/service:backdrop-blur-0 transition-all duration-300">
                <CardTitle 
                  class="text-xl font-bold text-primary russo-font transition-all duration-300 group-hover/service:-translate-y-2"
                >
                  {{ service.title }}
                </CardTitle>
              </div>

              <CardContent 
                class="text-muted-foreground relative z-10 mt-4 opacity-0 transform translate-y-5 transition-all duration-300 group-hover/service:opacity-100 group-hover/service:translate-y-0 group-hover/service:text-foreground pb-0 px-0 w-full"
              >
                <p class="mb-4">
                  {{ service.subdescription }}
                </p>
                
                <div 
                  class="hover-description-container overflow-hidden text-primary/90 w-full"
                >
                  <p class="px-3 py-2 bg-primary/5 rounded-lg border border-primary/10 text-sm">
                    {{ service.hoverDescription }}
                  </p>
                </div>
              </CardContent>
            </div>
            
            <div 
              class="absolute bottom-4 right-4 opacity-0 transform translate-y-2 transition-all duration-300 text-primary z-30 group-hover/service:opacity-100 group-hover/service:translate-y-0"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M7 17l9.2-9.2M17 17V7H7"></path>
              </svg>
            </div>
          </Card>
        </div>
      </AnimateOnScroll>
    </div>

    <AnimateOnScroll :delay="200" animation="fade-up">
      <div class="mt-16 text-center relative z-10">
        <a href="#contacto">
          <button class="btn-pulse mt-6 bg-primary hover:bg-primary/90 text-black px-6 py-3 rounded-md font-medium transition-all duration-300 hover:-translate-y-1 hover:shadow-lg hover:shadow-primary/30 shadow-md russo-font" @click="navigateTo('#contacto')">
            <span class="relative z-10">Solicitar un presupuesto</span>
          </button>
        </a>
      </div>
    </AnimateOnScroll>

    <div class="absolute bottom-0 left-0 w-full h-16 md:h-24 bg-background skew-y-2 z-0"></div>
  </section>
</template>

<style scoped>
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

.isolated-card {
  isolation: isolate;
}

.service-wrapper {
  isolation: isolate;
}

.service-card {
  isolation: isolate;
  will-change: auto;
  display: flex;
  flex-direction: column;
}

.glow-text {
  text-shadow: 0 0 5px rgba(255, 204, 1, 0.5);
}

.creative-highlight {
  position: relative;
  display: inline-block;
  color: #FFCC01;
}

.creative-highlight::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: -5%;
  right: -5%;
  height: 30%;
  background-color: rgba(255, 204, 1, 0.2);
  z-index: -1;
  transform: rotate(-2deg) skewX(-15deg);
}

.icon-container {
  position: relative;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  background-color: rgba(255, 204, 1, 0.15);
  padding: 8px;
  z-index: 10;
}

.service-card .card-title {
  color: #FFCC01;
}

.service-card > * {
  position: relative;
  z-index: 10;
}

.service-card > div:first-child {
  z-index: 0;
}

.hover-description-container {
  background-color: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(2px);
}

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
  border-color: #1e1e1e;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.dark .service-card:hover {
  box-shadow: 0 4px 20px rgba(255, 204, 1, 0.1);
}

.russo-font {
  font-family: 'Russo One', sans-serif;
}

.service-card > .border-animation,
.service-card > .radial-gradient,
.service-card > div:first-child {
  z-index: 0;
}

.service-card p {
  margin-bottom: 0.5rem;
  max-width: 100%;
  overflow-wrap: break-word;
  font-size: 0.95rem;
}

.dark .hover-description-container {
  background-color: rgba(0, 0, 0, 0.2);
}
</style>
