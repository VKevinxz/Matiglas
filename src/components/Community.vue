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
  <section id="proyectos" class="container py-24 sm:py-32">
    <div>
      <AnimateOnScroll>
        <h2 class="text-lg text-primary mb-2 tracking-wider">Proyectos</h2>
        <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary">
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
                <div class="overflow-hidden rounded-lg border border-border">
                  <img 
                    :src="proyecto.imagen" 
                    :alt="proyecto.titulo" 
                    class="h-52 w-full object-cover"
                  />
                  <div class="p-4">
                    <h3 class="font-bold text-secondary">{{ proyecto.titulo }}</h3>
                    <p class="text-muted-foreground">{{ proyecto.descripcion }}</p>
                  </div>
                </div>
              </div>
            </CarouselItem>
          </CarouselContent>
          <CarouselPrevious />
          <CarouselNext />
        </Carousel>
      </AnimateOnScroll>
    </div>
  </section>
</template>
