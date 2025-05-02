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
  PaintBucket
} from "lucide-vue-next";
import AnimateOnScroll from './AnimateOnScroll.vue';

// Definiendo un tipo para las claves de iconos permitidas
type IconKey = 'paintbrush' | 'wrench' | 'home' | 'construction' | 'sprout' | 'shield-check' | 'hammer' | 'paint-bucket';

interface ServiceProps {
  icon: IconKey;
  title: string;
  description: string;
}

const services: ServiceProps[] = [
  {
    icon: "paintbrush",
    title: "Pintura y Acabados",
    description:
      "Servicios profesionales de pintura interior y exterior con acabados de primera calidad, aplicación de texturas y revestimientos especiales.",
  },
  {
    icon: "wrench",
    title: "Instalaciones Sanitarias",
    description:
      "Instalación y reparación de sistemas de agua y desagüe, termas, bombas de agua, cisternas y tanques elevados.",
  },
  {
    icon: "construction",
    title: "Construcción y Albañilería",
    description:
      "Edificación y remodelación de estructuras, demoliciones controladas, levantamiento de muros y tabiques divisorios.",
  },
  {
    icon: "hammer",
    title: "Carpintería",
    description:
      "Fabricación, reparación e instalación de muebles, puertas, ventanas y estructuras de madera para interiores y exteriores.",
  },
  {
    icon: "home",
    title: "Remodelaciones",
    description:
      "Renovación y actualización de espacios residenciales y comerciales, modificación de distribuciones y mejoramiento de ambientes.",
  },
  {
    icon: "shield-check",
    title: "Impermeabilización",
    description:
      "Protección contra filtraciones y humedad en techos, muros, losas y estructuras expuestas a condiciones climáticas adversas.",
  },
  {
    icon: "paint-bucket",
    title: "Resanes y Empastes",
    description:
      "Preparación y tratamiento de superficies, corrección de imperfecciones en paredes y techos previo al pintado.",
  },
  {
    icon: "sprout",
    title: "Áreas Verdes",
    description:
      "Diseño, instalación y mantenimiento de jardines, sistema de riego, instalación de grass natural y artificial.",
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
    class="container py-24 sm:py-32 relative overflow-hidden"
  >
    <!-- Elementos decorativos de fondo con parallax -->
    <div class="absolute -top-20 -left-20 w-64 h-64 bg-primary/5 rounded-full blur-3xl"></div>
    <div class="absolute -bottom-40 -right-20 w-80 h-80 bg-secondary/5 rounded-full blur-3xl"></div>
    
    <!-- Header de la sección con efectos -->
    <div :class="{'scale-in-center': sectionInView}" class="text-center mb-12 relative z-10">
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider glow-text">Servicios</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary">
          Nuestras <span class="creative-highlight">Especialidades</span>
        </h2>
        <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
          Ofrecemos soluciones integrales en construcción, mantenimiento y remodelación para proyectos residenciales y comerciales.
        </p>
      </AnimateOnScroll>
    </div>

    <!-- Grid de servicios con microinteracciones -->
    <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-4 w-full">
      <AnimateOnScroll
        v-for="(service, index) in services"
        :key="service.title"
        :delay="50 * index"
        :distance="30"
        :direction="index % 2 === 0 ? 'up' : 'down'"
      >
        <div 
          @mouseenter="rotateIcon(index); startBorderAnimation(index)" 
          @mouseleave="resetIconRotation(index); stopBorderAnimation(index)"
          class="h-full"
        >
          <Card
            class="service-card border group bg-background/60 backdrop-blur-sm hover:bg-background transition-all duration-300 h-full relative overflow-hidden"
            :class="{
              'border-primary/50': borderAnimationStates[index]
            }"
          >
            <!-- Efecto de borde animado -->
            <div 
              class="border-animation"
              :class="{ 'animate-border': borderAnimationStates[index] }"
            ></div>
            
            <!-- Efecto de resplandor en hover -->
            <div
              class="absolute inset-0 radial-gradient opacity-0 transition-opacity duration-500 group-hover:opacity-100"
            ></div>
            
            <CardHeader>
              <div class="icon-container mb-4">
                <component
                  :is="iconMap[service.icon]"
                  class="size-8 icon-transition text-primary transition-all duration-300"
                  :class="{
                    'rotate-12 scale-125': hoverStates[index]
                  }"
                />
              </div>
              <CardTitle class="text-lg font-bold relative z-10 text-secondary group-hover:text-secondary/90">{{ service.title }}</CardTitle>
            </CardHeader>
            <CardContent class="text-muted-foreground relative z-10">
              {{ service.description }}
            </CardContent>
            
            <!-- Indicador de flecha en hover -->
            <div 
              class="absolute bottom-4 right-4 opacity-0 transform translate-y-2 group-hover:opacity-100 group-hover:translate-y-0 transition-all duration-300 text-primary"
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
          <button class="btn-pulse mt-6 bg-primary hover:bg-primary/90 text-primary-foreground px-6 py-3 rounded-md font-medium transition-all duration-300 hover:-translate-y-1 hover:shadow-lg shadow-md">
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

/* Texto con brillo */
.glow-text {
  text-shadow: 0 0 5px rgba(241, 229, 20, 0.5);
}

/* Destacado creativo para texto */
.creative-highlight {
  position: relative;
  display: inline-block;
  color: hsl(var(--primary));
}

.creative-highlight::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: -5%;
  right: -5%;
  height: 30%;
  background-color: hsl(var(--primary) / 0.2);
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
}

@keyframes border-glow {
  0%, 100% {
    box-shadow: 0 0 0 2px hsl(var(--primary) / 0.2) inset;
  }
  50% {
    box-shadow: 0 0 0 2px hsl(var(--primary) / 0.6) inset;
  }
}

.animate-border {
  animation: border-glow 2s ease-in-out infinite;
}

/* Gradiente radial para el efecto hover */
.radial-gradient {
  background: radial-gradient(circle at center, rgba(241, 229, 20, 0.05) 0%, transparent 70%);
  transform: scale(0.9);
  transition: transform 0.5s ease-out;
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
  background-color: rgba(241, 229, 20, 0.08);
  transition: all 0.3s ease;
  padding: 8px;
}

.service-card:hover .icon-container {
  background-color: rgba(241, 229, 20, 0.15);
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
</style>
