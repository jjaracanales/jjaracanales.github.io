<template>
  <Transition name="window">
    <div v-if="isVisible" 
         class="finder-window" 
         :class="{ 'maximized': state === 'maximized' }"
         @click.stop>
      <div class="window-header finder-header">
        <span class="dot red" @click="$emit('close')" title="Cerrar">
          <CloseOutlined class="dot-icon" />
        </span>
        <span class="dot yellow" @click="$emit('minimize')" title="Minimizar">
          <MinusOutlined class="dot-icon" />
        </span>
        <span class="dot green" @click="$emit('maximize')" title="Maximizar">
          <ExpandOutlined class="dot-icon" />
        </span>
        <span class="window-title">{{ currentTitle }}</span>
      </div>
      
      <!-- Finder Toolbar -->
      <div class="finder-toolbar">
        <button class="finder-nav-btn" @click="goBack" :disabled="path.length === 0">
          ←
        </button>
        <button class="finder-nav-btn" disabled>→</button>
        <div class="finder-breadcrumb">
          <span @click="path = []">Proyectos</span>
          <template v-for="(item, index) in path" :key="index">
            <span class="breadcrumb-separator">/</span>
            <span @click="path = path.slice(0, index + 1)">{{ item }}</span>
          </template>
        </div>
      </div>
      
      <!-- Finder Content -->
      <div class="finder-content">
        <!-- Root: Proyectos -->
        <template v-if="path.length === 0">
          <div class="finder-item folder folder-blue" @dblclick="navigate('Cómo conquistar el mundo')">
            <FolderOutlined class="finder-icon" />
            <span>Cómo conquistar el mundo</span>
          </div>
          <div class="finder-item folder folder-purple" @dblclick="navigate('IA Secreta')">
            <FolderOutlined class="finder-icon" />
            <span>🤖 IA Secreta</span>
          </div>
          <div class="finder-item folder folder-red" @dblclick="navigate('🔒 PRIVADO')">
            <FolderOutlined class="finder-icon" />
            <span>🔒 PRIVADO - NO ABRIR</span>
          </div>
          <div class="finder-item folder folder-green" @dblclick="navigate('Proyecto NASA')">
            <FolderOutlined class="finder-icon" />
            <span>🚀 Proyecto NASA</span>
          </div>
        </template>
        
        <!-- Subcarpeta: Cómo conquistar el mundo -->
        <template v-else-if="path.length === 1 && path[0] === 'Cómo conquistar el mundo'">
          <div class="finder-item file" @dblclick="navigate('plan.txt')">
            <FileTextOutlined class="finder-icon" />
            <span>plan.txt</span>
          </div>
        </template>
        
        <!-- Subcarpeta: IA Secreta -->
        <template v-else-if="path.length === 1 && path[0] === 'IA Secreta'">
          <div class="finder-item file ia-file" @dblclick="$emit('open-ia'); $emit('close')">
            <span class="robot-emoji">🤖</span>
            <span>sofi.ai</span>
          </div>
        </template>
        
        <!-- Subcarpeta: Proyecto NASA -->
        <template v-else-if="path.length === 1 && path[0] === 'Proyecto NASA'">
          <div class="finder-item file nasa-file" @dblclick="$emit('open-marciano')">
            <span class="alien-emoji">👽</span>
            <span>marciano.gif</span>
          </div>
        </template>
        

        
        <!-- PRIVADO - Carpetas anidadas -->
        <template v-else-if="path[0] === '🔒 PRIVADO'">
          <template v-if="path.length === 1">
            <div class="finder-item folder folder-red" @dblclick="navigate('⚠️ ¿Por qué estás aquí?')">
              <FolderOutlined class="finder-icon" />
              <span>⚠️ ¿Por qué estás aquí?</span>
            </div>
          </template>
          <template v-else-if="path.length === 2">
            <div class="finder-item folder folder-orange" @dblclick="navigate('🚫 Esto es PRIVADO')">
              <FolderOutlined class="finder-icon" />
              <span>🚫 Esto es PRIVADO</span>
            </div>
          </template>
          <template v-else-if="path.length === 3">
            <div class="finder-item folder folder-yellow" @dblclick="navigate('😤 ¡Deja de clickear!')">
              <FolderOutlined class="finder-icon" />
              <span>😤 ¡Deja de clickear!</span>
            </div>
          </template>
          <template v-else-if="path.length === 4">
            <div class="finder-item folder folder-pink" @dblclick="navigate('🙄 Ok sigues aquí...')">
              <FolderOutlined class="finder-icon" />
              <span>🙄 Ok sigues aquí...</span>
            </div>
          </template>
          <template v-else-if="path.length === 5">
            <div class="finder-item folder folder-cyan" @dblclick="navigate('👀 Eres muy curioso eh')">
              <FolderOutlined class="finder-icon" />
              <span>👀 Eres muy curioso eh</span>
            </div>
          </template>
          <template v-else-if="path.length === 6">
            <div class="finder-item folder folder-purple" @dblclick="navigate('🤔 ¿Buscas algo?')">
              <FolderOutlined class="finder-icon" />
              <span>🤔 ¿Buscas algo?</span>
            </div>
          </template>
          <template v-else-if="path.length === 7">
            <div class="finder-item folder folder-blue" @dblclick="navigate('😅 No hay nada aquí')">
              <FolderOutlined class="finder-icon" />
              <span>😅 No hay nada aquí</span>
            </div>
          </template>
          <template v-else-if="path.length === 8">
            <div class="finder-item folder folder-green" @dblclick="navigate('🎉 ¡Felicidades persistente!')">
              <FolderOutlined class="finder-icon" />
              <span>🎉 ¡Felicidades persistente!</span>
            </div>
          </template>
          <template v-else-if="path.length === 9">
            <div class="finder-item folder folder-gold" @dblclick="navigate('🏆 El tesoro final')">
              <FolderOutlined class="finder-icon" />
              <span>🏆 El tesoro final</span>
            </div>
          </template>
          <template v-else-if="path.length === 10">
            <div class="finder-item file secret-file" @dblclick="navigate('carta_secreta.txt')">
              <FileTextOutlined class="finder-icon" />
              <span>📜 carta_secreta.txt</span>
            </div>
          </template>
          <!-- Carta secreta -->
          <template v-else-if="path.includes('carta_secreta.txt')">
            <div class="text-file-content secret-content">
              <pre>
    ╔═══════════════════════════════════════════╗
    ║                                           ║
    ║     🎊 ¡WOW! ¡LO LOGRASTE! 🎊             ║
    ║                                           ║
    ║   Sí que eres CURIOSO/A, ¿eh? 😏          ║
    ║                                           ║
    ║   Has llegado hasta aquí después de       ║
    ║   abrir como 10 carpetas...               ║
    ║                                           ║
    ║   Se nota que te gusta el CHISME 🫖       ║
    ║                                           ║
    ║   Pero bueno, ya que estás aquí te        ║
    ║   cuento un secreto:                      ║
    ║                                           ║
    ║   ✨ José es un crack programando ✨       ║
    ║                                           ║
    ║   Y tú eres crack por llegar hasta        ║
    ║   aquí. ¡Contáctalo! 🚀                   ║
    ║                                           ║
    ║               😄 🎉 🏆                     ║
    ║                                           ║
    ╚═══════════════════════════════════════════╝
              </pre>
            </div>
          </template>
        </template>
        
        <!-- Archivo: plan.txt -->
        <template v-else-if="path.includes('plan.txt')">
          <div class="text-file-content">
            <pre>╔══════════════════════════════════════╗
║   PLAN PARA CONQUISTAR EL MUNDO      ║
║          (Top Secret 🤫)             ║
╠══════════════════════════════════════╣
║                                      ║
║  Paso 1: Aprender a programar ✅     ║
║                                      ║
║  Paso 2: Hacer un portafolio cool ✅ ║
║                                      ║
║  Paso 3: Conseguir café ☕            ║
║          (en progreso...)            ║
║                                      ║
║  Paso 4: ???                         ║
║                                      ║
║  Paso 5: ¡Conquistar el mundo! 🌍    ║
║                                      ║
║  Nota: Si estás leyendo esto,        ║
║  probablemente deberías              ║
║  contratarme 😄                      ║
║                                      ║
╚══════════════════════════════════════╝</pre>
          </div>
        </template>
      </div>
      
      <!-- Finder Status Bar -->
      <div class="finder-statusbar">
        <span v-if="path.length === 0">4 elementos</span>
        <span v-else-if="path.includes('plan.txt')">plan.txt - 420 bytes</span>
        <span v-else-if="path.includes('carta_secreta.txt')">carta_secreta.txt - 1337 bytes 🏆</span>
        <span v-else>1 elemento</span>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref, computed } from 'vue'
import { 
  CloseOutlined, 
  MinusOutlined, 
  ExpandOutlined, 
  FolderOutlined,
  FileTextOutlined 
} from '@ant-design/icons-vue'

const props = defineProps({
  state: {
    type: String,
    default: 'closed',
    validator: (value) => ['closed', 'normal', 'minimized', 'maximized'].includes(value)
  }
})

defineEmits(['close', 'minimize', 'maximize', 'open-ia', 'open-marciano'])

const path = ref([])

const isVisible = computed(() => props.state !== 'closed' && props.state !== 'minimized')

const currentTitle = computed(() => {
  if (path.value.length === 0) return 'Proyectos'
  return path.value[path.value.length - 1]
})

const navigate = (item) => {
  path.value = [...path.value, item]
}

const goBack = () => {
  path.value = path.value.slice(0, -1)
}
</script>

<style scoped>
.finder-window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 380px;
  height: 320px;
  background: var(--glass-bg, rgba(30, 30, 30, 0.95));
  backdrop-filter: blur(20px);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
  z-index: 10;
  display: flex;
  flex-direction: column;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.finder-window.maximized {
  width: 450px;
  height: 400px;
}

.window-header {
  background: rgba(0, 0, 0, 0.4);
  padding: 12px 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.finder-header {
  background: rgba(60, 60, 60, 0.95) !important;
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

.dot:hover .dot-icon {
  opacity: 1;
}

.dot.red { background: #ff5f56; }
.dot.yellow { background: #ffbd2e; }
.dot.green { background: #27ca40; }

.dot.red:hover { 
  background: #ff3b30;
  transform: scale(1.2);
}
.dot.yellow:hover { 
  background: #ff9500;
  transform: scale(1.2);
}
.dot.green:hover { 
  background: #34c759;
  transform: scale(1.2);
}

.finder-toolbar {
  background: rgba(50, 50, 50, 0.9);
  padding: 8px 12px;
  display: flex;
  align-items: center;
  gap: 8px;
  border-bottom: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
}

.finder-nav-btn {
  width: 24px;
  height: 24px;
  border: none;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s ease;
}

.finder-nav-btn:hover:not(:disabled) {
  background: rgba(255, 255, 255, 0.2);
}

.finder-nav-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.finder-breadcrumb {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.75rem;
  color: var(--color-text-muted, #888);
  margin-left: 8px;
}

.finder-breadcrumb span {
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.finder-breadcrumb span:hover {
  background: rgba(255, 255, 255, 0.1);
  color: white;
}

.breadcrumb-separator {
  color: var(--color-text-muted, #888);
  cursor: default !important;
}

.breadcrumb-separator:hover {
  background: transparent !important;
}

.finder-content {
  flex: 1;
  padding: 16px;
  overflow-y: auto;
  background: rgba(30, 30, 30, 0.9);
}

.finder-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 14px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-bottom: 4px;
}

.finder-item:hover {
  background: rgba(24, 144, 255, 0.2);
}

.finder-item.folder .finder-icon {
  color: #5ac8fa;
  font-size: 1.8rem;
}

.finder-item.file .finder-icon {
  color: #a0a0a0;
  font-size: 1.6rem;
}

.finder-item span {
  font-size: 0.85rem;
  color: white;
}

/* Folder Colors */
.folder-blue .finder-icon { color: #5ac8fa !important; }
.folder-purple .finder-icon { color: #bf5af2 !important; }
.folder-red .finder-icon { color: #ff453a !important; }
.folder-green .finder-icon { color: #32d74b !important; }
.folder-orange .finder-icon { color: #ff9f0a !important; }
.folder-yellow .finder-icon { color: #ffd60a !important; }
.folder-pink .finder-icon { color: #ff6482 !important; }
.folder-cyan .finder-icon { color: #64d2ff !important; }
.folder-gold .finder-icon { color: #ffd700 !important; }

.ia-file {
  background: linear-gradient(135deg, rgba(191, 90, 242, 0.2) 0%, rgba(94, 92, 230, 0.2) 100%);
}

.ia-file:hover {
  background: linear-gradient(135deg, rgba(191, 90, 242, 0.4) 0%, rgba(94, 92, 230, 0.4) 100%) !important;
}

.game-file {
  background: linear-gradient(135deg, rgba(50, 215, 75, 0.15) 0%, rgba(40, 167, 69, 0.15) 100%);
}

.game-file.snake:hover {
  background: linear-gradient(135deg, rgba(50, 215, 75, 0.3) 0%, rgba(40, 167, 69, 0.3) 100%) !important;
}

.game-file.flappy {
  background: linear-gradient(135deg, rgba(255, 214, 10, 0.15) 0%, rgba(255, 159, 10, 0.15) 100%);
}

.game-file.flappy:hover {
  background: linear-gradient(135deg, rgba(255, 214, 10, 0.3) 0%, rgba(255, 159, 10, 0.3) 100%) !important;
}

.robot-emoji,
.alien-emoji,
.game-emoji {
  font-size: 1.8rem;
}

.text-file-content {
  background: rgba(20, 20, 20, 0.95);
  border-radius: 8px;
  padding: 16px;
  height: 100%;
  overflow: auto;
}

.text-file-content pre {
  margin: 0;
  font-family: 'Fira Code', monospace;
  font-size: 0.7rem;
  color: #00ff00;
  line-height: 1.4;
  white-space: pre;
}

.finder-statusbar {
  background: rgba(50, 50, 50, 0.9);
  padding: 6px 12px;
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
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
  100% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
}

@keyframes windowClose {
  0% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.8);
  }
}
</style>
