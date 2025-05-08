<script setup lang="ts">
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Card, CardContent } from "@/components/ui/card";
import { ref, onMounted, onUnmounted } from 'vue';
import AnimateOnScroll from "./AnimateOnScroll.vue";
import { Alert, AlertDescription } from "@/components/ui/alert";
import { 
  MapPin, 
  Phone, 
  Mail, 
  Send,
  CheckCircle2,
  Loader2,
  Facebook,
  Linkedin
} from "lucide-vue-next";
import TikTokIcon from "@/icons/TikTokIcon.vue";
import emailjs from '@emailjs/browser';

const focusedField = ref<string | null>(null);
const formSubmitted = ref(false);
const isSubmitting = ref(false);

const formData = ref({
  nombre: '',
  email: '',
  telefono: '',
  mensaje: ''
});

const errorMessage = ref('');

const setFocusedField = (fieldName: string) => {
  focusedField.value = fieldName;
};

const clearFocusedField = () => {
  focusedField.value = null;
};

const handleSubmit = () => {
  isSubmitting.value = true;
  errorMessage.value = '';
  
  const templateParams = {
    from_name: formData.value.nombre,
    name: formData.value.nombre,
    nombre: formData.value.nombre,
    user_name: formData.value.nombre,
    from_email: formData.value.email,
    email: formData.value.email,
    reply_to: formData.value.email,
    telefono: formData.value.telefono,
    phone: formData.value.telefono,
    mensaje: formData.value.mensaje,
    message: formData.value.mensaje,
    to_name: 'Matiglas',
    subject: 'Nuevo mensaje de contacto desde el sitio web'
  };
  
  console.log('Enviando datos:', templateParams);
  
  emailjs.init("tGGlGcrL9nXegkfnF");
  
  emailjs.send('service_3bn8uov', 'template_auvip4f', templateParams)
    .then((response: any) => {
      console.log('Email enviado!', response.status, response.text);
      isSubmitting.value = false;
      formSubmitted.value = true;
      
      formData.value = {
        nombre: '',
        email: '',
        telefono: '',
        mensaje: ''
      };
      
      setTimeout(() => {
        formSubmitted.value = false;
      }, 3000);
    })
    .catch((error: any) => {
      console.error('Error al enviar el email:', error);
      isSubmitting.value = false;
      errorMessage.value = 'Ocurrió un error al enviar el mensaje. Por favor intente nuevamente.';
    });
};

const socialLinks = [
  { 
    name: 'Facebook', 
    icon: Facebook, 
    url: 'https://www.facebook.com/profile.php?id=61576167746931', 
    color: 'bg-[#1877F2]',
    hoverColor: 'hover:text-[#1877F2]'
  },
  { 
    name: 'LinkedIn', 
    icon: Linkedin, 
    url: '#', 
    color: 'bg-[#0077B5]',
    hoverColor: 'hover:text-[#0077B5]'
  },
  { 
    name: 'TikTok', 
    icon: TikTokIcon, 
    url: 'https://www.tiktok.com/@matiglas.com', 
    color: 'bg-[#000000]',
    hoverColor: 'hover:text-[#000000]'
  }
];

const isMobile = ref(false);

const checkIfMobile = () => {
  isMobile.value = window.innerWidth < 768;
};

onMounted(() => {
  checkIfMobile();
  window.addEventListener('resize', checkIfMobile);
});

onUnmounted(() => {
  window.removeEventListener('resize', checkIfMobile);
});
</script>

<template>
  <section id="contacto" class="py-24 sm:py-32 relative">
    <div class="absolute top-40 -left-20 w-80 h-80 bg-primary/5 rounded-full blur-3xl opacity-70 animate-blob animation-delay-3000"></div>
    <div class="absolute bottom-20 right-10 w-80 h-80 bg-secondary/5 rounded-full blur-3xl opacity-70 animate-blob"></div>
    
    <div class="container">
      <div 
        v-if="formSubmitted" 
        class="fixed inset-0 flex items-center justify-center bg-black/30 dark:bg-black/50 z-50 transition-opacity duration-300"
        :class="formSubmitted ? 'opacity-100' : 'opacity-0 pointer-events-none'"
      >
        <Alert 
          class="max-w-md w-11/12 bg-background border-primary shadow-xl rounded-lg p-6 transform transition-all duration-300 ease-out"
          :class="formSubmitted ? 'scale-100 opacity-100' : 'scale-95 opacity-0'"
        >
          <div class="flex items-center space-x-3">
            <CheckCircle2 class="h-6 w-6 text-primary" />
            <div>
              <AlertDescription class="text-foreground">
                ¡Mensaje enviado! Revise su bandeja de entrada. Si no lo encuentra, por favor revise su carpeta de spam.
              </AlertDescription>
            </div>
          </div>
        </Alert>
      </div>

      <AnimateOnScroll>
        <div class="text-center mb-12">
          <h2 class="text-lg text-primary mb-2 tracking-wider">Contacto</h2>
          <h2 class="text-3xl md:text-4xl font-bold mb-4 text-secondary">
            Estamos para Atenderte
          </h2>
          <p class="md:w-3/4 mx-auto text-xl text-muted-foreground">
            Contáctanos para solicitar un presupuesto o para resolver cualquier consulta sobre nuestros servicios.
          </p>
        </div>
      </AnimateOnScroll>

      <div class="grid lg:grid-cols-2 gap-8 w-full max-w-5xl mx-auto">
        <AnimateOnScroll>
          <Card class="bg-card shadow-lg overflow-hidden transform-gpu transition-all duration-500 hover:shadow-xl hover:shadow-primary/5">
            <CardContent class="p-6">
              <form @submit.prevent="handleSubmit" class="space-y-6">
                <div class="space-y-4">
                  <div 
                    class="relative border rounded-md transition-all duration-300 hover:border-primary/50"
                    :class="{'border-primary shadow-sm shadow-primary/20': focusedField === 'nombre'}"
                  >
                    <Input 
                      type="text" 
                      placeholder="Nombre completo" 
                      class="border-none focus-visible:ring-0 focus-visible:ring-offset-0" 
                      @focus="setFocusedField('nombre')"
                      @blur="clearFocusedField"
                      :disabled="formSubmitted || isSubmitting"
                      v-model="formData.nombre"
                      required
                    />
                    <div 
                      class="absolute bottom-0 left-0 h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-300 ease-out"
                      :class="{'scale-x-100': focusedField === 'nombre'}"
                    ></div>
                  </div>
                  
                  <div 
                    class="relative border rounded-md transition-all duration-300 hover:border-primary/50"
                    :class="{'border-primary shadow-sm shadow-primary/20': focusedField === 'email'}"
                  >
                    <Input 
                      type="email" 
                      placeholder="Correo electrónico" 
                      class="border-none focus-visible:ring-0 focus-visible:ring-offset-0"
                      @focus="setFocusedField('email')"
                      @blur="clearFocusedField"
                      :disabled="formSubmitted || isSubmitting"
                      v-model="formData.email"
                      required
                    />
                    <div 
                      class="absolute bottom-0 left-0 h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-300 ease-out"
                      :class="{'scale-x-100': focusedField === 'email'}"
                    ></div>
                  </div>
                  
                  <div 
                    class="relative border rounded-md transition-all duration-300 hover:border-primary/50"
                    :class="{'border-primary shadow-sm shadow-primary/20': focusedField === 'telefono'}"
                  >
                    <Input 
                      type="tel" 
                      placeholder="Teléfono" 
                      class="border-none focus-visible:ring-0 focus-visible:ring-offset-0"
                      @focus="setFocusedField('telefono')"
                      @blur="clearFocusedField"
                      :disabled="formSubmitted || isSubmitting"
                      v-model="formData.telefono"
                    />
                    <div 
                      class="absolute bottom-0 left-0 h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-300 ease-out"
                      :class="{'scale-x-100': focusedField === 'telefono'}"
                    ></div>
                  </div>
                  
                  <div 
                    class="relative border rounded-md transition-all duration-300 hover:border-primary/50"
                    :class="{'border-primary shadow-sm shadow-primary/20': focusedField === 'mensaje'}"
                  >
                    <Textarea 
                      placeholder="Tu mensaje" 
                      rows="4" 
                      class="border-none focus-visible:ring-0 focus-visible:ring-offset-0 resize-none"
                      @focus="setFocusedField('mensaje')"
                      @blur="clearFocusedField"
                      :disabled="formSubmitted || isSubmitting"
                      v-model="formData.mensaje"
                      required
                    />
                    <div 
                      class="absolute bottom-0 left-0 h-0.5 bg-primary transform scale-x-0 origin-left transition-transform duration-300 ease-out"
                      :class="{'scale-x-100': focusedField === 'mensaje'}"
                    ></div>
                  </div>
                </div>
                
                <div v-if="errorMessage" class="text-red-500 text-sm">
                  {{ errorMessage }}
                </div>
                
                <Button 
                  type="submit" 
                  class="w-full bg-primary text-secondary hover:bg-primary/90 font-semibold btn-hover-effect group relative overflow-hidden"
                  :disabled="formSubmitted || isSubmitting"
                >
                  <span v-if="!formSubmitted && !isSubmitting" class="flex items-center">
                    Enviar mensaje
                    <Send class="ml-2 size-4 group-hover:translate-x-1 group-hover:-translate-y-1 transition-transform duration-300" />
                  </span>
                  <span v-else-if="isSubmitting" class="flex items-center">
                    Enviando
                    <Loader2 class="ml-2 size-4 animate-spin" />
                  </span>
                  <span v-else class="flex items-center">
                    Mensaje enviado
                    <CheckCircle2 class="ml-2 size-4 animate-bounce" />
                  </span>
                </Button>
              </form>
            </CardContent>
          </Card>
        </AnimateOnScroll>

        <AnimateOnScroll :delay="200" :direction="'left'">
          <div class="space-y-6">
            <div class="bg-card border border-border p-6 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 flex items-start space-x-4 group transform hover:translate-y-[-2px]">
              <div class="bg-secondary/10 p-3 rounded-full text-secondary group-hover:bg-secondary group-hover:text-white transition-all duration-300">
                <MapPin class="size-6" />
              </div>
              <div>
                <h3 class="font-semibold text-lg mb-1 text-secondary">Ubicación</h3>
                <p class="text-muted-foreground">
                  Av. Los Héroes 123, San Juan de Miraflores<br />
                  Lima, Perú
                </p>
              </div>
            </div>

            <div class="bg-card border border-border p-6 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 flex items-start space-x-4 group transform hover:translate-y-[-2px]">
              <div class="bg-secondary/10 p-3 rounded-full text-secondary group-hover:bg-secondary group-hover:text-white transition-all duration-300">
                <Phone class="size-6" />
              </div>
              <div>
                <h3 class="font-semibold text-lg mb-1 text-secondary">Teléfono</h3>
                <p class="text-muted-foreground">
                  <a href="tel:+51987654321" class="hover:text-primary transition-colors duration-300">+51 987 654 321</a><br />
                  <a href="tel:+51912345678" class="hover:text-primary transition-colors duration-300">+51 912 345 678</a>
                </p>
              </div>
            </div>

            <div class="bg-card border border-border p-6 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 flex items-start space-x-4 group transform hover:translate-y-[-2px]">
              <div class="bg-secondary/10 p-3 rounded-full text-secondary group-hover:bg-secondary group-hover:text-white transition-all duration-300">
                <Mail class="size-6" />
              </div>
              <div>
                <h3 class="font-semibold text-lg mb-1 text-secondary">Email</h3>
                <p class="text-muted-foreground">
                  <a href="mailto:info@matiglas.com" class="hover:text-primary transition-colors duration-300">info@matiglas.com</a><br />
                  <a href="mailto:ventas@matiglas.com" class="hover:text-primary transition-colors duration-300">ventas@matiglas.com</a>
                </p>
              </div>
            </div>

            <div class="bg-card border border-border p-6 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 space-y-3 transform hover:translate-y-[-2px]">
              <h3 class="font-semibold text-lg mb-3 text-secondary">Síguenos en redes sociales</h3>
              
              <div class="flex flex-wrap gap-3">
                <a 
                  v-for="(social, index) in socialLinks" 
                  :key="index"
                  :href="social.url"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="relative group/icon"
                >
                  <div class="absolute inset-0 rounded-full opacity-0 group-hover/icon:opacity-100 transition-opacity duration-300" :class="social.color"></div>
                  
                  <div 
                    class="relative z-10 p-3 rounded-full border border-border bg-background text-muted-foreground transition-colors duration-300 transform hover:scale-110 hover:rotate-3 hover:shadow-lg"
                    :class="isMobile ? '' : social.hoverColor"
                  >
                    <component :is="social.icon" class="size-6" />
                    
                    <span class="absolute -bottom-8 left-1/2 transform -translate-x-1/2 bg-secondary text-white text-xs py-1 px-2 rounded opacity-0 group-hover/icon:opacity-100 whitespace-nowrap transition-all duration-300 group-hover/icon:translate-y-[-4px]">
                      {{ social.name }}
                    </span>
                  </div>
                </a>
              </div>
            </div>

            <div class="relative h-64 md:h-80 rounded-lg overflow-hidden border border-border shadow-sm group">
              <iframe 
                src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3900.587839691002!2d-76.9980016851864!3d-12.14001199140398!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x9105b8111a0111c3%3A0x3d8e6f7a32a4e1e!2sAv.%20Los%20Heroes%20123%2C%20San%20Juan%20de%20Miraflores%2015803%2C%20Peru!5e0!3m2!1sen!2sus!4v1678886400000!5m2!1sen!2sus" 
                width="100%" 
                height="100%" 
                style="border:0;" 
                allowfullscreen 
                loading="lazy" 
                referrerpolicy="no-referrer-when-downgrade"
                class="absolute inset-0 w-full h-full"
              ></iframe>
              <div class="absolute inset-0 bg-gradient-to-t from-black/30 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 pointer-events-none"></div>
              <div class="absolute bottom-2 right-2 bg-white dark:bg-secondary p-1 rounded shadow text-xs">
                <a href="https://www.google.com/maps/place/Av.+Los+Heroes+123,+San+Juan+de+Miraflores+15803,+Peru/@-12.140012,-76.9980017,17z/data=!3m1!4b1!4m5!3m4!1s0x9105b8111a0111c3:0x3d8e6f7a32a4e1e!8m2!3d-12.140012!4d-76.995813" target="_blank" rel="noopener noreferrer" class="text-blue-600 hover:underline">Ver en Google Maps</a>
              </div>
            </div>
          </div>
        </AnimateOnScroll>
      </div>
    </div>
  </section>
</template>

<style scoped>
.animate-blob {
  animation: blob-bounce 8s infinite ease;
}

.animation-delay-3000 {
  animation-delay: 3s;
}

@keyframes blob-bounce {
  0% {
    transform: translate(0, 0) scale(1);
  }
  33% {
    transform: translate(20px, -20px) scale(1.1);
  }
  66% {
    transform: translate(-15px, 15px) scale(0.9);
  }
  100% {
    transform: translate(0, 0) scale(1);
  }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

social-icon-pulse {
  animation: pulse 2s infinite;
}
</style>
