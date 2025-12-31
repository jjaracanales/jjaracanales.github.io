<template>
  <Transition name="window">
    <div v-if="isVisible" 
         class="games-window" 
         :class="{ 'maximized': state === 'maximized' }"
         @click.stop>
      <div class="window-header">
        <span class="dot red" @click="$emit('close')" title="Cerrar">
          <CloseOutlined class="dot-icon" />
        </span>
        <span class="dot yellow" @click="$emit('minimize')" title="Minimizar">
          <MinusOutlined class="dot-icon" />
        </span>
        <span class="dot green" @click="$emit('maximize')" title="Maximizar">
          <ExpandOutlined class="dot-icon" />
        </span>
        <span class="window-title">🎮 Juegos</span>
      </div>
      
      <div class="games-content">
        <div class="game-item snake" @dblclick="$emit('open-snake'); $emit('close')">
          <div class="game-icon">🐍</div>
          <span>Snake.app</span>
        </div>
        <div class="game-item flappy" @dblclick="$emit('open-flappy'); $emit('close')">
          <div class="game-icon">🐦</div>
          <span>FlappyBird.app</span>
        </div>
      </div>
      
      <div class="games-statusbar">
        2 elementos
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { computed } from 'vue'
import { CloseOutlined, MinusOutlined, ExpandOutlined } from '@ant-design/icons-vue'

const props = defineProps({
  state: {
    type: String,
    default: 'closed',
    validator: (value) => ['closed', 'normal', 'minimized', 'maximized'].includes(value)
  }
})

defineEmits(['close', 'minimize', 'maximize', 'open-snake', 'open-flappy'])

const isVisible = computed(() => props.state !== 'closed' && props.state !== 'minimized')
</script>

<style scoped>
.games-window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 280px;
  background: var(--glass-bg, rgba(30, 30, 30, 0.95));
  backdrop-filter: blur(20px);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
  z-index: 12;
  display: flex;
  flex-direction: column;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.games-window.maximized {
  width: 350px;
}

.window-header {
  background: rgba(60, 60, 60, 0.95);
  padding: 12px 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.window-title {
  margin-left: auto;
  font-size: 0.75rem;
  color: var(--color-text-muted, #888);
  font-family: 'Fira Code', monospace;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  transition: all 0.2s ease;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.dot-icon {
  font-size: 8px;
  opacity: 0;
  transition: opacity 0.2s ease;
  color: rgba(0, 0, 0, 0.6);
}

.dot:hover .dot-icon { opacity: 1; }
.dot.red { background: #ff5f56; }
.dot.yellow { background: #ffbd2e; }
.dot.green { background: #27ca40; }

.dot.red:hover { background: #ff3b30; transform: scale(1.2); }
.dot.yellow:hover { background: #ff9500; transform: scale(1.2); }
.dot.green:hover { background: #34c759; transform: scale(1.2); }

.games-content {
  padding: 16px;
  background: rgba(30, 30, 30, 0.9);
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.game-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.game-item.snake {
  background: linear-gradient(135deg, rgba(50, 215, 75, 0.15) 0%, rgba(40, 167, 69, 0.15) 100%);
}

.game-item.snake:hover {
  background: linear-gradient(135deg, rgba(50, 215, 75, 0.3) 0%, rgba(40, 167, 69, 0.3) 100%);
  transform: translateX(4px);
}

.game-item.flappy {
  background: linear-gradient(135deg, rgba(255, 214, 10, 0.15) 0%, rgba(255, 159, 10, 0.15) 100%);
}

.game-item.flappy:hover {
  background: linear-gradient(135deg, rgba(255, 214, 10, 0.3) 0%, rgba(255, 159, 10, 0.3) 100%);
  transform: translateX(4px);
}

.game-icon {
  font-size: 2rem;
}

.game-item span {
  font-size: 0.9rem;
  color: white;
  font-weight: 500;
}

.games-statusbar {
  background: rgba(50, 50, 50, 0.9);
  padding: 8px 12px;
  font-size: 0.7rem;
  color: var(--color-text-muted, #888);
  border-top: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
}

/* Window Transitions */
.window-enter-active {
  animation: windowOpen 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.window-leave-active {
  animation: windowClose 0.3s ease-in;
}

@keyframes windowOpen {
  0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
  100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
}

@keyframes windowClose {
  0% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
  100% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
}
</style>
