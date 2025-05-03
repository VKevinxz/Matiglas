<script setup lang="ts">
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import AnimateOnScroll from './AnimateOnScroll.vue';
import { ArrowRight } from 'lucide-vue-next';

interface ProjectProps {
  id: number;
  title: string;
  description: string;
  image: string;
  category: string;
}

const projects: ProjectProps[] = [
  {
    id: 1,
    title: "Remodelación Edificio",
    description: "Renovación completa de fachada e interiores, incluyendo pintura, sistemas eléctricos y acabados.",
    image: "albañileria.jpg",
    category: "Remodelación"
  },
  {
    id: 2,
    title: "Instalación Eléctrica Industrial",
    description: "Diseño e implementación de sistema eléctrico de baja tensión para planta industrial.",
    image: "sistemas_electricos.jpg",
    category: "Electricidad"
  },
  {
    id: 3,
    title: "Construcción de Oficinas Corporativas",
    description: "Implementación de divisiones con drywall, cielo raso y carpintería para nuevas oficinas.",
    image: "drywall.jpg",
    category: "Construcción"
  },
   {
    id: 4,
    title: "Mantenimiento General Condominio",
    description: "Servicios integrales de gasfitería, pintura y limpieza de áreas comunes.",
    image: "pintura.jpg",
    category: "Mantenimiento"
  },
];

const scrollToElement = (id: string) => {
  if (typeof document !== 'undefined') {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<template>
  <section id="proyectos" class="w-full py-24 sm:py-32 bg-muted/30 dark:bg-muted/10 relative overflow-hidden">
    <!-- Top Separator (Curve) -->
    <div class="absolute top-0 left-0 right-0 z-0">
      <svg viewBox="0 0 1440 100" class="w-full h-auto transform rotate-180">
        <path fill="hsl(var(--background))" d="M0,64L48,58.7C96,53,192,43,288,48C384,53,480,75,576,85.3C672,96,768,96,864,85.3C960,75,1056,53,1152,48C1248,43,1344,53,1392,58.7L1440,64L1440,101L1392,101C1344,101,1248,101,1152,101C1056,101,960,101,864,101C768,101,672,101,576,101C480,101,384,101,288,101C192,101,96,101,48,101L0,101Z"></path>
      </svg>
    </div>

    <div class="max-w-[2000px] px-4 md:px-8 lg:px-12 mx-auto relative z-10 pt-12"> <!-- Added pt-12 for separator space -->
      <div class="text-center mb-12">
        <AnimateOnScroll animation="fade-up">
          <h2 class="text-lg text-primary mb-2 tracking-wider russo-font">Portafolio</h2>
          <h2 class="text-3xl md:text-4xl font-bold text-secondary russo-font">
            Nuestros <span class="creative-highlight">Trabajos Recientes</span>
          </h2>
          <p class="md:w-3/4 mx-auto mt-4 text-xl text-muted-foreground">
            Explora algunos de los proyectos que hemos completado exitosamente para nuestros clientes.
          </p>
        </AnimateOnScroll>
      </div>

      <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6">
        <AnimateOnScroll
          v-for="(project, index) in projects"
          :key="project.id"
          :delay="index * 100"
          animation="zoom-in"
          class="project-card-wrapper"
        >
          <Card class="h-full overflow-hidden group border border-border/50 hover:border-primary/30 transition-all duration-300 transform hover:-translate-y-1 hover:shadow-xl hover:shadow-primary/10">
            <div class="relative overflow-hidden h-48">
              <img
                :src="`/${project.image}`"
                :alt="project.title"
                class="w-full h-full object-cover transition-transform duration-500 ease-in-out group-hover:scale-110"
              />
              <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-black/30 to-transparent"></div>
              <span class="absolute top-2 right-2 bg-primary/80 text-black text-xs font-semibold px-2 py-0.5 rounded russo-font">{{ project.category }}</span>
            </div>
            <CardHeader class="pb-3 pt-4">
              <CardTitle class="text-lg font-semibold text-secondary group-hover:text-primary transition-colors duration-300 russo-font">{{ project.title }}</CardTitle>
            </CardHeader>
            <CardContent class="text-sm text-muted-foreground pb-4">
              <p>{{ project.description }}</p>
            </CardContent>
             <!-- Optional: Add a subtle link indicator -->
            <div class="absolute bottom-3 right-3 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                <ArrowRight class="size-4 text-primary" />
            </div>
          </Card>
        </AnimateOnScroll>
      </div>

       <!-- Optional: Button to view more projects -->
      <AnimateOnScroll :delay="projects.length * 100 + 100" animation="fade-up">
        <div class="mt-12 text-center">
            <button 
              class="btn-pulse bg-primary hover:bg-primary/90 text-black px-6 py-3 rounded-md font-medium transition-all duration-300 hover:-translate-y-1 hover:shadow-lg hover:shadow-primary/30 shadow-md russo-font"
              @click="scrollToElement('proyectos')"
            >
                <span class="relative z-10">Ver más proyectos</span>
            </button>
        </div>
      </AnimateOnScroll>

    </div>

     <!-- Bottom Separator (Curve) -->
    <div class="absolute bottom-0 left-0 right-0 z-0">
       <svg viewBox="0 0 1440 100" class="w-full h-auto">
         <path fill="hsl(var(--background))" d="M0,64L48,58.7C96,53,192,43,288,48C384,53,480,75,576,85.3C672,96,768,96,864,85.3C960,75,1056,53,1152,48C1248,43,1344,53,1392,58.7L1440,64L1440,101L1392,101C1344,101,1248,101,1152,101C1056,101,960,101,864,101C768,101,672,101,576,101C480,101,384,101,288,101C192,101,96,101,48,101L0,101Z"></path>
       </svg>
    </div>
  </section>
</template>

<style scoped>
.project-card-wrapper {
  /* Ensures shadows don't get clipped */
  padding-bottom: 4px;
}

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

/* Reusing button pulse effect if needed */
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

.russo-font {
  font-family: 'Russo One', sans-serif;
}
</style>
