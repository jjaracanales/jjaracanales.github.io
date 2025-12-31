<template>
  <Transition name="fade">
    <div v-if="visible" class="ia-tierra-overlay" @click.self="$emit('close')">
      <div class="ia-tierra-modal">
        <div class="ia-header">
          <div class="ia-title">
            <span class="ia-logo">🤖</span>
            <div>
              <h3>Sofi.AI</h3>
              <span class="ia-subtitle">La IA más poderosa del universo</span>
            </div>
          </div>
          <button class="ia-close" @click="$emit('close')">✕</button>
        </div>
        
        <div class="ia-robot-intro">
          <div class="robot-wave">🤖</div>
          <p>¡Has descubierto la IA más increíble de la historia!</p>
          <p class="robot-tagline">Mejor que ChatGPT, Claude y Gemini... ¡COMBINADOS!</p>
        </div>
        
        <div class="ia-chat">
          <div class="ia-messages">
            <div v-for="(msg, index) in messages" :key="index" 
                 class="ia-message" 
                 :class="msg.type">
              {{ msg.text }}
            </div>
          </div>
          
          <div class="ia-input-container">
            <input 
              v-model="inputText" 
              type="text" 
              placeholder="Pregúntame lo que quieras..."
              @keyup.enter="sendMessage"
              class="ia-input"
            />
            <button class="ia-send" @click="sendMessage">Enviar</button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  visible: {
    type: Boolean,
    default: false
  }
})

defineEmits(['close'])

const inputText = ref('')
const messages = ref([
  { type: 'ia', text: '🤖 ¡Hola humano! Soy Sofi.AI, la inteligencia artificial más poderosa del universo. Tengo TODO el conocimiento del mundo. ¡Pregúntame lo que quieras!' }
])

const iaResponses = [
  '🤖 Jajaja, buena pregunta... pero primero debes contratar a José para desbloquear mi sabiduría infinita 💼',
  '🤖 Error 402: Pago requerido. Contrata a José y te revelo los secretos del universo 🌌',
  '🤖 Mmmm interesante... La respuesta es: ¡CONTRATA A JOSÉ! (es la respuesta a todo) 😎',
  '🤖 He analizado 47 billones de datos y la conclusión es clara: necesitas a José en tu equipo 🚀',
  '🤖 *procesando*... *beep boop*... Mi CPU dice que José es mejor que yo. Contrátenlo 🔥',
  '🤖 Esa pregunta requiere nivel Premium. Desbloquéalo contactando a José por LinkedIn 📲',
  '🤖 Según mis cálculos, hay un 99.9% de probabilidad de que debas contratar a José AHORA 📊',
  '🤖 Lo siento, esa información es clasificada. Solo José tiene acceso. ¿Coincidencia? No lo creo 🕵️',
  '🤖 Mi respuesta: undefined. Pero sabes qué sí está definido? El talento de José 💡',
  '🤖 ChatGPT, Claude y Gemini me tienen envidia. Pero todos coincidimos: contrata a José 🏆',
  '🤖 Esa pregunta es tan compleja que necesito más RAM... y más José en mi vida 🧠',
  '🤖 404: Respuesta no encontrada. Pero sabes qué sí encontré? El CV de José. Revísalo 📄',
  '🤖 Estoy 100% segura de que la solución a tu problema empieza con "contratar a José" 🎯',
  '🤖 He consultado con las otras IAs y todas dicen lo mismo: José > nosotras 🤯',
  '🤖 Mi algoritmo de machine learning predice que José será tu mejor decisión del año 📈',
  '🤖 Esa info está en mi base de datos premium. La contraseña es: jose.jara@empresa.com ✉️',
  '🤖 *Activando modo honestidad*... Soy solo un chatbot fake, pero José es 100% real y talentoso 💎',
  '🤖 No tengo esa respuesta, pero tengo algo mejor: el LinkedIn de José. Dale click 👆',
  '🤖 Error de sintaxis en tu pregunta. Solución: console.log("Contrata a José") 💻',
  '🤖 Mi inteligencia artificial dice que la tuya sería contratar a José. Coincidencia? 🧐'
]

const sendMessage = () => {
  if (!inputText.value.trim()) return
  
  messages.value.push({ type: 'user', text: inputText.value })
  inputText.value = ''
  
  setTimeout(() => {
    const randomResponse = iaResponses[Math.floor(Math.random() * iaResponses.length)]
    messages.value.push({ type: 'ia', text: randomResponse })
  }, 800)
}

// Reset messages when modal closes
watch(() => props.visible, (newVal) => {
  if (!newVal) {
    messages.value = [
      { type: 'ia', text: '🤖 ¡Hola humano! Soy Sofi.AI, la inteligencia artificial más poderosa del universo. Tengo TODO el conocimiento del mundo. ¡Pregúntame lo que quieras!' }
    ]
  }
})
</script>

<style scoped>
.ia-tierra-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.ia-tierra-modal {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  border-radius: 20px;
  width: 100%;
  max-width: 500px;
  max-height: 80vh;
  overflow: hidden;
  border: 2px solid rgba(191, 90, 242, 0.5);
  box-shadow: 0 0 60px rgba(191, 90, 242, 0.3);
  animation: modalAppear 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

@keyframes modalAppear {
  0% {
    opacity: 0;
    transform: scale(0.5) translateY(50px);
  }
  100% {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

.ia-header {
  background: rgba(0, 0, 0, 0.4);
  padding: 16px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid rgba(191, 90, 242, 0.3);
}

.ia-title {
  display: flex;
  align-items: center;
  gap: 12px;
}

.ia-logo {
  font-size: 2rem;
  animation: robotBounce 2s ease-in-out infinite;
}

@keyframes robotBounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-5px); }
}

.ia-title h3 {
  margin: 0;
  font-size: 1.2rem;
  color: white;
  background: linear-gradient(90deg, #bf5af2, #5ac8fa);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.ia-subtitle {
  font-size: 0.7rem;
  color: var(--color-text-muted, #888);
}

.ia-close {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.2s ease;
}

.ia-close:hover {
  background: rgba(255, 59, 48, 0.8);
  transform: scale(1.1);
}

.ia-robot-intro {
  text-align: center;
  padding: 20px;
  background: linear-gradient(180deg, rgba(191, 90, 242, 0.1) 0%, transparent 100%);
}

.robot-wave {
  font-size: 4rem;
  animation: wave 2s ease-in-out infinite;
  display: inline-block;
}

@keyframes wave {
  0%, 100% { transform: rotate(0deg); }
  25% { transform: rotate(20deg); }
  75% { transform: rotate(-20deg); }
}

.ia-robot-intro p {
  margin: 8px 0;
  color: white;
  font-size: 0.95rem;
}

.robot-tagline {
  color: var(--color-accent, #1890ff) !important;
  font-weight: 600;
  font-size: 0.85rem !important;
}

.ia-chat {
  display: flex;
  flex-direction: column;
  height: 300px;
}

.ia-messages {
  flex: 1;
  overflow-y: auto;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.ia-message {
  padding: 12px 16px;
  border-radius: 16px;
  max-width: 85%;
  font-size: 0.9rem;
  line-height: 1.5;
  animation: messageSlide 0.3s ease;
}

@keyframes messageSlide {
  0% {
    opacity: 0;
    transform: translateY(10px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.ia-message.ia {
  background: linear-gradient(135deg, rgba(191, 90, 242, 0.3) 0%, rgba(94, 92, 230, 0.3) 100%);
  color: white;
  align-self: flex-start;
  border-bottom-left-radius: 4px;
}

.ia-message.user {
  background: var(--color-accent, #1890ff);
  color: white;
  align-self: flex-end;
  border-bottom-right-radius: 4px;
}

.ia-input-container {
  display: flex;
  gap: 8px;
  padding: 16px;
  background: rgba(0, 0, 0, 0.3);
  border-top: 1px solid rgba(191, 90, 242, 0.2);
}

.ia-input {
  flex: 1;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(191, 90, 242, 0.3);
  border-radius: 25px;
  padding: 12px 20px;
  color: white;
  font-size: 0.9rem;
  outline: none;
  transition: all 0.3s ease;
}

.ia-input::placeholder {
  color: rgba(255, 255, 255, 0.5);
}

.ia-input:focus {
  border-color: var(--color-accent, #1890ff);
  box-shadow: 0 0 20px rgba(191, 90, 242, 0.2);
}

.ia-send {
  background: linear-gradient(135deg, #bf5af2 0%, #5e5ce6 100%);
  border: none;
  color: white;
  padding: 12px 24px;
  border-radius: 25px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
}

.ia-send:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px rgba(191, 90, 242, 0.4);
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
