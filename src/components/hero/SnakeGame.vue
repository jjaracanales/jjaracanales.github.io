<template>
  <Transition name="window">
    <div v-if="isVisible" class="game-window" @click.stop>
      <div class="window-header">
        <span class="dot red" @click="$emit('close')" title="Cerrar">
          <CloseOutlined class="dot-icon" />
        </span>
        <span class="dot yellow" @click="$emit('minimize')" title="Minimizar">
          <MinusOutlined class="dot-icon" />
        </span>
        <span class="dot green" @click="togglePause" title="Pausar">
          <ExpandOutlined class="dot-icon" />
        </span>
        <span class="window-title">🐍 Snake</span>
      </div>
      
      <div class="game-content">
        <div class="game-header">
          <span class="score">Score: {{ score }}</span>
          <span class="high-score">High: {{ highScore }}</span>
        </div>
        
        <canvas ref="gameCanvas" width="280" height="280" class="game-canvas"></canvas>
        
        <div v-if="gameOver" class="game-overlay">
          <div class="game-over-text">Game Over!</div>
          <div class="final-score">Score: {{ score }}</div>
          <button class="play-btn" @click="startGame">🔄 Play Again</button>
        </div>
        
        <div v-if="!gameStarted && !gameOver" class="game-overlay">
          <div class="start-text">🐍 Snake</div>
          <button class="play-btn" @click="startGame">▶ Start</button>
          <div class="controls-hint">Use ← ↑ → ↓ keys</div>
        </div>
        
        <div v-if="isPaused && gameStarted && !gameOver" class="game-overlay">
          <div class="pause-text">⏸ Paused</div>
          <button class="play-btn" @click="togglePause">▶ Resume</button>
        </div>
        
        <!-- Mobile Controls -->
        <div class="mobile-controls">
          <button class="control-btn up" @click="changeDirection('up')">↑</button>
          <div class="control-row">
            <button class="control-btn left" @click="changeDirection('left')">←</button>
            <button class="control-btn down" @click="changeDirection('down')">↓</button>
            <button class="control-btn right" @click="changeDirection('right')">→</button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch, nextTick } from 'vue'
import { CloseOutlined, MinusOutlined, ExpandOutlined } from '@ant-design/icons-vue'

const props = defineProps({
  state: {
    type: String,
    default: 'closed'
  }
})

defineEmits(['close', 'minimize', 'maximize'])

const isVisible = computed(() => props.state === 'normal' || props.state === 'maximized')

const gameCanvas = ref(null)
const score = ref(0)
const highScore = ref(0)
const gameOver = ref(false)
const gameStarted = ref(false)
const isPaused = ref(false)

let ctx = null
let snake = []
let food = { x: 0, y: 0 }
let direction = 'right'
let nextDirection = 'right'
let gameLoop = null
const gridSize = 14
const tileCount = 20

const initCanvas = () => {
  if (gameCanvas.value) {
    ctx = gameCanvas.value.getContext('2d')
    drawInitialState()
  }
}

const drawInitialState = () => {
  if (!ctx) return
  // Draw initial empty canvas
  ctx.fillStyle = '#1a1a2e'
  ctx.fillRect(0, 0, 280, 280)
  
  // Draw grid
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)'
  for (let i = 0; i < tileCount; i++) {
    ctx.beginPath()
    ctx.moveTo(i * gridSize, 0)
    ctx.lineTo(i * gridSize, 280)
    ctx.stroke()
    ctx.beginPath()
    ctx.moveTo(0, i * gridSize)
    ctx.lineTo(280, i * gridSize)
    ctx.stroke()
  }
}

const startGame = () => {
  // Initialize snake in the middle of the grid
  snake = [
    { x: 10, y: 10 },
    { x: 9, y: 10 },
    { x: 8, y: 10 }
  ]
  direction = 'right'
  nextDirection = 'right'
  score.value = 0
  gameOver.value = false
  gameStarted.value = true
  isPaused.value = false
  
  // Spawn food
  spawnFood()
  
  // Draw initial state
  if (ctx) {
    draw()
  }
  
  // Clear any existing loop
  if (gameLoop) {
    clearInterval(gameLoop)
  }
  
  // Start game loop with slight delay to ensure everything is initialized
  setTimeout(() => {
    gameLoop = setInterval(update, 120)
  }, 100)
}

const spawnFood = () => {
  let validPosition = false
  while (!validPosition) {
    food.x = Math.floor(Math.random() * tileCount)
    food.y = Math.floor(Math.random() * tileCount)
    
    validPosition = true
    // Make sure food doesn't spawn on snake
    for (let segment of snake) {
      if (segment.x === food.x && segment.y === food.y) {
        validPosition = false
        break
      }
    }
  }
}

const update = () => {
  if (isPaused.value || gameOver.value || !gameStarted.value) return
  
  direction = nextDirection
  
  const head = { x: snake[0].x, y: snake[0].y }
  
  switch (direction) {
    case 'up': head.y--; break
    case 'down': head.y++; break
    case 'left': head.x--; break
    case 'right': head.x++; break
  }
  
  // Check wall collision
  if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
    endGame()
    return
  }
  
  // Check self collision (skip the tail as it will move)
  for (let i = 0; i < snake.length - 1; i++) {
    if (snake[i].x === head.x && snake[i].y === head.y) {
      endGame()
      return
    }
  }
  
  snake.unshift(head)
  
  // Check food collision
  if (head.x === food.x && head.y === food.y) {
    score.value += 10
    spawnFood()
  } else {
    snake.pop()
  }
  
  draw()
}

const draw = () => {
  if (!ctx) return
  
  // Clear canvas
  ctx.fillStyle = '#1a1a2e'
  ctx.fillRect(0, 0, 280, 280)
  
  // Draw grid
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)'
  for (let i = 0; i < tileCount; i++) {
    ctx.beginPath()
    ctx.moveTo(i * gridSize, 0)
    ctx.lineTo(i * gridSize, 280)
    ctx.stroke()
    ctx.beginPath()
    ctx.moveTo(0, i * gridSize)
    ctx.lineTo(280, i * gridSize)
    ctx.stroke()
  }
  
  // Draw food
  ctx.fillStyle = '#ff453a'
  ctx.beginPath()
  ctx.arc(
    food.x * gridSize + gridSize / 2,
    food.y * gridSize + gridSize / 2,
    gridSize / 2 - 2,
    0,
    Math.PI * 2
  )
  ctx.fill()
  
  // Draw snake
  snake.forEach((segment, index) => {
    if (index === 0) {
      // Head - brighter green
      ctx.fillStyle = '#32d74b'
    } else {
      // Body - darker green
      ctx.fillStyle = '#28a745'
    }
    
    ctx.fillRect(
      segment.x * gridSize + 1,
      segment.y * gridSize + 1,
      gridSize - 2,
      gridSize - 2
    )
    
    // Eyes for head
    if (index === 0) {
      ctx.fillStyle = 'white'
      let eyeX1 = segment.x * gridSize + 4
      let eyeX2 = segment.x * gridSize + 10
      let eyeY = segment.y * gridSize + 5
      
      ctx.beginPath()
      ctx.arc(eyeX1, eyeY, 2, 0, Math.PI * 2)
      ctx.arc(eyeX2, eyeY, 2, 0, Math.PI * 2)
      ctx.fill()
    }
  })
}

const endGame = () => {
  gameOver.value = true
  gameStarted.value = false
  if (score.value > highScore.value) {
    highScore.value = score.value
  }
  if (gameLoop) {
    clearInterval(gameLoop)
    gameLoop = null
  }
}

const togglePause = () => {
  if (gameStarted.value && !gameOver.value) {
    isPaused.value = !isPaused.value
  }
}

const changeDirection = (newDir) => {
  if (!gameStarted.value || gameOver.value || isPaused.value) return
  
  const opposites = { up: 'down', down: 'up', left: 'right', right: 'left' }
  if (newDir !== opposites[direction]) {
    nextDirection = newDir
  }
}

const handleKeydown = (e) => {
  if (!isVisible.value || !gameStarted.value || gameOver.value || isPaused.value) return
  
  const keyMap = {
    ArrowUp: 'up',
    ArrowDown: 'down',
    ArrowLeft: 'left',
    ArrowRight: 'right'
  }
  
  if (keyMap[e.key]) {
    e.preventDefault()
    changeDirection(keyMap[e.key])
  }
}

watch(isVisible, async (newVal) => {
  if (newVal) {
    await nextTick()
    initCanvas()
  } else {
    // Clean up when hidden
    if (gameLoop) {
      clearInterval(gameLoop)
      gameLoop = null
    }
    gameStarted.value = false
    gameOver.value = false
  }
})

onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
  if (gameLoop) clearInterval(gameLoop)
})
</script>

<style scoped>
.game-window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: var(--glass-bg, rgba(30, 30, 30, 0.95));
  backdrop-filter: blur(20px);
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  border: 1px solid var(--glass-border, rgba(255, 255, 255, 0.1));
  z-index: 15;
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

.dot:hover .dot-icon { opacity: 1; }
.dot.red { background: #ff5f56; }
.dot.yellow { background: #ffbd2e; }
.dot.green { background: #27ca40; }

.dot.red:hover { background: #ff3b30; transform: scale(1.2); }
.dot.yellow:hover { background: #ff9500; transform: scale(1.2); }
.dot.green:hover { background: #34c759; transform: scale(1.2); }

.game-content {
  padding: 12px;
  position: relative;
}

.game-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  font-size: 0.8rem;
  color: white;
}

.score { color: #32d74b; font-weight: 600; }
.high-score { color: #ff9f0a; }

.game-canvas {
  display: block;
  border-radius: 8px;
  border: 2px solid rgba(255, 255, 255, 0.1);
}

.game-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  background: rgba(0, 0, 0, 0.8);
  padding: 24px 32px;
  border-radius: 12px;
  z-index: 10;
}

.game-over-text, .start-text, .pause-text {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  margin-bottom: 8px;
}

.final-score {
  color: #32d74b;
  font-size: 1.1rem;
  margin-bottom: 16px;
}

.play-btn {
  background: linear-gradient(135deg, #32d74b 0%, #28a745 100%);
  border: none;
  color: white;
  padding: 10px 24px;
  border-radius: 20px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 600;
}

.play-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px rgba(50, 215, 75, 0.4);
}

.controls-hint {
  margin-top: 12px;
  font-size: 0.75rem;
  color: var(--color-text-muted, #888);
}

.mobile-controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 12px;
  gap: 4px;
}

.control-row {
  display: flex;
  gap: 4px;
}

.control-btn {
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  color: white;
  font-size: 1.2rem;
  cursor: pointer;
  transition: all 0.2s ease;
}

.control-btn:hover {
  background: var(--color-accent, #1890ff);
}

.control-btn:active {
  transform: scale(0.95);
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
