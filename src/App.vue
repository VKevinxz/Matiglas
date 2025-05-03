<script setup lang="ts">
import Navbar from "./components/Navbar.vue";
import Hero from "./components/Hero.vue";
import Benefits from "./components/Benefits.vue";
import Services from "./components/Services.vue";
import RecentProjects from "./components/RecentProjects.vue";
import Homologaciones from "./components/Homologaciones.vue";
/* import Testimonials from "./components/Testimonials.vue"; */
/* import Community from "./components/Community.vue"; */
import Contact from "./components/Contact.vue";
import Footer from "./components/Footer.vue";
</script>

<template>
  <div class="full-width-layout">
    <Navbar />
    <Hero />
    <Benefits />
    <Services />
    <RecentProjects />
    <Homologaciones />
<!--     <Testimonials /> -->
<!--     <Community /> -->
    <Contact />
    <Footer />
  </div>
</template>

<style>
.full-width-layout {
  width: 100vw;
  max-width: 100%;
  overflow-x: hidden;
}

/* Aseguramos que los contenedores internos puedan expandirse a todo el ancho */
.full-width-layout section {
  width: 100%;
  margin: 0;
  padding-left: 0;
  padding-right: 0;
}
</style>
