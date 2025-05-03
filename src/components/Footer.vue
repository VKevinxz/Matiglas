<script setup lang="ts">
import Separator from "./ui/separator/Separator.vue";
import AnimateOnScroll from "./AnimateOnScroll.vue";
import { 
  Phone, 
  Mail, 
  Building2, 
  Globe
} from "lucide-vue-next";
</script>

<template>
  <footer
    id="footer"
    class="container py-24 pb-16 sm:py-32 sm:pb-24"
  >
    <AnimateOnScroll>
      <div class="p-10 bg-secondary text-white rounded-2xl">
        <div
          class="grid grid-cols-2 md:grid-cols-4 xl:grid-cols-6 gap-x-12 gap-y-8"
        >
          <div class="col-span-full xl:col-span-2">
            <a
              href="#"
              class="flex font-bold items-center"
            >
              <img 
                src="/matiglas_logo.png" 
                alt="MATIGLAS" 
                class="h-10 w-auto mr-2"
              />
              <h3 class="text-2xl">MATIGLAS <span class="text-primary">E.I.R.L.</span></h3>
            </a>
            <p class="mt-4 text-white/80">
              Empresa peruana especializada en servicios generales de construcción, mantenimiento y remodelación para
              proyectos residenciales y comerciales.
            </p>
          </div>

          <div class="flex flex-col gap-2">
            <h3 class="font-bold text-lg">Enlaces Rápidos</h3>
            <div>
              <a
                href="#inicio"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Inicio
              </a>
            </div>

            <div>
              <a
                href="#nosotros"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Nosotros
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Servicios
              </a>
            </div>

            <div>
              <a
                href="#contacto"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Contacto
              </a>
            </div>
          </div>

          <div class="flex flex-col gap-2">
            <h3 class="font-bold text-lg">Servicios</h3>
            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Pintura en general
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Sistemas eléctricos
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Albañilería
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100 hover:text-primary transition-colors duration-300"
              >
                Sistema Drywall
              </a>
            </div>
          </div>

          <div class="flex flex-col gap-3">
            <h3 class="font-bold text-lg">Contacto</h3>
            
            <div class="flex items-center gap-2">
              <Phone class="size-5 text-primary" />
              <span class="opacity-80 hover:opacity-100 transition-opacity duration-200">972960526 / 947258074</span>
            </div>

            <div class="flex items-center gap-2">
              <Mail class="size-5 text-primary" />
              <span class="opacity-80 hover:opacity-100 transition-opacity duration-200">matiglas.2014@gmail.com</span>
            </div>

            <div class="flex items-center gap-2">
              <Building2 class="size-5 text-primary" />
              <span class="opacity-80 hover:opacity-100 transition-opacity duration-200">Mza. Q Lote. 13 Urb. Los Jazmines Etp.4</span>
            </div>
            
            <div class="flex items-center gap-2">
              <Globe class="size-5 text-primary" />
              <span class="opacity-80 hover:opacity-100 transition-opacity duration-200">www.serviciosmatiglas.com</span>
            </div>
          </div>
        </div>

        <Separator class="my-6 bg-white/20" />
        <section class="text-center">
          <h3 class="opacity-80">
            &copy; {{ new Date().getFullYear() }} MATIGLAS <span class="text-primary">E.I.R.L.</span> - Todos los derechos reservados
          </h3>
        </section>
      </div>
    </AnimateOnScroll>
  </footer>
</template>
