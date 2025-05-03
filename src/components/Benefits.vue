<script setup lang="ts">
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import AnimateOnScroll from "./AnimateOnScroll.vue";
import { ref, computed } from "vue"; // Import computed
import { useColorMode } from "@vueuse/core"; // Import useColorMode
import { Goal, Eye } from 'lucide-vue-next'; // Import Lucide icons

const mode = useColorMode(); // Get the color mode

const logoSrc = computed(() => {
  return mode.value === 'dark' ? '/matiglas_logo.png' : '/matiglas_logo_light.png';
});

interface ValoresProps {
  title: string;
  description: string;
  image: string;
}

const valoresList: ValoresProps[] = [
  {
    title: "Calidad",
    description:
      "Implementamos procesos y utilizamos materiales de primera calidad para garantizar un servicio que cumple con los más altos estándares.",
    image: "calidad.jpg",
  },
  {
    title: "Profesionalismo",
    description:
      "Contamos con personal técnico calificado y experimentado, que trabaja con seriedad y compromiso en cada proyecto asignado.",
    image: "profesionalismo.jpg",
  },
  {
    title: "Puntualidad",
    description:
      "Cumplimos con los plazos acordados. Sabemos la importancia de los tiempos en cada proyecto y trabajamos para que nuestros clientes cumplan sus cronogramas.",
    image: "puntualidad.jpg",
  },
  {
    title: "Versatilidad",
    description:
      "Ofrecemos una amplia gama de servicios especializados para dar solución integral a las necesidades de construcción y mantenimiento de su hogar o negocio.",
    image: "versatilidad.jpg",
  },
];

const hoveredBox = ref<number | string | null>(null);

const setHoveredBox = (index: number | string) => {
  hoveredBox.value = index;
};

const clearHoveredBox = () => {
  hoveredBox.value = null;
};
</script>

<template>
  <section
    id="nosotros"
    class="w-full py-24 sm:py-32 relative dots-background-primary overflow-hidden"
  >
    <!-- Top Separator -->
    <div class="absolute top-0 left-0 right-0 z-0">
      <svg viewBox="0 0 1440 100" class="w-full h-auto transform rotate-180">
        <path fill="hsl(var(--background))" d="M0,64L48,58.7C96,53,192,43,288,48C384,53,480,75,576,85.3C672,96,768,96,864,85.3C960,75,1056,53,1152,48C1248,43,1344,53,1392,58.7L1440,64L1440,101L1392,101C1344,101,1248,101,1152,101C1056,101,960,101,864,101C768,101,672,101,576,101C480,101,384,101,288,101C192,101,96,101,48,101L0,101Z"></path>
      </svg>
    </div>

    <!-- Elemento decorativo de fondo con parallax -->
    <div class="absolute -top-20 -right-20 w-72 h-72 bg-primary/10 rounded-full blur-3xl opacity-70 animate-blob"></div>
    <div class="absolute -bottom-20 -left-20 w-72 h-72 bg-secondary/10 rounded-full blur-3xl opacity-70 animate-blob animation-delay-2000"></div>
    
    <div class="text-center mb-12 relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll animation="fade-up">
        <h2 class="text-lg text-primary mb-2 tracking-wider russo-font">Nosotros</h2>
        <div class="flex justify-center mb-6">
          <img :src="logoSrc" alt="MATIGLAS Logo" class="max-w-[200px] h-auto" />
        </div>
        <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
          Empresa peruana especializada en servicios generales de construcción, 
          mantenimiento y remodelación para proyectos residenciales y comerciales.
        </p>
      </AnimateOnScroll>
    </div>

    <!-- Misión y Visión con iconos de Lucide -->
    <div class="grid md:grid-cols-2 gap-8 mb-16 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto relative z-10">
      <AnimateOnScroll animation="fade-right">
        <div 
          class="bg-muted/30 dark:bg-card p-6 rounded-2xl border border-border flex flex-col h-full transform-gpu transition-all duration-500 hover:shadow-2xl hover:shadow-primary/10 relative overflow-hidden group/mision"
          @mouseenter="setHoveredBox('mision')" 
          @mouseleave="clearHoveredBox()"
        >
          <!-- Efecto de destello en el borde -->
          <span class="absolute top-0 left-0 w-full h-0.5 bg-gradient-to-r from-transparent via-primary to-transparent transform -translate-x-full group-hover/mision:translate-x-full transition-transform duration-1000 ease-out"></span>
          
          <div class="flex items-center gap-4 mb-4 relative">
            <div class="bg-secondary/10 p-3 rounded-lg transform transition-all duration-500 group-hover/mision:scale-110 group-hover/mision:rotate-6">
              <Goal 
                class="w-12 h-12 text-primary transition-transform duration-500 group-hover/mision:animate-pulse"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300 group-hover/mision:translate-x-2 russo-font">Misión</h3>
          </div>
          
          <p class="text-muted-foreground flex-grow transition-colors duration-300 group-hover/mision:text-foreground">
            "Brindar servicios que excedan las expectativas de nuestros clientes, garantizando calidad, profesionalismo y puntualidad en cada uno de nuestros proyectos, contribuyendo así al desarrollo y bienestar de la comunidad."
          </p>
        </div>
      </AnimateOnScroll>

      <AnimateOnScroll animation="fade-left" :delay="200">
        <div 
          class="bg-muted/30 dark:bg-card p-6 rounded-2xl border border-border flex flex-col h-full transform-gpu transition-all duration-500 hover:shadow-2xl hover:shadow-primary/10 relative overflow-hidden group/vision"
          @mouseenter="setHoveredBox('vision')" 
          @mouseleave="clearHoveredBox()"
        >
          <!-- Efecto de destello en el borde -->
          <span class="absolute bottom-0 right-0 w-full h-0.5 bg-gradient-to-l from-transparent via-primary to-transparent transform translate-x-full group-hover/vision:-translate-x-full transition-transform duration-1000 ease-out"></span>
          
          <div class="flex items-center gap-4 mb-4">
            <div class="bg-primary/10 p-3 rounded-lg transform transition-all duration-500 group-hover/vision:scale-110 group-hover/vision:-rotate-6">
              <Eye 
                class="w-12 h-12 text-primary transition-transform duration-500 group-hover/vision:animate-pulse"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300 group-hover/vision:translate-x-2 russo-font">Visión</h3>
          </div>
          
          <p class="text-muted-foreground flex-grow transition-colors duration-300 group-hover/vision:text-foreground">
            "Ser la empresa de servicios líder en el mercado peruano, reconocida por su excelencia, innovación y compromiso con nuestros clientes, colaboradores y el medio ambiente."
          </p>
        </div>
      </AnimateOnScroll>
    </div>

    <!-- Valores -->
    <div class="relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <div class="mb-6">
        <AnimateOnScroll animation="zoom-in">
          <h3 class="text-2xl font-bold text-secondary text-center mb-8 russo-font">Nuestros Valores</h3>
        </AnimateOnScroll>
      </div>

      <!-- Background for Valores section -->
      <div class="absolute inset-0 z-0 opacity-5 dark:opacity-[0.02] overflow-hidden rounded-3xl">
        <img src="/matiglas_workers.jpg" alt="Background" class="w-full h-full object-cover object-center scale-110 blur-sm"/>
        <div class="absolute inset-0 bg-gradient-to-t from-background via-background/80 to-background"></div>
      </div>

      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 w-full relative z-10">
        <AnimateOnScroll
          v-for="({ title, description, image }, index) in valoresList"
          :key="title"
          :delay="index * 100"
          :distance="40"
          animation="fade-up"
        >
          <Card
            class="bg-muted/30 dark:bg-card hover:bg-background dark:hover:bg-background transition-all duration-300 ease-in-out group/number border border-border/50 hover:border-primary/30 h-full flex flex-col transform hover:scale-[1.03] hover:shadow-xl hover:shadow-primary/10 relative overflow-hidden"
            @mouseenter="setHoveredBox(index)" 
            @mouseleave="clearHoveredBox()"
          >
            <!-- Imagen como fondo tenue -->
            <div class="absolute inset-0 w-full h-full z-0">
              <img :src="`/${image}`" :alt="title" class="w-full h-full object-cover opacity-10 dark:opacity-5 group-hover/number:opacity-20 transition-opacity duration-300" />
              <div class="absolute inset-0 bg-gradient-to-t from-background/80 via-background/50 to-transparent dark:from-background/90 dark:via-background/70"></div>
            </div>

            <CardHeader class="flex-none relative z-10">
              <div class="flex justify-between items-start mb-4">
                <!-- Imagen pequeña en esquina -->
                <div 
                  class="p-1.5 bg-primary/10 rounded-lg border border-primary/20 transition-all duration-500 transform group-hover/number:rotate-6 group-hover/number:scale-110"
                >
                  <img 
                    :src="`/${image}`" 
                    :alt="title" 
                    class="w-10 h-10 object-cover rounded-md"
                  />
                </div>

                <span
                  class="text-5xl text-muted-foreground/15 font-medium transition-all delay-75 group-hover/number:text-primary/30 group-hover/number:scale-110 group-hover/number:-translate-y-1 russo-font"
                  >0{{ index + 1 }}</span
                >
              </div>

              <CardTitle 
                class="text-secondary transition-all duration-300 group-hover/number:translate-y-[-3px] group-hover/number:text-primary/90 russo-font"
              >{{ title }}</CardTitle>
            </CardHeader>

            <CardContent class="text-muted-foreground flex-grow relative z-10 transition-colors duration-300 group-hover/number:text-foreground">
              {{ description }}
            </CardContent>
            
            <!-- Indicador visual de hover -->
            <div 
              class="absolute bottom-0 left-0 w-full h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-500 ease-out z-10 group-hover/number:scale-x-100"
            ></div>
          </Card>
        </AnimateOnScroll>
      </div>
    </div>
  </section>
</template>

<style scoped>
.benefit-border-animation {
  animation: slide-right 3s linear infinite;
}

@keyframes slide-right {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(100%);
  }
}

.animate-blob {
  animation: blob-bounce 7s infinite ease;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

@keyframes blob-bounce {
  0% {
    transform: translate(0, 0) scale(1);
  }
  33% {
    transform: translate(30px, -30px) scale(1.1);
  }
  66% {
    transform: translate(-20px, 20px) scale(0.9);
  }
  100% {
    transform: translate(0, 0) scale(1);
  }
}

.delay-1000 {
  animation-delay: 1s;
}

/* Ensure card content is above the background image */
.card-header,
.card-content {
  position: relative;
  z-index: 10;
}

/* Eliminando los grupos hover vacíos que causaban error */
</style>
