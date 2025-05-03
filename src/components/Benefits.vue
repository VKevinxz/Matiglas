<script setup lang="ts">
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import AnimateOnScroll from "./AnimateOnScroll.vue";
import { ref, computed } from "vue"; // Import computed
import { useColorMode } from "@vueuse/core"; // Import useColorMode

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
    class="w-full py-24 sm:py-32 relative dots-background-primary"
  >
    <!-- Elemento decorativo de fondo con parallax -->
    <div class="absolute -top-20 -right-20 w-72 h-72 bg-primary/10 rounded-full blur-3xl opacity-70 animate-blob"></div>
    <div class="absolute -bottom-20 -left-20 w-72 h-72 bg-secondary/10 rounded-full blur-3xl opacity-70 animate-blob animation-delay-2000"></div>
    
    <div class="text-center mb-12 relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
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

    <!-- Misión y Visión con imágenes -->
    <div class="grid md:grid-cols-2 gap-8 mb-16 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
        <div 
          class="bg-muted/30 dark:bg-card p-6 rounded-2xl border border-border flex flex-col h-full transform-gpu transition-all duration-500 hover:shadow-2xl hover:shadow-primary/10 relative overflow-hidden"
          @mouseenter="setHoveredBox('mision')" 
          @mouseleave="clearHoveredBox()"
        >
          <!-- Efecto de destello en el borde -->
          <span class="absolute top-0 left-0 w-full h-0.5 bg-gradient-to-r from-transparent via-primary to-transparent transform -translate-x-full benefit-border-animation"></span>
          
          <div class="flex items-center gap-4 mb-4 relative">
            <div class="bg-secondary/10 p-2 rounded-lg transform transition-all duration-500" :class="{'scale-110 rotate-6': hoveredBox === 'mision'}">
              <img 
                src="/mision.png" 
                alt="Misión" 
                class="w-14 h-14 object-contain transition-transform duration-500"
                :class="{'animate-pulse': hoveredBox === 'mision'}"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300 russo-font" :class="{'translate-x-2': hoveredBox === 'mision'}">Misión</h3>
          </div>
          
          <p class="text-muted-foreground flex-grow transition-colors duration-300" :class="{'text-foreground': hoveredBox === 'mision'}">
            "Brindar servicios que excedan las expectativas de nuestros clientes, garantizando calidad, profesionalismo y puntualidad en cada uno de nuestros proyectos, contribuyendo así al desarrollo y bienestar de la comunidad."
          </p>
        </div>
      </AnimateOnScroll>

      <AnimateOnScroll :delay="200">
        <div 
          class="bg-muted/30 dark:bg-card p-6 rounded-2xl border border-border flex flex-col h-full transform-gpu transition-all duration-500 hover:shadow-2xl hover:shadow-primary/10 relative overflow-hidden"
          @mouseenter="setHoveredBox('vision')" 
          @mouseleave="clearHoveredBox()"
        >
          <!-- Efecto de destello en el borde -->
          <span class="absolute top-0 left-0 w-full h-0.5 bg-gradient-to-r from-transparent via-primary to-transparent transform -translate-x-full benefit-border-animation delay-1000"></span>
          
          <div class="flex items-center gap-4 mb-4">
            <div class="bg-primary/10 p-2 rounded-lg transform transition-all duration-500" :class="{'scale-110 -rotate-6': hoveredBox === 'vision'}">
              <img 
                src="/vision.png" 
                alt="Visión" 
                class="w-14 h-14 object-contain transition-transform duration-500"
                :class="{'animate-pulse': hoveredBox === 'vision'}"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300 russo-font" :class="{'translate-x-2': hoveredBox === 'vision'}">Visión</h3>
          </div>
          
          <p class="text-muted-foreground flex-grow transition-colors duration-300" :class="{'text-foreground': hoveredBox === 'vision'}">
            "Ser la empresa de servicios líder en el mercado peruano, reconocida por su excelencia, innovación y compromiso con nuestros clientes, colaboradores y el medio ambiente."
          </p>
        </div>
      </AnimateOnScroll>
    </div>

    <!-- Valores -->
    <div class="mb-6 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
        <h3 class="text-2xl font-bold text-secondary text-center mb-8 russo-font">Nuestros Valores</h3>
      </AnimateOnScroll>
    </div>

    <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6 w-full relative z-10 px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll
        v-for="({ title, description, image }, index) in valoresList"
        :key="title"
        :delay="index * 100"
        :distance="40"
      >
        <Card
          class="bg-muted/30 dark:bg-card hover:bg-background dark:hover:bg-background transition-all duration-300 ease-in-out group/number border border-border/50 hover:border-primary/30 h-full flex flex-col transform hover:scale-[1.03] hover:shadow-xl relative overflow-hidden"
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
                class="p-1.5 bg-primary/10 rounded-lg border border-primary/20 transition-all duration-500 transform"
                :class="{
                  'rotate-6 scale-110': hoveredBox === index
                }"
              >
                <img 
                  :src="`/${image}`" 
                  :alt="title" 
                  class="w-10 h-10 object-cover rounded-md"
                />
              </div>

              <span
                class="text-5xl text-muted-foreground/15 font-medium transition-all delay-75 group-hover/number:text-primary/30 russo-font"
                :class="{'scale-110 -translate-y-1': hoveredBox === index}"
                >0{{ index + 1 }}</span
              >
            </div>

            <CardTitle 
              class="text-secondary transition-all duration-300 russo-font"
              :class="{'translate-y-[-3px] text-primary/90': hoveredBox === index}"
            >{{ title }}</CardTitle>
          </CardHeader>

          <CardContent class="text-muted-foreground flex-grow relative z-10 transition-colors duration-300" :class="{'text-foreground': hoveredBox === index}">
            {{ description }}
          </CardContent>
          
          <!-- Indicador visual de hover -->
          <div 
            class="absolute bottom-0 left-0 w-full h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-500 ease-out z-10"
            :class="{'scale-x-100': hoveredBox === index}"
          ></div>
        </Card>
      </AnimateOnScroll>
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
</style>
