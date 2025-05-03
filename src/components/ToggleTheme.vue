<script setup lang="ts">
import { useColorMode } from "@vueuse/core";
import { ref, computed, watch } from 'vue';
const mode = useColorMode();
import { Moon, Sun } from "lucide-vue-next";

// Estado para la animación
const isAnimating = ref(false);

// Controlar la dirección de la animación basada en el modo
const animationClass = computed(() => {
  return mode.value === 'dark' ? 'rotate-in' : 'rotate-out';
});

// Observar los cambios de modo para activar la animación
watch(mode, () => {
  isAnimating.value = true;
  setTimeout(() => {
    isAnimating.value = false;
  }, 600); // Duración de la animación
});
</script>

<template>
  <button
    @click="mode = mode === 'dark' ? 'light' : 'dark'"
    class="theme-toggle-button"
    aria-label="Cambiar tema"
  >
    <div class="toggle-container">
      <!-- Iconos con transición suave -->
      <div 
        class="icon-wrapper" 
        :class="{ 'animate': isAnimating, [animationClass]: isAnimating }"
      >
        <Moon 
          v-if="mode === 'light'"
          class="toggle-icon"
        />
        <Sun 
          v-else
          class="toggle-icon sun-icon-glow"
        />
      </div>
      
      <!-- Indicador visual del estado (opcional, solo para pantallas más grandes) -->
      <div class="mode-indicator">
        <span class="mode-text">{{ mode === 'dark' ? 'Claro' : 'Oscuro' }}</span>
      </div>
    </div>
    <span class="sr-only">Cambiar tema</span>
  </button>
</template>

<style scoped>
.theme-toggle-button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6px 10px; /* Ajuste de padding */
  border-radius: 20px; /* Más redondeado */
  border: none;
  outline: none;
  cursor: pointer;
  background: transparent; /* Fondo transparente por defecto */
  color: theme('colors.white / 80%'); /* Color base blanco semi-transparente, como otros iconos */
  transition: all 0.3s ease;
}

.theme-toggle-button:hover {
  color: theme('colors.primary.DEFAULT'); /* Amarillo en hover */
  background-color: theme('colors.white / 10%'); /* Fondo muy sutil en hover */
  transform: scale(1.1); /* Efecto de escala en hover */
}

.toggle-container {
  display: flex;
  align-items: center;
  gap: 6px; /* Espacio reducido */
}

.icon-wrapper {
  position: relative;
  width: 20px; /* Tamaño ajustado */
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.toggle-icon {
  width: 18px; /* Tamaño del icono */
  height: 18px;
}

/* Efecto de brillo solo para el sol (modo oscuro) */
.sun-icon-glow {
  filter: drop-shadow(0 0 3px theme('colors.primary.DEFAULT / 60%'));
}

.mode-indicator {
  display: none; /* Oculto por defecto */
}

/* Mostrar texto en pantallas medianas y grandes */
@media (min-width: 768px) { /* md breakpoint */
  .mode-indicator {
    display: block;
    font-size: 0.8rem; /* Tamaño de fuente ajustado */
    font-weight: 500;
    line-height: 1;
  }
}

/* Animaciones para los íconos */
.animate {
  animation-duration: 0.6s;
  animation-fill-mode: forwards;
}

.rotate-in {
  animation-name: rotateIn;
}

.rotate-out {
  animation-name: rotateOut;
}

@keyframes rotateIn {
  0% {
    transform: rotate(-90deg) scale(0.5);
    opacity: 0;
  }
  100% {
    transform: rotate(0deg) scale(1);
    opacity: 1;
  }
}

@keyframes rotateOut {
  0% {
    transform: rotate(90deg) scale(0.5);
    opacity: 0;
  }
  100% {
    transform: rotate(0deg) scale(1);
    opacity: 1;
  }
}
</style>
