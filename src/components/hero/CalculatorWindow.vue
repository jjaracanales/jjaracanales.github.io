<template>
  <Transition name="window">
    <div v-if="isVisible" 
         class="calculator-window" 
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
        <span class="window-title">Calculadora</span>
      </div>
      
      <div class="calc-display">{{ display }}</div>
      <div class="calc-buttons">
        <button class="calc-btn clear" @click="clear">C</button>
        <button class="calc-btn operator" @click="input('/')">÷</button>
        <button class="calc-btn operator" @click="input('*')">×</button>
        <button class="calc-btn operator" @click="backspace">⌫</button>
        <button class="calc-btn" @click="input('7')">7</button>
        <button class="calc-btn" @click="input('8')">8</button>
        <button class="calc-btn" @click="input('9')">9</button>
        <button class="calc-btn operator" @click="input('-')">−</button>
        <button class="calc-btn" @click="input('4')">4</button>
        <button class="calc-btn" @click="input('5')">5</button>
        <button class="calc-btn" @click="input('6')">6</button>
        <button class="calc-btn operator" @click="input('+')">+</button>
        <button class="calc-btn" @click="input('1')">1</button>
        <button class="calc-btn" @click="input('2')">2</button>
        <button class="calc-btn" @click="input('3')">3</button>
        <button class="calc-btn equals" @click="calculate">=</button>
        <button class="calc-btn zero" @click="input('0')">0</button>
        <button class="calc-btn" @click="input('.')">.</button>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref, computed } from 'vue'
import { CloseOutlined, MinusOutlined, ExpandOutlined } from '@ant-design/icons-vue'

const props = defineProps({
  state: {
    type: String,
    default: 'closed',
    validator: (value) => ['closed', 'normal', 'minimized', 'maximized'].includes(value)
  }
})

defineEmits(['close', 'minimize', 'maximize'])

const isVisible = computed(() => props.state !== 'closed' && props.state !== 'minimized')

const display = ref('0')

const input = (value) => {
  if (display.value === '0' && !isNaN(value)) {
    display.value = value
  } else if (display.value === 'Error') {
    display.value = value
  } else {
    display.value += value
  }
}

const clear = () => {
  display.value = '0'
}

const backspace = () => {
  if (display.value.length > 1) {
    display.value = display.value.slice(0, -1)
  } else {
    display.value = '0'
  }
}

const calculate = () => {
  try {
    const result = eval(display.value)
    display.value = String(result)
  } catch (e) {
    display.value = 'Error'
  }
}
</script>

<style scoped>
.calculator-window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 240px;
  background: var(--glass-bg, rgba(30, 30, 30, 0.95));
  backdrop-filter: blur(20px);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
  z-index: 10;
}

.calculator-window.maximized {
  width: 300px;
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

.calc-display {
  background: rgba(0, 0, 0, 0.5);
  padding: 20px 16px;
  text-align: right;
  font-family: 'Fira Code', monospace;
  font-size: 2rem;
  color: white;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  min-height: 70px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.calculator-window.maximized .calc-display {
  font-size: 2.5rem;
  padding: 24px 20px;
  min-height: 90px;
}

.calc-buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2px;
  padding: 2px;
  background: rgba(0, 0, 0, 0.4);
}

.calc-btn {
  padding: 14px;
  border: none;
  background: rgba(80, 80, 80, 0.9);
  color: white;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.15s ease;
  font-family: inherit;
  font-weight: 500;
}

.calculator-window.maximized .calc-btn {
  padding: 18px;
  font-size: 1.3rem;
}

.calc-btn:hover {
  background: rgba(100, 100, 100, 0.95);
}

.calc-btn:active {
  background: rgba(120, 120, 120, 0.95);
  transform: scale(0.95);
}

.calc-btn.operator {
  background: rgba(255, 159, 10, 0.95);
  font-weight: 600;
}

.calc-btn.operator:hover {
  background: rgba(255, 179, 64, 1);
}

.calc-btn.clear {
  background: rgba(165, 165, 165, 0.95);
  color: #1c1c1c;
  font-weight: 600;
}

.calc-btn.clear:hover {
  background: rgba(200, 200, 200, 1);
}

.calc-btn.equals {
  grid-row: span 2;
  background: var(--color-accent, #1890ff);
  font-weight: 600;
}

.calc-btn.equals:hover {
  background: rgba(64, 169, 255, 1);
}

.calc-btn.zero {
  grid-column: span 2;
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
