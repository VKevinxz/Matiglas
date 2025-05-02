<script setup lang="ts">
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import AnimateOnScroll from "./AnimateOnScroll.vue";
import { ref } from "vue";

import {
  Target,
  Briefcase,
  Award,
  Clock,
} from "lucide-vue-next";

interface ValoresProps {
  icon: string;
  title: string;
  description: string;
}

const valoresList: ValoresProps[] = [
  {
    icon: "award",
    title: "Calidad",
    description:
      "Implementamos procesos y utilizamos materiales de primera calidad para garantizar un servicio que cumple con los más altos estándares.",
  },
  {
    icon: "briefcase",
    title: "Profesionalismo",
    description:
      "Contamos con personal técnico calificado y experimentado, que trabaja con seriedad y compromiso en cada proyecto asignado.",
  },
  {
    icon: "clock",
    title: "Puntualidad",
    description:
      "Cumplimos con los plazos acordados. Sabemos la importancia de los tiempos en cada proyecto y trabajamos para que nuestros clientes cumplan sus cronogramas.",
  },
  {
    icon: "target",
    title: "Versatilidad",
    description:
      "Ofrecemos una amplia gama de servicios especializados para dar solución integral a las necesidades de construcción y mantenimiento de su hogar o negocio.",
  },
];

const iconMap: Record<
  string,
  | typeof Award
  | typeof Clock
  | typeof Target
  | typeof Briefcase
> = {
  award: Award,
  clock: Clock,
  target: Target,
  briefcase: Briefcase,
};

// Para efectos interactivos
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
    class="container py-24 sm:py-32 relative"
  >
    <!-- Elemento decorativo de fondo con parallax -->
    <div class="absolute -top-20 -right-20 w-72 h-72 bg-primary/10 rounded-full blur-3xl opacity-70 animate-blob"></div>
    <div class="absolute -bottom-20 -left-20 w-72 h-72 bg-secondary/10 rounded-full blur-3xl opacity-70 animate-blob animation-delay-2000"></div>
    
    <div class="text-center mb-12 relative z-10">
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider">Nosotros</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary">
          MATIGLAS E.I.R.L.
        </h2>
        <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
          Empresa peruana especializada en servicios generales de construcción, 
          mantenimiento y remodelación para proyectos residenciales y comerciales.
        </p>
      </AnimateOnScroll>
    </div>

    <!-- Misión y Visión con imágenes -->
    <div class="grid md:grid-cols-2 gap-8 mb-16">
      <AnimateOnScroll>
        <div 
          class="bg-muted/30 dark:bg-card p-6 rounded-2xl border border-border flex flex-col h-full transform-gpu transition-all duration-500 hover:shadow-2xl hover:shadow-primary/10 relative overflow-hidden"
          @mouseenter="setHoveredBox('mision')" 
          @mouseleave="clearHoveredBox()"
        >
          <!-- Efecto de destello en el borde -->
          <span class="absolute top-0 left-0 w-full h-0.5 bg-gradient-to-r from-transparent via-primary to-transparent transform -translate-x-full benefit-border-animation"></span>
          
          <div class="flex items-center gap-4 mb-4 relative">
            <div class="bg-secondary-light p-2 rounded-lg transform transition-all duration-500" :class="{'scale-110 rotate-6': hoveredBox === 'mision'}">
              <img 
                src="/mision.png" 
                alt="Misión" 
                class="w-14 h-14 object-contain transition-transform duration-500"
                :class="{'animate-pulse': hoveredBox === 'mision'}"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300" :class="{'translate-x-2': hoveredBox === 'mision'}">Misión</h3>
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
            <div class="bg-primary-light p-2 rounded-lg transform transition-all duration-500" :class="{'scale-110 -rotate-6': hoveredBox === 'vision'}">
              <img 
                src="/vision.png" 
                alt="Visión" 
                class="w-14 h-14 object-contain transition-transform duration-500"
                :class="{'animate-pulse': hoveredBox === 'vision'}"
              />
            </div>
            <h3 class="text-2xl font-bold text-secondary transition-transform duration-300" :class="{'translate-x-2': hoveredBox === 'vision'}">Visión</h3>
          </div>
          
          <p class="text-muted-foreground flex-grow transition-colors duration-300" :class="{'text-foreground': hoveredBox === 'vision'}">
            "Ser la empresa de servicios líder en el mercado peruano, reconocida por su excelencia, innovación y compromiso con nuestros clientes, colaboradores y el medio ambiente."
          </p>
        </div>
      </AnimateOnScroll>
    </div>

    <!-- Valores -->
    <div class="mb-6">
      <AnimateOnScroll>
        <h3 class="text-2xl font-bold text-secondary text-center mb-8">Nuestros Valores</h3>
      </AnimateOnScroll>
    </div>

    <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-4 w-full relative z-10">
      <AnimateOnScroll
        v-for="({ icon, title, description }, index) in valoresList"
        :key="title"
        :delay="index * 100"
        :distance="40"
      >
        <Card
          class="bg-muted/50 dark:bg-card hover:bg-background dark:hover:bg-background transition-all duration-300 ease-in-out group/number border-t-2 border-t-primary/30 hover:border-t-primary/60 h-full flex flex-col transform hover:scale-[1.03] hover:shadow-xl"
          @mouseenter="setHoveredBox(index)" 
          @mouseleave="clearHoveredBox()"
        >
          <CardHeader class="flex-none">
            <div class="flex justify-between">
              <component
                class="size-8 mb-6 transition-all duration-500"
                :class="{
                  'text-secondary': hoveredBox !== index,
                  'text-primary rotate-12 scale-125': hoveredBox === index
                }"
                :is="iconMap[icon]"
              />

              <span
                class="text-5xl text-muted-foreground/15 font-medium transition-all delay-75 group-hover/number:text-primary/30"
                :class="{'scale-110 -translate-y-1': hoveredBox === index}"
                >0{{ index + 1 }}</span
              >
            </div>

            <CardTitle 
              class="text-secondary transition-all duration-300"
              :class="{'translate-y-[-3px]': hoveredBox === index}"
            >{{ title }}</CardTitle>
          </CardHeader>

          <CardContent class="text-muted-foreground flex-grow transition-colors duration-300" :class="{'text-foreground': hoveredBox === index}">
            {{ description }}
          </CardContent>
          
          <!-- Indicador visual de hover -->
          <div 
            class="absolute bottom-0 left-0 w-full h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-500 ease-out"
            :class="{'scale-x-100': hoveredBox === index}"
          ></div>
        </Card>
      </AnimateOnScroll>
    </div>
  </section>
</template>

<style scoped>
.bg-secondary-light {
  background-color: rgba(25, 81, 155, 0.1);
}

.bg-primary-light {
  background-color: rgba(241, 229, 20, 0.1);
}

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
</style>
