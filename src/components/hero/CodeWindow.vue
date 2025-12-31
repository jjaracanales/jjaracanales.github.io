<template>
  <Transition name="window">
    <div v-if="isVisible" 
         class="code-window" 
         :class="{ 'maximized': state === 'maximized' }">
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
        <span class="window-title">developer.js</span>
      </div>
      
      <!-- Window Content -->
      <div class="window-content">
        <div class="line-numbers">
          <span v-for="n in (state === 'maximized' ? 12 : 7)" :key="n">{{ n }}</span>
        </div>
        <pre><code><span class="keyword">const</span> <span class="variable">developer</span> = {
  <span class="property">name</span>: <span class="string">"José Jara"</span>,
  <span class="property">role</span>: <span class="string">"Full-Stack"</span>,
  <span class="property">company</span>: <span class="string">"Plutonia"</span>,
  <span class="property">apps</span>: <span class="number">12</span>,
  <span class="property">projects</span>: <span class="number">30</span>+,
  <span class="property">passion</span>: <span class="string">"∞"</span>
};<template v-if="state === 'maximized'">

<span class="comment">// 🚀 Skills</span>
<span class="keyword">const</span> <span class="variable">skills</span> = [
  <span class="string">"Vue"</span>, <span class="string">"React"</span>, <span class="string">"Node"</span>
];</template></code></pre>
      </div>
      
      <!-- Maximized extra info -->
      <div v-if="state === 'maximized'" class="window-footer">
        <span>📍 Santiago, Chile</span>
        <span>💼 CEO @ Plutonia SpA</span>
        <span>⚡ Modo: Máximo rendimiento</span>
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

defineEmits(['close', 'minimize', 'maximize'])

const isVisible = computed(() => props.state === 'normal' || props.state === 'maximized')
</script>

<style scoped>
.code-window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: var(--glass-bg, rgba(30, 30, 30, 0.95));
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
  border-radius: 12px;
  overflow: hidden;
  width: 380px;
  height: 280px;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.4);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.code-window.maximized {
  width: 420px;
  height: 380px;
  box-shadow: 0 35px 70px rgba(24, 144, 255, 0.3);
}

.window-header {
  background: rgba(0, 0, 0, 0.4);
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

.window-content {
  padding: 24px;
  display: flex;
  gap: 16px;
}

.line-numbers {
  display: flex;
  flex-direction: column;
  color: var(--color-text-muted, #888);
  font-family: 'Fira Code', 'Monaco', monospace;
  font-size: 0.9rem;
  line-height: 1.6;
  user-select: none;
  opacity: 0.5;
}

.window-content pre {
  margin: 0;
  font-family: 'Fira Code', 'Monaco', monospace;
  font-size: 0.9rem;
  line-height: 1.6;
}

.keyword { color: #c678dd; }
.variable { color: #e06c75; }
.property { color: #61afef; }
.string { color: #98c379; }
.number { color: #d19a66; }
.comment { color: #5c6370; font-style: italic; }

.window-footer {
  background: rgba(0, 0, 0, 0.3);
  padding: 8px 16px;
  display: flex;
  justify-content: space-between;
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
