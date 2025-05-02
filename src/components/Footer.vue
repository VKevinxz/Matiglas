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
              <div class="bg-primary rounded-lg w-9 h-9 mr-2 border flex items-center justify-center text-secondary font-bold">
                M
              </div>

              <h3 class="text-2xl">MATIGLAS E.I.R.L.</h3>
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
                class="opacity-60 hover:opacity-100"
              >
                Inicio
              </a>
            </div>

            <div>
              <a
                href="#nosotros"
                class="opacity-60 hover:opacity-100"
              >
                Nosotros
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100"
              >
                Servicios
              </a>
            </div>

            <div>
              <a
                href="#contacto"
                class="opacity-60 hover:opacity-100"
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
                class="opacity-60 hover:opacity-100"
              >
                Pintura en general
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100"
              >
                Sistemas eléctricos
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100"
              >
                Albañilería
              </a>
            </div>

            <div>
              <a
                href="#servicios"
                class="opacity-60 hover:opacity-100"
              >
                Sistema Drywall
              </a>
            </div>
          </div>

          <div class="flex flex-col gap-3">
            <h3 class="font-bold text-lg">Contacto</h3>
            
            <div class="flex items-center gap-2">
              <Phone class="size-5" />
              <span class="opacity-80">972960526 / 947258074</span>
            </div>

            <div class="flex items-center gap-2">
              <Mail class="size-5" />
              <span class="opacity-80">matiglas.2014@gmail.com</span>
            </div>

            <div class="flex items-center gap-2">
              <Building2 class="size-5" />
              <span class="opacity-80">Mza. Q Lote. 13 Urb. Los Jazmines Etp.4</span>
            </div>
            
            <div class="flex items-center gap-2">
              <Globe class="size-5" />
              <span class="opacity-80">www.serviciosmatiglas.com</span>
            </div>
          </div>
        </div>

        <Separator class="my-6 bg-white/20" />
        <section className="text-center">
          <h3 class="opacity-80">
            &copy; {{ new Date().getFullYear() }} MATIGLAS E.I.R.L. - Todos los derechos reservados
          </h3>
        </section>
      </div>
    </AnimateOnScroll>
  </footer>
</template>
