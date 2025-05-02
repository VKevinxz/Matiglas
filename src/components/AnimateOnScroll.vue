<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';

interface Props {
  /** Retraso adicional en ms (útil para animaciones escalonadas) */
  delay?: number;
  /** Distancia de desplazamiento en píxeles (valor positivo para subir desde abajo) */
  distance?: number;
  /** Duración de la animación en ms */
  duration?: number;
  /** Margen de activación (cuándo debe comenzar la animación al ver el elemento) */
  threshold?: number;
  /** Dirección de la animación: 'up', 'down', 'left', 'right' */
  direction?: 'up' | 'down' | 'left' | 'right';
  /** Habilitar el efecto de salida al scrollear hacia arriba */
  enableExitAnimation?: boolean;
  /** Activar la animación solo una vez (sin salida) */
  once?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  delay: 0,
  distance: 50,
  duration: 800,
  threshold: 0.1,
  direction: 'up',
  enableExitAnimation: true,
  once: false,
});

const element = ref<HTMLElement | null>(null);
const isVisible = ref(false);
const hasAnimatedIn = ref(false);

const getTransform = () => {
  switch (props.direction) {
    case 'up': return `translateY(${props.distance}px)`;
    case 'down': return `translateY(-${props.distance}px)`;
    case 'left': return `translateX(${props.distance}px)`;
    case 'right': return `translateX(-${props.distance}px)`;
    default: return `translateY(${props.distance}px)`;
  }
};

const checkVisibility = () => {
  if (!element.value) return;
  
  const rect = element.value.getBoundingClientRect();
  const windowHeight = window.innerHeight || document.documentElement.clientHeight;
  
  // Verifica si el elemento está en el viewport
  const elementTop = rect.top;
  const elementBottom = rect.bottom;
  
  // El elemento es visible cuando una parte está en pantalla
  const isElementInView = 
    (elementTop <= windowHeight * (1 - props.threshold)) && 
    (elementBottom >= windowHeight * props.threshold);
  
  // Si está configurado para animarse solo una vez y ya se ha animado, no hacer nada más
  if (props.once && hasAnimatedIn.value) {
    return;
  }
  
  // Actualizar el estado de visibilidad
  if (isElementInView) {
    isVisible.value = true;
    hasAnimatedIn.value = true;
  } else if (props.enableExitAnimation) {
    isVisible.value = false;
  }
};

onMounted(() => {
  checkVisibility();
  window.addEventListener('scroll', checkVisibility);
});

onBeforeUnmount(() => {
  window.removeEventListener('scroll', checkVisibility);
});
</script>

<template>
  <div 
    ref="element" 
    :style="{
      opacity: isVisible ? 1 : 0,
      transform: isVisible ? 'translate(0)' : getTransform(),
      transition: `opacity ${props.duration}ms ease-out, transform ${props.duration}ms ease-out`,
      transitionDelay: isVisible ? `${props.delay}ms` : '0ms',
    }"
  >
    <slot></slot>
  </div>
</template>