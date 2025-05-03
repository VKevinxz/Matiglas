<script setup lang="ts">
import { 
  Carousel, 
  CarouselContent, 
  CarouselItem, 
  CarouselNext, 
  CarouselPrevious 
} from '@/components/ui/carousel'
import AnimateOnScroll from "./AnimateOnScroll.vue";

interface ProyectoProps {
  imagen: string;
  titulo: string;
  descripcion: string;
}

const proyectos: ProyectoProps[] = [
  {
    imagen: "pacheco.png",
    titulo: "Remodelación Residencial Los Olivos",
    descripcion: "Servicio completo de pintura, albañilería y sistema eléctrico"
  },
  {
    imagen: "pacheco.png",
    titulo: "Oficinas Corporativas San Isidro",
    descripcion: "Instalación de mamparas y sistema drywall divisorio"
  },
  {
    imagen: "pacheco.png",
    titulo: "Edificio Multifamiliar Surco",
    descripcion: "Reparación y mantenimiento de instalaciones eléctricas"
  },
  {
    imagen: "pacheco.png",
    titulo: "Local Comercial La Victoria",
    descripcion: "Estructura metálica para ampliación y techo"
  },
  {
    imagen: "pacheco.png",
    titulo: "Residencia Familiar Miraflores",
    descripcion: "Remodelación completa de baños y cocina"
  }
]
</script>

<template>
  <section id="proyectos" class="w-full py-24 sm:py-32">
    <div class="px-4 md:px-8 lg:px-12 max-w-[2000px] mx-auto">
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider russo-font">Proyectos</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary russo-font">
          Nuestros Trabajos
        </h2>
        <p class="md:w-3/4 text-xl text-muted-foreground mb-12">
          Conoce algunos de nuestros principales proyectos donde hemos aplicado nuestros servicios profesionales, destacando por la calidad e innovación en cada implementación.
        </p>
      </AnimateOnScroll>

      <AnimateOnScroll :delay="300">
        <Carousel class="w-full">
          <CarouselContent>
            <CarouselItem v-for="proyecto in proyectos" :key="proyecto.titulo" class="md:basis-1/2 lg:basis-1/3">
              <div class="p-1">
                <div class="overflow-hidden rounded-lg border border-border bg-card hover:shadow-lg transition-all duration-300">
                  <img 
                    :src="proyecto.imagen" 
                    :alt="proyecto.titulo" 
                    class="h-52 w-full object-cover"
                  />
                  <div class="p-4">
                    <h3 class="font-bold text-secondary russo-font">{{ proyecto.titulo }}</h3>
                    <p class="text-muted-foreground">{{ proyecto.descripcion }}</p>
                  </div>
                </div>
              </div>
            </CarouselItem>
          </CarouselContent>
          <div class="flex justify-center gap-2 mt-4">
            <CarouselPrevious class="static translate-y-0 mx-2" />
            <CarouselNext class="static translate-y-0 mx-2" />
          </div>
        </Carousel>
      </AnimateOnScroll>
    </div>
  </section>
</template>

<style scoped>
.carousel-control {
  position: relative;
  transform: translateY(0);
  margin: 0 0.5rem;
}
</style>
