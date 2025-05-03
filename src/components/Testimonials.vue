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
const transitionName = ref('slide-next'); // For controlling transition direction

function prevTestimonial() {
  transitionName.value = 'slide-prev';
  if (currentTestimonialIndex.value > 0) {
    currentTestimonialIndex.value--;
  } else {
    currentTestimonialIndex.value = testimonials.length - 1;
  }
}

function nextTestimonial() {
  transitionName.value = 'slide-next';
  if (currentTestimonialIndex.value < testimonials.length - 1) {
    currentTestimonialIndex.value++;
  } else {
    currentTestimonialIndex.value = 0;
  }
}

function setTestimonial(index: number) {
  transitionName.value = index > currentTestimonialIndex.value ? 'slide-next' : 'slide-prev';
  currentTestimonialIndex.value = index;
}
</script>

<template>
  <section id="testimonios" class="w-full py-24 sm:py-32 relative overflow-hidden">
    <!-- Background Image -->
    <div class="absolute inset-0 z-0 opacity-[0.03] dark:opacity-[0.02]">
      <img src="/estructuras_metalicas.jpg" alt="Background" class="w-full h-full object-cover object-center filter grayscale"/>
      <div class="absolute inset-0 bg-gradient-to-b from-background via-background/90 to-background"></div>
    </div>

    <!-- Decorative Elements -->
    <div class="absolute top-10 right-10 w-4 md:w-8 h-16 md:h-32 bg-primary/20 rounded-sm transform rotate-12 opacity-50"></div>
    <div class="absolute bottom-10 left-10 w-16 md:w-32 h-4 md:h-8 bg-primary/20 rounded-sm transform -rotate-6 opacity-50"></div>

    <div class="max-w-[2000px] px-4 md:px-8 lg:px-12 mx-auto relative z-10">
      <div class="text-center mb-16 relative">
        <AnimateOnScroll animation="zoom-in">
          <h2 class="text-3xl md:text-4xl lg:text-5xl font-bold mb-4 russo-font">
            Lo que dicen <span class="text-primary">nuestros clientes</span>
          </h2>
          <div class="w-24 h-1 bg-primary mx-auto mt-6"></div>
        </AnimateOnScroll>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 md:gap-16 items-center">
        <!-- Testimonials Carousel -->
        <AnimateOnScroll animation="fade-right">
          <div class="relative bg-card/50 dark:bg-card/70 backdrop-blur-sm p-8 rounded-2xl border border-border/50 shadow-lg">
            <div class="testimonial-container">
              <!-- Use TransitionGroup for smoother animations -->
              <TransitionGroup :name="transitionName">
                <div class="testimonial-slide" v-for="(testimonial, index) in testimonials" :key="testimonial.id"
                     v-show="index === currentTestimonialIndex">
                  <div class="flex flex-col space-y-4 relative">
                    <!-- Comillas decorativas -->
                    <div class="text-primary mb-2">
                      <Quote class="size-10 md:size-12 transform -scale-x-100 opacity-20 absolute -top-4 -left-4" />
                      <Quote class="size-10 md:size-12 opacity-20 absolute -bottom-4 -right-4" />
                    </div>

                    <p class="text-lg text-muted-foreground italic leading-relaxed z-10">
                      {{ testimonial.content }}
                    </p>

                    <div class="flex items-center mt-4 z-10">
                      <div class="flex-shrink-0">
                        <img :src="testimonial.avatar" class="h-14 w-14 rounded-full object-cover border-2 border-primary/50"
                             :alt="testimonial.name" />
                      </div>
                      <div class="ml-4">
                        <h4 class="font-bold text-secondary russo-font">{{ testimonial.name }}</h4>
                        <p class="text-sm text-muted-foreground">
                          {{ testimonial.position }}, <span class="text-primary font-medium">{{ testimonial.company }}</span>
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </TransitionGroup>
            </div>

            <!-- Controles de navegación -->
            <div class="flex justify-between items-center mt-8">
              <div class="flex space-x-2">
                <button v-for="(_, index) in testimonials" :key="index"
                        @click="setTestimonial(index)"
                        class="w-3 h-3 rounded-full transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-primary focus:ring-offset-2 focus:ring-offset-background"
                        :class="index === currentTestimonialIndex ? 'bg-primary scale-125' : 'bg-muted hover:bg-primary/50'">
                </button>
              </div>

              <div class="flex space-x-3">
                <button @click="prevTestimonial"
                        class="p-2 rounded-full border border-muted hover:border-primary hover:text-primary hover:bg-primary/10 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-primary focus:ring-offset-2 focus:ring-offset-background group">
                  <ChevronLeft class="size-5 group-hover:-translate-x-1 transition-transform" />
                </button>
                <button @click="nextTestimonial"
                        class="p-2 rounded-full border border-muted hover:border-primary hover:text-primary hover:bg-primary/10 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-primary focus:ring-offset-2 focus:ring-offset-background group">
                  <ChevronRight class="size-5 group-hover:translate-x-1 transition-transform" />
                </button>
              </div>
            </div>
          </div>
        </AnimateOnScroll>

        <!-- Logos de clientes -->
        <AnimateOnScroll animation="fade-left" :delay="200">
          <div class="logos-container">
            <h3 class="text-xl mb-8 font-medium text-secondary russo-font text-center lg:text-left">Empresas que confían en nosotros</h3>
            <div class="grid grid-cols-2 sm:grid-cols-3 gap-6 items-center justify-items-center">
              <div v-for="logo in clientLogos" :key="logo.id"
                   class="bg-card/50 dark:bg-card/70 backdrop-blur-sm p-4 rounded-lg border border-border/30 hover:border-primary/50 hover:shadow-lg hover:shadow-primary/10 transition-all duration-300 w-full h-24 flex items-center justify-center group">
                <img :src="logo.logo" :alt="logo.name" class="max-h-12 max-w-full filter grayscale group-hover:grayscale-0 group-hover:scale-110 transition-all duration-300" />
              </div>
            </div>
          </div>
        </AnimateOnScroll>
      </div>
    </div>
     <!-- Bottom Separator -->
    <div class="absolute bottom-0 left-0 right-0 z-0">
       <svg viewBox="0 0 1440 100" class="w-full h-auto">
         <path fill="hsl(var(--background))" d="M0,64L48,58.7C96,53,192,43,288,48C384,53,480,75,576,85.3C672,96,768,96,864,85.3C960,75,1056,53,1152,48C1248,43,1344,53,1392,58.7L1440,64L1440,101L1392,101C1344,101,1248,101,1152,101C1056,101,960,101,864,101C768,101,672,101,576,101C480,101,384,101,288,101C192,101,96,101,48,101L0,101Z"></path>
       </svg>
    </div>
  </section>
</template>

<style scoped>
.testimonial-container {
  position: relative;
  overflow: hidden;
  min-height: 280px;
}

.testimonial-slide {
  width: 100%;
}

/* Slide Transitions */
.slide-next-enter-active,
.slide-next-leave-active,
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* Slide Next */
.slide-next-enter-from {
  opacity: 0;
  transform: translateX(50px);
}
.slide-next-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}

/* Slide Previous */
.slide-prev-enter-from {
  opacity: 0;
  transform: translateX(-50px);
}
.slide-prev-leave-to {
  opacity: 0;
  transform: translateX(50px);
}

/* Estilo para los logos de clientes */
.logos-container img {
  transition: all 0.3s ease;
}
</style>
