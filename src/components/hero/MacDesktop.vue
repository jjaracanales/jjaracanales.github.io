<template>
  <Transition name="fade">
    <div v-if="showDesktop" class="mac-desktop">
      <div class="desktop-wallpaper">
        <div class="desktop-icons">
          <div class="desktop-icon" @click.stop="$emit('open-window')">
            <div class="icon-image">
              <CodeOutlined />
            </div>
            <span>developer.js</span>
          </div>
          <div class="desktop-icon" @click.stop="$emit('open-finder')">
            <div class="icon-image folder">
              <FolderOutlined />
            </div>
            <span>Proyectos</span>
          </div>
          <div class="desktop-icon" @click.stop="$emit('open-games')">
            <div class="icon-image games">
              🎮
            </div>
            <span>Juegos</span>
          </div>
        </div>
        
        <div class="dock">
          <div class="dock-item" 
               :class="{ 'has-window': windowState === 'minimized' }" 
               @click.stop="$emit('open-window')" 
               title="developer.js">
            <CodeOutlined />
            <div v-if="windowState === 'minimized'" class="dock-indicator"></div>
            <div v-if="windowState === 'minimized'" class="dock-preview">
              <span>developer.js</span>
            </div>
          </div>
          <div class="dock-item" 
               @click.stop="$emit('open-finder')" 
               :class="{ 'has-window': finderState === 'minimized', 'active': isFinderOpen }" 
               title="Finder">
            <FolderOutlined />
            <div v-if="finderState === 'minimized'" class="dock-indicator"></div>
            <div class="dock-preview"><span>Proyectos</span></div>
          </div>
          <div class="dock-item" @click.stop="$emit('open-github')" title="GitHub">
            <GithubOutlined />
            <div class="dock-preview"><span>GitHub</span></div>
          </div>
          <div class="dock-item" @click.stop="$emit('open-linkedin')" title="LinkedIn">
            <LinkedinOutlined />
            <div class="dock-preview"><span>LinkedIn</span></div>
          </div>
          <div class="dock-item" 
               @click.stop="$emit('open-calculator')" 
               :class="{ 'has-window': calcState === 'minimized', 'active': isCalcOpen }" 
               title="Calculadora">
            <CalculatorOutlined />
            <div v-if="calcState === 'minimized'" class="dock-indicator"></div>
            <div class="dock-preview"><span>Calculadora</span></div>
          </div>
          <div v-if="gamesState === 'minimized'" class="dock-item has-window" 
               @click.stop="$emit('open-games')" 
               title="Juegos">
            <span class="dock-emoji">🎮</span>
            <div class="dock-indicator"></div>
            <div class="dock-preview"><span>Juegos</span></div>
          </div>
          <div v-if="snakeState === 'minimized'" class="dock-item has-window" 
               @click.stop="$emit('open-snake')" 
               title="Snake">
            <span class="dock-emoji">🐍</span>
            <div class="dock-indicator"></div>
            <div class="dock-preview"><span>Snake</span></div>
          </div>
          <div v-if="flappyState === 'minimized'" class="dock-item has-window" 
               @click.stop="$emit('open-flappy')" 
               title="Flappy Bird">
            <span class="dock-emoji">🐦</span>
            <div class="dock-indicator"></div>
            <div class="dock-preview"><span>Flappy Bird</span></div>
          </div>
          <div class="dock-item" @click.stop="$emit('open-email')" title="Email">
            <MailOutlined />
            <div class="dock-preview"><span>Email</span></div>
          </div>
          <div class="dock-divider"></div>
          <div class="dock-item trash" title="Papelera">
            <DeleteOutlined />
          </div>
        </div>
        
        <div class="menu-bar">
          <span class="apple-logo"><AppleOutlined /></span>
          <span>Finder</span>
          <span>Archivo</span>
          <span>Editar</span>
          <span>Ver</span>
        </div>
        
        <div class="click-hint">{{ hintText }}</div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { computed } from 'vue'
import { 
  CodeOutlined,
  FolderOutlined,
  GithubOutlined,
  LinkedinOutlined,
  CalculatorOutlined,
  MailOutlined,
  DeleteOutlined,
  AppleOutlined
} from '@ant-design/icons-vue'

const props = defineProps({
  windowState: {
    type: String,
    default: 'closed'
  },
  calcState: {
    type: String,
    default: 'closed'
  },
  finderState: {
    type: String,
    default: 'closed'
  },
  gamesState: {
    type: String,
    default: 'closed'
  },
  snakeState: {
    type: String,
    default: 'closed'
  },
  flappyState: {
    type: String,
    default: 'closed'
  }
})

defineEmits([
  'open-window', 
  'open-finder', 
  'open-calculator', 
  'open-github', 
  'open-linkedin', 
  'open-email',
  'open-games',
  'open-snake',
  'open-flappy'
])

const showDesktop = computed(() => {
  return (props.windowState === 'closed' || props.windowState === 'minimized') &&
         props.calcState !== 'normal' && props.calcState !== 'maximized' &&
         props.finderState !== 'normal' && props.finderState !== 'maximized' &&
         props.gamesState !== 'normal' && props.gamesState !== 'maximized' &&
         props.snakeState !== 'normal' && props.snakeState !== 'maximized' &&
         props.flappyState !== 'normal' && props.flappyState !== 'maximized'
})

const isFinderOpen = computed(() => props.finderState === 'normal' || props.finderState === 'maximized')
const isCalcOpen = computed(() => props.calcState === 'normal' || props.calcState === 'maximized')

const hintText = computed(() => {
  return props.windowState === 'minimized' ? 'Click en el dock para restaurar' : 'Click para abrir'
})
</script>

<style scoped>
.mac-desktop {
  width: 420px;
  height: 380px;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.4);
  cursor: default;
  position: relative;
}

.desktop-wallpaper {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, 
    #1a1a2e 0%, 
    #16213e 30%,
    #0f3460 60%,
    #533483 100%
  );
  position: relative;
  overflow: hidden;
}

.desktop-wallpaper::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(24, 144, 255, 0.3) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  animation: desktopGlow 4s ease-in-out infinite;
}

@keyframes desktopGlow {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.2); }
}

.menu-bar {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 24px;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  padding: 0 12px;
  gap: 16px;
  font-size: 0.7rem;
  color: white;
}

.apple-logo {
  font-size: 0.9rem;
}

.desktop-icons {
  position: absolute;
  top: 45px;
  right: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.desktop-icon {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.desktop-icon:hover {
  transform: scale(1.1);
}

.icon-image {
  width: 52px;
  height: 52px;
  background: linear-gradient(135deg, var(--color-accent, #1890ff) 0%, #0050b3 100%);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  color: white;
  box-shadow: 0 4px 12px rgba(24, 144, 255, 0.3);
}

.icon-image.folder {
  background: linear-gradient(135deg, #1890ff 0%, #40a9ff 100%);
}

.icon-image.games {
  background: linear-gradient(135deg, #32d74b 0%, #28a745 100%);
  font-size: 1.8rem;
}

.desktop-icon span {
  font-size: 0.65rem;
  color: white;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.8);
  max-width: 60px;
  text-align: center;
}

.dock {
  position: absolute;
  bottom: 12px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(20px);
  border-radius: 18px;
  padding: 8px 12px;
  display: flex;
  gap: 8px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.dock-item {
  width: 38px;
  height: 38px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.2) 0%, rgba(255, 255, 255, 0.05) 100%);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  color: white;
  cursor: pointer;
  transition: all 0.2s ease;
  position: relative;
}

.dock-emoji {
  font-size: 1.3rem;
}

.dock-item:hover {
  transform: translateY(-8px) scale(1.15);
  background: var(--color-accent, #1890ff);
}

.dock-item.has-window {
  background: linear-gradient(135deg, var(--color-accent, #1890ff) 0%, #0050b3 100%);
  animation: dockBounce 0.5s ease;
}

@keyframes dockBounce {
  0% { transform: translateY(0) scale(1); }
  30% { transform: translateY(-15px) scale(1.2); }
  50% { transform: translateY(-5px) scale(1.1); }
  70% { transform: translateY(-10px) scale(1.15); }
  100% { transform: translateY(0) scale(1); }
}

.dock-item.has-window:hover {
  transform: translateY(-12px) scale(1.2);
}

.dock-indicator {
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  width: 4px;
  height: 4px;
  background: white;
  border-radius: 50%;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.8);
}

.dock-preview {
  position: absolute;
  bottom: 45px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  padding: 6px 12px;
  border-radius: 6px;
  white-space: nowrap;
  opacity: 0;
  transition: all 0.2s ease;
  pointer-events: none;
}

.dock-preview span {
  font-size: 0.7rem;
  color: white;
}

.dock-preview::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 50%;
  transform: translateX(-50%);
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 6px solid rgba(0, 0, 0, 0.85);
}

.dock-item:hover .dock-preview {
  opacity: 1;
  bottom: 50px;
}

.dock-item.active {
  background: var(--color-accent, #1890ff);
  box-shadow: 0 0 15px rgba(24, 144, 255, 0.5);
}

.dock-divider {
  width: 1px;
  height: 28px;
  background: rgba(255, 255, 255, 0.3);
  margin: 4px 4px;
}

.dock-item.trash {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1) 0%, rgba(255, 255, 255, 0.05) 100%);
}

.click-hint {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 0.8rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.mac-desktop:hover .click-hint {
  opacity: 1;
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
