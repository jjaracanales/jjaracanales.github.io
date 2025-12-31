<template>
  <Transition name="fade">
    <div v-if="visible" class="marciano-overlay" @click="$emit('close')">
      <div class="marciano-modal" @click.stop>
        <div class="marciano-header">
          <span>marciano.gif</span>
          <button class="marciano-close" @click="$emit('close')">✕</button>
        </div>
        <div class="marciano-content">
          <div class="alien-face">
            <div class="alien-antennas">
              <div class="antenna left">
                <div class="antenna-ball"></div>
              </div>
              <div class="antenna right">
                <div class="antenna-ball"></div>
              </div>
            </div>
            <div class="alien-head-shape">
              <div class="alien-eyes">
                <div class="eye left">
                  <div class="pupil"></div>
                </div>
                <div class="eye right">
                  <div class="pupil"></div>
                </div>
              </div>
              <div class="alien-mouth">👄</div>
            </div>
          </div>
          <p class="marciano-caption">🛸 ¡Hola terrícola! Soy el marciano mascota de José 👽</p>
          <p class="marciano-subcaption">PD: Si me estás viendo, deberías contratar a mi amigo José 🚀</p>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
defineProps({
  visible: {
    type: Boolean,
    default: false
  }
})

defineEmits(['close'])
</script>

<style scoped>
.marciano-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.marciano-modal {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  border-radius: 16px;
  overflow: hidden;
  border: 2px solid rgba(24, 144, 255, 0.3);
  box-shadow: 0 0 60px rgba(24, 144, 255, 0.2);
  animation: modalAppear 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  min-width: 320px;
}

@keyframes modalAppear {
  0% {
    opacity: 0;
    transform: scale(0.5);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.marciano-header {
  background: rgba(0, 0, 0, 0.4);
  padding: 12px 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: white;
  font-size: 0.9rem;
}

.marciano-close {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s ease;
}

.marciano-close:hover {
  background: rgba(255, 59, 48, 0.8);
}

.marciano-content {
  padding: 30px 40px;
  text-align: center;
}

/* Alien Face - Animated */
.alien-face {
  position: relative;
  margin-bottom: 24px;
}

.alien-antennas {
  display: flex;
  justify-content: center;
  gap: 60px;
  margin-bottom: -10px;
}

.antenna {
  width: 4px;
  height: 30px;
  background: linear-gradient(to top, #32d74b, #5ac8fa);
  border-radius: 4px;
  position: relative;
}

.antenna.left {
  transform: rotate(-15deg);
  animation: antennaWiggle 2s ease-in-out infinite;
}

.antenna.right {
  transform: rotate(15deg);
  animation: antennaWiggle 2s ease-in-out infinite 0.5s;
}

@keyframes antennaWiggle {
  0%, 100% { transform: rotate(-15deg); }
  50% { transform: rotate(-25deg); }
}

.antenna.right {
  animation-name: antennaWiggleRight;
}

@keyframes antennaWiggleRight {
  0%, 100% { transform: rotate(15deg); }
  50% { transform: rotate(25deg); }
}

.antenna-ball {
  position: absolute;
  top: -8px;
  left: 50%;
  transform: translateX(-50%);
  width: 12px;
  height: 12px;
  background: radial-gradient(circle at 30% 30%, #5ac8fa, #32d74b);
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(90, 200, 250, 0.8);
  animation: glowPulse 1.5s ease-in-out infinite;
}

@keyframes glowPulse {
  0%, 100% { box-shadow: 0 0 10px rgba(90, 200, 250, 0.8); }
  50% { box-shadow: 0 0 20px rgba(90, 200, 250, 1), 0 0 30px rgba(50, 215, 75, 0.5); }
}

.alien-head-shape {
  width: 120px;
  height: 140px;
  background: linear-gradient(180deg, #32d74b 0%, #28a745 50%, #1e7e34 100%);
  border-radius: 50% 50% 45% 45%;
  margin: 0 auto;
  position: relative;
  box-shadow: 
    inset -10px -10px 30px rgba(0, 0, 0, 0.3),
    inset 10px 10px 30px rgba(255, 255, 255, 0.1),
    0 10px 30px rgba(50, 215, 75, 0.3);
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.alien-eyes {
  display: flex;
  justify-content: center;
  gap: 20px;
  padding-top: 30px;
}

.eye {
  width: 35px;
  height: 45px;
  background: white;
  border-radius: 50%;
  position: relative;
  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.2);
}

.pupil {
  width: 15px;
  height: 20px;
  background: #1a1a2e;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: lookAround 4s ease-in-out infinite;
}

@keyframes lookAround {
  0%, 100% { transform: translate(-50%, -50%); }
  25% { transform: translate(-30%, -50%); }
  50% { transform: translate(-50%, -30%); }
  75% { transform: translate(-70%, -50%); }
}

.pupil::after {
  content: '';
  position: absolute;
  width: 5px;
  height: 5px;
  background: white;
  border-radius: 50%;
  top: 3px;
  right: 3px;
}

.alien-mouth {
  position: absolute;
  bottom: 25px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 1.5rem;
  animation: mouthWiggle 3s ease-in-out infinite;
}

@keyframes mouthWiggle {
  0%, 100% { transform: translateX(-50%) scale(1); }
  50% { transform: translateX(-50%) scale(1.1); }
}

.marciano-caption {
  color: white;
  font-size: 1.1rem;
  margin: 0 0 8px;
}

.marciano-subcaption {
  color: var(--color-text-muted, #888);
  font-size: 0.9rem;
  margin: 0;
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
