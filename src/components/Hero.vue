<script setup lang="ts">
import { ref, onMounted, computed, watch, onUnmounted } from 'vue';
import { Button } from "@/components/ui/button";
import { useColorMode } from "@vueuse/core";
import { ChevronRight, ChevronDownIcon, Phone, Mail } from "lucide-vue-next";
import AnimateOnScroll from './AnimateOnScroll.vue';

// Usamos directamente useColorMode en lugar de importar useTheme
const mode = useColorMode();

// Para el efecto parallax
const heroSection = ref<HTMLElement | null>(null);
const parallaxOffset = ref(0);
const mouseX = ref(0);
const mouseY = ref(0);
const isMobile = ref(window.innerWidth < 768);

// Configuración de efecto Parallax
const parallaxOffsetNew = ref({ x: 0, y: 0 });
const parallaxIntensity = computed(() => isMobile.value ? 5 : 20); // Reduce la intensidad en móviles

const handleMouseMove = (e: MouseEvent) => {
  // Reducimos el movimiento en dispositivos móviles
  const intensityFactor = isMobile.value ? 0.5 : 1;
  
  mouseX.value = (e.clientX / window.innerWidth - 0.5) * 20 * intensityFactor;
  mouseY.value = (e.clientY / window.innerHeight - 0.5) * 20 * intensityFactor;

  // Calculamos la posición relativa del mouse en la pantalla (valores entre -1 y 1)
  const mouseXNew = (e.clientX / window.innerWidth) * 2 - 1;
  const mouseYNew = (e.clientY / window.innerHeight) * 2 - 1;
  
  // Aplicamos la intensidad del parallax (negativo para efecto "natural")
  parallaxOffsetNew.value = {
    x: -mouseXNew * parallaxIntensity.value,
    y: -mouseYNew * parallaxIntensity.value
  };
};

// Verificar si el dispositivo es móvil al cambiar el tamaño de ventana
const handleResize = () => {
  isMobile.value = window.innerWidth < 768;
};

// Imagen de hero basada en el modo actual
const heroImage = computed(() => {
  return mode.value === 'light' ? '/matiglas_ing.jpg' : '/matiglas_ing.jpg';
});

// Para el efecto de texto flotante en el slogan
const isVisible = ref(false);
const words = ref(['Construcción', 'Mantenimiento', 'Remodelación']);
const currentWordIndex = ref(0);
const currentCharIndex = ref(0);
const isDeleting = ref(false);
const typingText = ref('');

const handleScroll = () => {
  if (!heroSection.value) return;
  const scrollY = window.scrollY;
  
  // Verificar que heroSection.value no es null antes de llamar a getBoundingClientRect
  const heroRect = heroSection.value ? heroSection.value.getBoundingClientRect() : { top: 0, bottom: 0 };
  
  // Solo aplicar parallax si la sección es visible
  if (heroRect.bottom > 0 && heroRect.top < window.innerHeight) {
    // Reducir el parallax en dispositivos móviles
    const scrollFactor = isMobile.value ? 0.1 : 0.3;
    parallaxOffset.value = scrollY * scrollFactor;
  }
};

// Función para animar el texto del slogan
const typeEffect = () => {
  const currentWord = words.value[currentWordIndex.value];
  
  // Si está borrando, elimina un caracter
  if (isDeleting.value) {
    typingText.value = currentWord.substring(0, currentCharIndex.value - 1);
    currentCharIndex.value--;
    
    // Si ya borró toda la palabra
    if (currentCharIndex.value === 0) {
      isDeleting.value = false;
      currentWordIndex.value = (currentWordIndex.value + 1) % words.value.length;
    }
  } 
  // Si está escribiendo, agrega un caracter
  else {
    typingText.value = currentWord.substring(0, currentCharIndex.value + 1);
    currentCharIndex.value++;
    
    // Si completó la palabra actual
    if (currentCharIndex.value === currentWord.length) {
      // Pausa antes de comenzar a borrar
      setTimeout(() => {
        isDeleting.value = true;
      }, 1500);
      return;
    }
  }
  
  // Velocidad variable: más rápido al borrar, más lento al escribir
  const typingSpeed = isDeleting.value ? 80 : 150;
  setTimeout(typeEffect, typingSpeed);
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  window.addEventListener('mousemove', handleMouseMove);
  window.addEventListener('resize', handleResize);
  
  // Iniciar con el valor correcto de isMobile
  handleResize();
  
  // Iniciar la animación de typing
  setTimeout(() => {
    isVisible.value = true;
    typeEffect();
  }, 500);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  window.removeEventListener('mousemove', handleMouseMove);
  window.removeEventListener('resize', handleResize);
});

// Actualizar el efecto de parallax cuando cambia el tema
watch(mode, () => {
  const scrollFactor = isMobile.value ? 0.1 : 0.2;
  parallaxOffset.value = window.scrollY * scrollFactor;
});
</script>

<template>
  <section 
    ref="heroSection" 
    class="relative flex flex-col justify-center items-center min-h-screen overflow-hidden w-full dots-background-light"
    :style="{
      '--mouse-x': `${mouseX}px`,
      '--mouse-y': `${mouseY}px`
    }"
  >
    <!-- Fondo con efecto parallax -->
    <div 
      class="absolute inset-0 bg-cover bg-center sm:bg-center bg-no-repeat z-0 transform"
      :style="{
        backgroundImage: `url(${heroImage})`,
        transform: `translateY(${parallaxOffset}px) scale(1.1)`,
        backgroundPosition: `calc(50% + ${mouseX * 0.02}px) calc(50% + ${mouseY * 0.02}px)`,
        backgroundSize: 'cover',
        objectFit: 'cover',
        width: '100vw'
      }"
    >
      <!-- Contenedor interno para mejorar la responsividad en móviles -->
      <div class="absolute inset-0 md:hidden" style="background-size: cover; background-position: center center;"
           :style="{ backgroundImage: `url(${heroImage})` }"></div>
    </div>

    <!-- Overlay con gradiente -->
    <div class="absolute inset-0 bg-gradient-to-b from-secondary/70 via-secondary/60 to-background z-10 w-full"></div>
    
    <!-- Rectángulo amarillo vertical al estilo Bauhaus -->
    <div class="absolute left-0 md:left-0 lg:left-16 top-1/2 transform -translate-y-1/2 h-3/4 w-6 md:w-12 lg:w-16 bg-primary z-20"></div>
    
    <!-- Contenido principal con estructura tipo Bauhaus -->
    <div class="w-full max-w-[2000px] px-4 md:px-8 lg:px-12 mx-auto relative z-20">
      <div class="grid grid-cols-1 md:grid-cols-12 gap-6 items-center">
        <!-- Columna izquierda con rectángulo amarillo y "CONSTRUCCIÓN" -->
        <div class="md:col-span-2 hidden md:flex flex-col items-start">
          <div class="bg-primary p-3 md:p-5 lg:p-6">
            <p class="text-black font-medium transform -rotate-90 origin-center whitespace-nowrap text-xs md:text-sm lg:text-base russo-font">SERVICIOS</p>
          </div>
        </div>
        
        <!-- Contenido principal -->
        <div class="md:col-span-10 flex flex-col items-center md:items-start text-center md:text-left transform transition-transform duration-700"
          :class="{ 'translate-y-0 opacity-100': isVisible, 'translate-y-10 opacity-0': !isVisible }">
          
          <div class="w-full mb-6">
            <div class="inline-block bg-transparent">
              <h1 class="text-4xl md:text-6xl lg:text-8xl font-bold text-white tracking-tighter mb-4 russo-font">
                MATIGLAS<span class="text-primary">.</span>
              </h1>
              <div class="h-2 w-1/3 md:w-1/2 bg-primary mt-2"></div>
            </div>
            <h2 class="text-2xl md:text-4xl lg:text-5xl text-white mt-4 font-bold max-w-3xl russo-font">
              Soluciones en <span class="text-primary font-semibold">{{ typingText }}</span><span class="typing-cursor">|</span>
            </h2>
            <p class="text-white/80 mt-6 max-w-2xl">
              Somos una empresa peruana con más de 10 años de experiencia en el sector, brindando servicios de calidad y garantía en todo Lima Metropolitana.
            </p>
          </div>

          <div class="flex flex-col sm:flex-row gap-4 mt-6 w-full max-w-md">
            <Button variant="default" size="lg" class="group btn-hover-effect w-full sm:w-auto relative overflow-hidden bg-primary text-black hover:bg-primary/90">
              <span class="relative z-10 russo-font">Ver proyectos</span>
              <ChevronRight class="ml-2 size-4 group-hover:translate-x-1 transition-transform duration-300" />
              <div class="btn-glow"></div>
            </Button>
            
            <a href="#contacto" class="w-full sm:w-auto">
              <Button variant="outline" size="lg" class="bg-background/20 backdrop-blur-md border-white/20 text-white hover:text-white hover:bg-white/10 transition-all duration-300 group w-full">
                <span class="russo-font">Contáctanos</span>
                <ChevronRight class="ml-2 size-4 group-hover:translate-x-1 group-hover:rotate-90 transition-all duration-300" />
              </Button>
            </a>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Ondulación inferior -->
    <div class="absolute bottom-0 left-0 right-0 z-10 w-full">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 1440 220"
        class="w-full h-auto"
      >
        <path
          class="fill-background"
          d="M0,160L48,144C96,128,192,96,288,90.7C384,85,480,107,576,128C672,149,768,171,864,165.3C960,160,1056,128,1152,112C1248,96,1344,96,1392,96L1440,96L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"
        ></path>
      </svg>
    </div>
  </section>

  <section
    id="inicio"
    class="w-full py-24 sm:py-32 min-h-[92vh] md:h-[92vh] flex flex-col justify-center overflow-hidden dots-background"
  >
    <div class="max-w-[2000px] px-4 md:px-8 lg:px-12 mx-auto">
      <div class="grid lg:grid-cols-2 place-items-center gap-8 lg:pt-8">
        <AnimateOnScroll>
          <div class="flex flex-col items-start space-y-4 z-10">
            <div class="inline-block bg-primary px-3 py-1 text-lg relative">
              <span class="text-black russo-font">MATIGLAS E.I.R.L.</span>
            </div>

            <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold text-secondary russo-font">
              <span class="relative inline-block">
                <span class="accent-text">Servicios</span>
                <span class="accent-underline"></span>
              </span>
              <span class="relative z-10">generales de</span><br />
              <span class="relative z-10">construcción</span>
            </h1>

            <p class="text-muted-foreground text-lg md:text-xl lg:pr-10">
              Especialistas en servicios de mantenimiento y construcción para edificios residenciales 
              y comerciales con más de 10 años de experiencia.
            </p>

            <div class="flex flex-col sm:flex-row gap-4 pt-4">
              <Button variant="default" class="hero-btn-primary bg-primary text-black hover:bg-primary/90">
                <Phone class="mr-2 h-4 w-4" />
                <span class="russo-font">Contáctanos</span>
                <div class="btn-glow"></div>
              </Button>
              <Button variant="outline" class="hero-btn-secondary">
                <Mail class="mr-2 h-4 w-4" />
                <span class="russo-font">Solicitar presupuesto</span>
              </Button>
            </div>

            <div class="flex items-center pt-6 text-muted-foreground">
              <div class="flex -space-x-2">
                <div class="client-avatar">MA</div>
                <div class="client-avatar">CV</div>
                <div class="client-avatar">BS</div>
                <div class="client-avatar">+</div>
              </div>
              <div class="ml-3 text-sm">
                <span class="font-medium">Más de 200 clientes satisfechos</span>
              </div>
            </div>
          </div>
        </AnimateOnScroll>

        <AnimateOnScroll :delay="100" :distance="80" direction="left">
          <div 
            class="relative w-full h-[400px] md:h-[450px] lg:h-[500px] parallax-container"
          >
            <!-- Fondo con efecto parallax -->
            <div 
              class="absolute top-0 right-0 bottom-0 left-0 bg-gradient-to-br from-primary/10 to-secondary/10 rounded-2xl transform-gpu"
              :style="{ transform: `translate(${-parallaxOffsetNew.x * 0.5}px, ${-parallaxOffsetNew.y * 0.5}px)` }"
            ></div>
            
            <!-- Elemento decorativo (círculo) -->
            <div 
              class="circle-element circle-1"
              :style="{ transform: `translate(${parallaxOffsetNew.x * 2}px, ${parallaxOffsetNew.y * 2}px)` }"
            ></div>
            <div 
              class="circle-element circle-2"
              :style="{ transform: `translate(${-parallaxOffsetNew.x * 1.5}px, ${-parallaxOffsetNew.y * 1.5}px)` }"
            ></div>
            
            <!-- Imagen principal con efecto parallax más pronunciado -->
            <img
              src="/hero-image-light.jpg"
              alt="Servicios de construcción"
              class="absolute object-cover rounded-2xl shadow-lg transform-gpu img-hero dark:hidden"
              :style="{ transform: `translate(${parallaxOffsetNew.x}px, ${parallaxOffsetNew.y}px) scale(1.05)` }"
            />
            <img
              src="/hero-image-dark.jpg"
              alt="Servicios de construcción"
              class="absolute object-cover rounded-2xl shadow-lg transform-gpu img-hero hidden dark:block"
              :style="{ transform: `translate(${parallaxOffsetNew.x}px, ${parallaxOffsetNew.y}px) scale(1.05)` }"
            />
            
            <!-- Capa de efecto hover -->
            <div class="hover-effect"></div>
          </div>
        </AnimateOnScroll>
      </div>
    </div>
    
    <!-- Scroll indicator animado -->
    <div class="scroll-indicator">
      <ChevronDownIcon class="animate-bounce h-6 w-6 text-primary" />
    </div>
  </section>
</template>

<style scoped>
.hero-title {
  text-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
  letter-spacing: 1px;
}

.hero-subtitle {
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.shadow-glow {
  text-shadow: 0 0 10px rgba(255, 204, 1, 0.5), 0 4px 8px rgba(0, 0, 0, 0.5); /* Amarillo corporativo */
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}

.animate-blob {
  animation: blob-bounce 10s infinite ease;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

/* Mejoras responsive para imagen de fondo */
@media (max-width: 768px) {
  section[ref="heroSection"] {
    background-position: center center;
  }
  
  section[ref="heroSection"] > div:first-child {
    background-position: center center !important;
    background-size: cover !important;
    transform: scale(1.05) !important;
  }
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

@keyframes float {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0);
  }
}

.typing-cursor {
  display: inline-block;
  opacity: 1;
  animation: blink 0.7s infinite;
  font-weight: bold;
  color: #FFCC01; /* Amarillo corporativo */
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}

.accent-text {
  color: #FFCC01; /* Amarillo corporativo */
  position: relative;
  z-index: 10;
  display: inline-block;
  font-family: 'Russo One', sans-serif;
}

.accent-underline {
  position: absolute;
  bottom: 3px;
  left: 0;
  width: 100%;
  height: 0.35em;
  background-color: rgba(255, 204, 1, 0.3); /* Amarillo corporativo */
  z-index: 1;
  transform: rotate(-1deg) skewX(-15deg);
}

.client-avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: #FFCC01; /* Amarillo corporativo */
  color: #000000; /* Negro */
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: 600;
  border: 2px solid hsl(var(--background));
  transition: all 0.3s ease;
}

.client-avatar:hover {
  transform: translateY(-3px) scale(1.15);
  z-index: 10;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
}

/* Estilos para efecto parallax */
.parallax-container {
  perspective: 1000px;
  overflow: hidden;
  border-radius: 1rem;
}

.img-hero {
  inset: 0;
  width: 100%;
  height: 100%;
  transition: transform 0.1s ease-out;
}

.hover-effect {
  position: absolute;
  inset: 0;
  border-radius: 1rem;
  background: linear-gradient(45deg, rgba(255, 204, 1, 0.2), rgba(4, 56, 86, 0.2)); /* Amarillo y azul corporativos */
  opacity: 0;
  transition: opacity 0.3s ease;
}

.parallax-container:hover .hover-effect {
  opacity: 1;
}

.circle-element {
  position: absolute;
  border-radius: 50%;
  transition: transform 0.1s ease-out;
}

.circle-1 {
  width: 120px;
  height: 120px;
  background-color: rgba(255, 204, 1, 0.15); /* Amarillo corporativo */
  top: -30px;
  right: 40px;
}

.circle-2 {
  width: 80px;
  height: 80px;
  background-color: rgba(4, 56, 86, 0.15); /* Azul corporativo */
  bottom: 20px;
  left: -20px;
}

/* Botones con efectos */
.hero-btn-primary {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.hero-btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 7px 14px rgba(255, 204, 1, 0.3); /* Amarillo corporativo */
}

.btn-glow {
  position: absolute;
  top: 0;
  left: -50%;
  width: 50%;
  height: 100%;
  background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.25), transparent);
  transform: skewX(-25deg);
  animation: btn-shine 3s infinite;
}

@keyframes btn-shine {
  0%, 100% {
    left: -50%;
  }
  50% {
    left: 150%;
  }
}

.hero-btn-secondary {
  transition: all 0.3s ease;
}

.hero-btn-secondary:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
  background-color: hsl(var(--background));
  border-color: #FFCC01; /* Amarillo corporativo */
}

/* Indicador de scroll */
.scroll-indicator {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  opacity: 0.8;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.scroll-indicator:hover {
  opacity: 1;
  transform: translateX(-50%) scale(1.2);
}
</style>
