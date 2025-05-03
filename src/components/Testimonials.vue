<script setup lang="ts">
import { ref } from 'vue';
import AnimateOnScroll from './AnimateOnScroll.vue';
import { ChevronLeft, ChevronRight, Quote } from 'lucide-vue-next';

interface TestimonialProps {
  id: number;
  name: string;
  position: string;
  company: string;
  content: string;
  avatar: string;
}

interface ClientLogoProps {
  id: number;
  name: string;
  logo: string;
}

const testimonials: TestimonialProps[] = [
  {
    id: 1,
    name: 'Carlos Mendoza',
    position: 'Director',
    company: 'Constructora Andina',
    content: 'MATIGLAS ha sido un aliado estratégico en nuestros proyectos. Su profesionalismo y puntualidad en la entrega de trabajos son características que valoramos enormemente en nuestra relación comercial.',
    avatar: '/pacheco.png'
  },
  {
    id: 2,
    name: 'Ana María Flores',
    position: 'Gerente de Proyectos',
    company: 'Grupo Inmobiliario Lima',
    content: 'Han superado nuestras expectativas en cada proyecto. El personal técnico de MATIGLAS destaca por su conocimiento y compromiso, brindando soluciones efectivas aún en situaciones complejas.',
    avatar: '/pacheco.png'
  },
  {
    id: 3,
    name: 'Luis Ramírez',
    position: 'Propietario',
    company: 'Edificio Miraflores 360',
    content: 'Contratamos a MATIGLAS para renovar completamente nuestro edificio y el resultado fue excepcional. Cumplieron con los tiempos y costos acordados, manteniendo siempre un alto estándar de calidad.',
    avatar: '/pacheco.png'
  }
];

const clientLogos: ClientLogoProps[] = [
  { id: 1, name: 'Cliente 1', logo: '/pacheco.png' },
  { id: 2, name: 'Cliente 2', logo: '/pacheco.png' },
  { id: 3, name: 'Cliente 3', logo: '/pacheco.png' },
  { id: 4, name: 'Cliente 4', logo: '/pacheco.png' },
  { id: 5, name: 'Cliente 5', logo: '/pacheco.png' }
];

const currentTestimonialIndex = ref(0);

function prevTestimonial() {
  if (currentTestimonialIndex.value > 0) {
    currentTestimonialIndex.value--;
  } else {
    currentTestimonialIndex.value = testimonials.length - 1;
  }
}

function nextTestimonial() {
  if (currentTestimonialIndex.value < testimonials.length - 1) {
    currentTestimonialIndex.value++;
  } else {
    currentTestimonialIndex.value = 0;
  }
}

function setTestimonial(index: number) {
  currentTestimonialIndex.value = index;
}
</script>

<template>
  <section id="testimonios" class="w-full py-24 sm:py-32 dots-background relative">
    <!-- Elementos decorativos con estilo Bauhaus -->
    <div class="absolute top-0 right-0 w-4 md:w-8 h-16 md:h-32 bg-primary"></div>
    <div class="absolute bottom-0 left-0 w-16 md:w-32 h-4 md:h-8 bg-primary"></div>
    
    <div class="max-w-[2000px] px-4 md:px-8 lg:px-12 mx-auto">
      <div class="text-center mb-16 relative">
        <AnimateOnScroll>
          <h2 class="text-3xl md:text-4xl lg:text-5xl font-bold mb-4 russo-font">
            Lo que dicen <span class="text-primary">nuestros clientes</span>
          </h2>
          <div class="w-24 h-1 bg-primary mx-auto mt-6"></div>
        </AnimateOnScroll>
      </div>
      
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 md:gap-16 items-center">
        <!-- Testimonios con flecha de navegación -->
        <AnimateOnScroll>
          <div class="relative">
            <div class="testimonial-container">
              <div class="testimonial-slide" v-for="(testimonial, index) in testimonials" :key="testimonial.id"
                   :class="{ 'active': index === currentTestimonialIndex }">
                <div class="flex flex-col space-y-4">
                  <!-- Comillas decorativas -->
                  <div class="text-primary mb-2">
                    <Quote class="size-12 transform -scale-x-100 opacity-50 absolute -top-6 -left-2" />
                    <Quote class="size-12 opacity-50 absolute -bottom-12 -right-2" />
                  </div>
                  
                  <p class="text-lg text-muted-foreground italic leading-relaxed">
                    {{ testimonial.content }}
                  </p>
                  
                  <div class="flex items-center mt-4">
                    <div class="flex-shrink-0">
                      <img :src="testimonial.avatar" class="h-14 w-14 rounded-full object-cover" 
                           :alt="testimonial.name" />
                    </div>
                    <div class="ml-4">
                      <h4 class="font-bold text-secondary russo-font">{{ testimonial.name }}</h4>
                      <p class="text-sm text-muted-foreground">
                        {{ testimonial.position }}, <span class="text-primary">{{ testimonial.company }}</span>
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Controles de navegación -->
            <div class="flex justify-between mt-6">
              <div class="flex space-x-2">
                <button v-for="(_, index) in testimonials" :key="index"
                        @click="setTestimonial(index)"
                        class="w-3 h-3 rounded-full transition-all duration-300"
                        :class="index === currentTestimonialIndex ? 'bg-primary scale-125' : 'bg-muted hover:bg-primary/50'">
                </button>
              </div>
              
              <div class="flex space-x-2">
                <button @click="prevTestimonial" 
                        class="p-2 rounded-full border border-muted hover:border-primary hover:text-primary transition-colors duration-300">
                  <ChevronLeft class="size-5" />
                </button>
                <button @click="nextTestimonial" 
                        class="p-2 rounded-full border border-muted hover:border-primary hover:text-primary transition-colors duration-300">
                  <ChevronRight class="size-5" />
                </button>
              </div>
            </div>
          </div>
        </AnimateOnScroll>
        
        <!-- Logos de clientes -->
        <AnimateOnScroll :delay="200">
          <div class="logos-container">
            <h3 class="text-xl mb-8 font-medium text-secondary russo-font">Empresas que confían en nosotros</h3>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-6 items-center justify-items-center">
              <div v-for="logo in clientLogos" :key="logo.id" 
                   class="bg-card p-4 rounded-lg border border-border hover:border-primary/50 hover:shadow-md transition-all duration-300 w-full h-20 flex items-center justify-center">
                <img :src="logo.logo" :alt="logo.name" class="max-h-10 max-w-full filter grayscale hover:grayscale-0 transition-all duration-300" />
              </div>
            </div>
          </div>
        </AnimateOnScroll>
      </div>
    </div>
  </section>
</template>

<style scoped>
.testimonial-container {
  position: relative;
  overflow: hidden;
  min-height: 250px;
}

.testimonial-slide {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transform: translateX(-20px);
  transition: all 0.5s ease-in-out;
  visibility: hidden;
}

.testimonial-slide.active {
  opacity: 1;
  transform: translateX(0);
  position: relative;
  visibility: visible;
}

/* Estilo para los logos de clientes */
.logos-container img {
  transition: all 0.3s ease;
}

.logos-container img:hover {
  transform: scale(1.1);
}

/* Efecto para resaltar los controles de navegación */
button:focus {
  outline: 2px solid rgba(255, 204, 1, 0.5);
  outline-offset: 2px;
}
</style>
