<template>
  <a 
    :href="whatsappUrl" 
    target="_blank" 
    rel="noopener noreferrer"
    class="whatsapp-button"
    title="Contáctame por WhatsApp"
    @mouseenter="isHovered = true"
    @mouseleave="isHovered = false"
  >
    <div class="whatsapp-icon">
      <WhatsAppOutlined />
    </div>
    <Transition name="tooltip">
      <span v-if="isHovered" class="whatsapp-tooltip">¡Hablemos! 💬</span>
    </Transition>
    <div class="pulse-ring"></div>
  </a>
</template>

<script setup>
import { ref } from 'vue'
import { WhatsAppOutlined } from '@ant-design/icons-vue'

const isHovered = ref(false)

const phoneNumber = '56959017230'
const defaultMessage = '¡Hola José! Vi tu portafolio y me gustaría contactarte.'
const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodeURIComponent(defaultMessage)}`
</script>

<style scoped>
.whatsapp-button {
  position: fixed;
  bottom: 30px;
  right: 30px;
  z-index: 1000;
  display: flex;
  align-items: center;
  gap: 12px;
  text-decoration: none;
}

.whatsapp-icon {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.8rem;
  color: white;
  box-shadow: 0 4px 20px rgba(37, 211, 102, 0.4);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  cursor: pointer;
  position: relative;
}

.whatsapp-button:hover .whatsapp-icon {
  transform: scale(1.1) rotate(10deg);
  box-shadow: 0 6px 30px rgba(37, 211, 102, 0.6);
}

.whatsapp-tooltip {
  position: absolute;
  right: 75px;
  background: rgba(30, 30, 30, 0.95);
  backdrop-filter: blur(10px);
  color: white;
  padding: 10px 16px;
  border-radius: 12px;
  font-size: 0.9rem;
  font-weight: 500;
  white-space: nowrap;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.whatsapp-tooltip::after {
  content: '';
  position: absolute;
  right: -8px;
  top: 50%;
  transform: translateY(-50%);
  border-left: 8px solid rgba(30, 30, 30, 0.95);
  border-top: 8px solid transparent;
  border-bottom: 8px solid transparent;
}

.pulse-ring {
  position: absolute;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: rgba(37, 211, 102, 0.4);
  animation: pulse 2s ease-out infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 0.6;
  }
  100% {
    transform: scale(1.8);
    opacity: 0;
  }
}

/* Tooltip transition */
.tooltip-enter-active {
  animation: tooltipIn 0.3s ease;
}

.tooltip-leave-active {
  animation: tooltipOut 0.2s ease;
}

@keyframes tooltipIn {
  0% {
    opacity: 0;
    transform: translateX(10px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes tooltipOut {
  0% {
    opacity: 1;
    transform: translateX(0);
  }
  100% {
    opacity: 0;
    transform: translateX(10px);
  }
}

/* Mobile adjustments */
@media (max-width: 768px) {
  .whatsapp-button {
    bottom: 20px;
    right: 20px;
  }
  
  .whatsapp-icon {
    width: 55px;
    height: 55px;
    font-size: 1.6rem;
  }
  
  .pulse-ring {
    width: 55px;
    height: 55px;
  }
  
  .whatsapp-tooltip {
    display: none;
  }
}
</style>
