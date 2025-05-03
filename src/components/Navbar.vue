<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { Button } from "@/components/ui/button";
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "@/components/ui/sheet";
import ToggleTheme from "./ToggleTheme.vue";
import { Menu, X } from "lucide-vue-next";
import FacebookIcon from "@/icons/FacebookIcon.vue";
import LinkedInIcon from "@/icons/LinkedInIcon.vue";
import TikTokIcon from "@/icons/TikTokIcon.vue";

const isScrolled = ref(false);
const lastScrollY = ref(0);
const isVisible = ref(true);
const scrollThreshold = 100;
const isMenuOpen = ref(false);
const menuItems = ref([
  { title: "Inicio", href: "#", icon: "home" },
  { title: "Nosotros", href: "#nosotros", icon: "users" },
  { title: "Servicios", href: "#servicios", icon: "wrench" },
  { title: "Proyectos", href: "#proyectos", icon: "briefcase" },
  { title: "Homologaciones", href: "#homologaciones", icon: "award" },
  { title: "Contacto", href: "#contacto", icon: "mail" },
]);

const activeSection = ref("inicio");

// Para el efecto de subrayado animado en hover
const hoverIndex = ref(-1);

const handleScroll = () => {
  const scrollY = window.scrollY;
  
  // Detectar si ha scrolleado más allá del umbral
  isScrolled.value = scrollY > 20;
  
  // Lógica para ocultar/mostrar navbar al scrollear
  if (scrollY > scrollThreshold && scrollY > lastScrollY.value) {
    isVisible.value = false;
  } else {
    isVisible.value = true;
  }
  
  lastScrollY.value = scrollY;
  
  // Detectar sección activa basada en el scroll
  const sections = document.querySelectorAll('section[id]');
  
  sections.forEach(current => {
    const sectionId = current.getAttribute('id');
    const currentElement = current as HTMLElement;
    const sectionHeight = currentElement.offsetHeight;
    const sectionTop = currentElement.offsetTop - 100;
    
    if (scrollY > sectionTop && scrollY <= sectionTop + sectionHeight) {
      activeSection.value = sectionId || 'inicio';
    }
  });
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
  isMenuOpen.value = false;
};

// Acentuar el elemento del menú al hacer click (para dispositivos móviles)
const handleMenuItemClick = () => {
  closeMenu();
};

const scrollToContact = () => {
  if (typeof window !== 'undefined') {
    window.location.href = '#contacto';
  } else {
    console.error('window object is not available.');
  }
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  handleScroll(); // Inicializar estado
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});

// Efecto de entrada con desplazamiento de los elementos del menú
const isLoaded = ref(false);
onMounted(() => {
  setTimeout(() => {
    isLoaded.value = true;
  }, 100);
});
</script>

<template>
  <header
    class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 ease-in-out"
    :class="{
      'py-2 bg-secondary/95 backdrop-blur-md shadow-md': isScrolled,
      'py-4 bg-transparent': !isScrolled,
      'translate-y-0': isVisible,
      '-translate-y-full': !isVisible,
    }"
  >
    <div class="w-full px-6 md:px-10 lg:px-16">
      <div class="flex justify-between items-center">
        <!-- Logo con efecto de resplandor en hover -->
        <a 
          href="#"
          class="flex items-center space-x-2 transition-all duration-300 ease-out hover:scale-105 logo-glow"
        >
          <div class="relative">
            <div class="absolute inset-0 bg-primary/20 rounded-full scale-0 transition-transform duration-500"></div>
            <img 
              src="/matiglas_logo.png" 
              alt="MATIGLAS" 
              class="h-20 w-auto z-10 relative transition-all duration-300" 
              :class="{ 'h-16': isScrolled }"
            />
          </div>
        </a>

        <!-- Navigation para desktop con efectos mejorados -->
        <nav class="hidden md:flex items-center space-x-1 relative">
          <!-- Indicador animado debajo de los items con brillo (solo visible cuando hay hover) -->
          <div 
            class="absolute bottom-0 h-0.5 bg-primary rounded-full transition-all duration-300 ease-in-out glow-effect"
            v-show="hoverIndex !== -1"
          ></div>
          
          <div
            v-for="(item, index) in menuItems"
            :key="index"
            class="relative"
            @mouseenter="hoverIndex = index"
            @mouseleave="hoverIndex = -1"
          >
            <a
              :href="item.href"
              class="px-4 py-2 text-sm rounded-md transition-all duration-300 inline-block relative menu-item overflow-hidden hover:scale-105"
              :class="{
                'text-white': true,
                'font-medium': activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio'),
                'active-item': activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio')
              }"
            >
              {{ item.title }}

              <!-- Indicador activo con animación (solo visible cuando es activo y NO hay hover en ningún elemento) -->
              <span 
                v-if="(activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio')) && hoverIndex === -1"
                class="absolute bottom-0 left-0 w-full h-0.5 bg-primary scale-x-100 transition-transform duration-300 nav-active-indicator"
              ></span>
            </a>
          </div>
        </nav>
        
        <!-- Social icons -->
        <div class="hidden md:flex items-center space-x-4 mr-4">
          <a href="#" class="border-white text-white/80 hover:text-primary transition-colors duration-300 hover:scale-110 transform">
            <FacebookIcon class="h-5 w-5" />
          </a>
          <a href="#" class="hover:text-primary transition-colors duration-300 hover:scale-110 transform">
            <LinkedInIcon class="h-5 w-5" />
          </a>
          <a href="#" class="border-white text-white/80 hover:text-primary transition-colors duration-300 hover:scale-110 transform">
            <TikTokIcon class="h-5 w-5" />
          </a>
        </div>
        
        <!-- Theme toggle and cotizar -->
        <div class="hidden md:flex items-center space-x-3">
          <div class="rotate-hover">
            <ToggleTheme />
          </div>
          
          <Button 
            variant="default" 
            size="sm" 
            class="button-pulse-effect bg-primary text-black hover:bg-primary/90" 
            @mouseenter="hoverIndex = -1"
            @click="scrollToContact"
          >
            <span class="relative z-10">Cotizar</span>
          </Button>
        </div>

        <!-- Mobile menu button con efecto de rotación mejorado -->
        <div class="flex items-center md:hidden space-x-4">
          <div class="rotate-hover">
            <ToggleTheme />
          </div>
          
          <Sheet :open="isMenuOpen" @update:open="isMenuOpen = $event">
            <SheetTrigger as-child>
              <Button 
                variant="ghost" 
                size="icon" 
                @click="toggleMenu" 
                class="relative overflow-hidden button-ripple text-white hover:text-white hover:bg-white/20"
              >
                <Menu v-if="!isMenuOpen" class="h-6 w-6 transition-transform duration-500 ease-elastic hover:rotate-180" />
                <X v-else class="h-6 w-6 transition-transform duration-500 ease-elastic hover:rotate-180" />
              </Button>
            </SheetTrigger>
            <SheetContent side="right" class="w-[85%] sm:w-[385px] bg-secondary text-white">
              <SheetHeader>
                <SheetTitle class="flex items-center space-x-2 text-white">
                  <img 
                    src="/matiglas_logo.png" 
                    alt="MATIGLAS" 
                    class="h-8 w-auto animate-pulse-subtle"
                  />
                  <span>MATIGLAS <span class="text-primary shine-effect">E.I.R.L.</span></span>
                </SheetTitle>
                <SheetDescription class="text-white/70">
                  Soluciones integrales en construcción y mantenimiento
                </SheetDescription>
              </SheetHeader>
              
              <nav class="mt-8 flex flex-col space-y-1">
                <div 
                  v-for="(item, index) in menuItems" 
                  :key="index"
                  class="transform transition-all duration-500"
                  :class="{
                    'translate-x-0 opacity-100': isLoaded,
                    'translate-x-10 opacity-0': !isLoaded,
                  }"
                  :style="{ transitionDelay: `${index * 50}ms` }"
                >
                  <a
                    :href="item.href"
                    class="flex items-center p-3 rounded-md hover:bg-secondary-foreground/10 hover:scale-[1.02] transition-all duration-200 group"
                    :class="{
                      'bg-secondary-foreground/20': activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio')
                    }"
                    @click="handleMenuItemClick"
                  >
                    <div class="w-10 h-10 flex items-center justify-center rounded-full bg-primary/10 text-primary mr-3 
                      group-hover:bg-primary/20 transition-all duration-300 group-hover:scale-110 group-hover:rotate-6">
                      <component :is="item.icon" class="w-5 h-5 transition-transform duration-300 group-hover:scale-110" />
                    </div>
                    <span class="font-medium text-white">{{ item.title }}</span>
                    <div 
                      class="ml-auto w-1.5 h-1.5 rounded-full transition-all duration-300"
                      :class="{
                        'bg-primary scale-100': activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio'),
                        'bg-transparent scale-0': !(activeSection === item.href.slice(1) || (item.href === '#' && activeSection === 'inicio'))
                      }"
                    ></div>
                  </a>
                </div>
                
                <div 
                  class="mt-6 transform transition-all duration-500"
                  :class="{
                    'translate-x-0 opacity-100': isLoaded,
                    'translate-x-10 opacity-0': !isLoaded,
                  }"
                  :style="{ transitionDelay: `${menuItems.length * 50 + 50}ms` }"
                >
                  <Button 
                    variant="default" 
                    class="w-full button-pulse-effect bg-primary text-black hover:bg-primary/90"
                    @click="() => { closeMenu(); scrollToContact(); }"
                  >
                    <span class="relative z-10">Cotizar ahora</span>
                  </Button>
                </div>
              </nav>
            </SheetContent>
          </Sheet>
        </div>
      </div>
    </div>
  </header>
</template>

<style scoped>
/* Animaciones y efectos mejorados */
.menu-item::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 0.15rem;
  background-color: #FFCC01; /* Amarillo corporativo */
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}
/* Solo mostrar el efecto de hover en elementos que no están activos */
.menu-item:not(.active-item):hover::before,
.active-item:hover::before {
  transform: scaleX(1);
  transform-origin: left;
}

/* Cuando un elemento está activo y también tiene hover, ocultar el indicador activo y mostrar solo el hover */
.active-item.hover::before {
  transform: scaleX(1);
}

/* Efecto de resplandor para el logo */
.logo-glow:hover .logo-circle {
  transform: scale(1.5);
  opacity: 0.6;
}

.logo-circle {
  transform-origin: center;
  transition: transform 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275), opacity 0.5s ease;
  opacity: 0;
}

/* Efecto de brillo en texto */
.shine-effect {
  position: relative;
  overflow: hidden;
}

.shine-effect::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 50%;
  height: 100%;
  background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.3), transparent);
  transform: skewX(-25deg);
  animation: shine 3s infinite ease-in-out;
}

@keyframes shine {
  0%, 100% {
    left: -100%;
  }
  50% {
    left: 100%;
  }
}

/* Efecto de pulso para botones */
.button-pulse-effect {
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.button-pulse-effect:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(255, 204, 1, 0.4); /* Amarillo corporativo */
}

.button-pulse-effect::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: radial-gradient(circle at center, rgba(255, 204, 1, 0.2), transparent 70%); /* Amarillo corporativo */
  transform: scale(0);
  transition: transform 0.5s cubic-bezier(0.19, 1, 0.22, 1);
}

.button-pulse-effect:hover::before {
  transform: scale(2);
}

/* Efecto de rotación al hacer hover */
.rotate-hover {
  transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.rotate-hover:hover {
  transform: rotate(10deg) scale(1.1);
}

/* Efecto elástico personalizado */
.ease-elastic {
  transition-timing-function: cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

/* Efecto de ondulación para botones */
.button-ripple::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 5px;
  height: 5px;
  background: rgba(255, 255, 255, 0.5);
  opacity: 0;
  border-radius: 100%;
  transform: scale(1) translate(-50%, -50%);
  transform-origin: center;
}

.button-ripple:active::after {
  animation: ripple 0.6s linear;
}

@keyframes ripple {
  0% {
    transform: scale(0) translate(-50%, -50%);
    opacity: 0.5;
  }
  100% {
    transform: scale(20) translate(-50%, -50%);
    opacity: 0;
  }
}

/* Indicador activo con animación de pulso */
.nav-active-indicator {
  animation: pulse-subtle 2s infinite;
}

@keyframes pulse-subtle {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.7;
  }
}

/* Animación sutil para logos */
.animate-pulse-subtle {
  animation: pulse-subtle 3s infinite;
}

/* Efecto de resplandor para elementos destacados */
.glow-effect {
  box-shadow: 0 0 8px rgba(255, 204, 1, 0.6); /* Amarillo corporativo */
}
</style>
