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
        <span class="window-title">🐦 Flappy Bird</span>
      </div>
      
      <div class="game-content" @click="handleGameClick">
        <div class="game-header">
          <span class="score">Score: {{ score }}</span>
          <span class="high-score">High: {{ highScore }}</span>
        </div>
        
        <canvas ref="gameCanvas" width="280" height="320" class="game-canvas"></canvas>
        
        <div v-if="gameOver" class="game-overlay" @click.stop>
          <div class="game-over-text">Game Over!</div>
          <div class="final-score">Score: {{ score }}</div>
          <button class="play-btn" @click.stop="startGame">🔄 Play Again</button>
        </div>
        
        <div v-if="!gameStarted && !gameOver" class="game-overlay" @click.stop>
          <div class="start-text">🐦 Flappy Bird</div>
          <button class="play-btn" @click.stop="startGame">▶ Start</button>
          <div class="controls-hint">Click or Space to flap</div>
        </div>
        
        <div v-if="isPaused && gameStarted && !gameOver" class="game-overlay" @click.stop>
          <div class="pause-text">⏸ Paused</div>
          <button class="play-btn" @click.stop="togglePause">▶ Resume</button>
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
let bird = { x: 50, y: 160, velocity: 0, width: 24, height: 18 }
let pipes = []
let animationId = null
// Easier settings: slower gravity, gentler jump, slower pipes, wider gap
const gravity = 0.25
const jumpForce = -5
const pipeWidth = 40
const pipeGap = 130
const pipeSpeed = 1.5

const initCanvas = () => {
  if (gameCanvas.value) {
    ctx = gameCanvas.value.getContext('2d')
    drawInitialState()
  }
}

const drawInitialState = () => {
  if (!ctx) return
  
  // Sky gradient
  const skyGradient = ctx.createLinearGradient(0, 0, 0, 320)
  skyGradient.addColorStop(0, '#1a1a2e')
  skyGradient.addColorStop(0.5, '#16213e')
  skyGradient.addColorStop(1, '#0f3460')
  ctx.fillStyle = skyGradient
  ctx.fillRect(0, 0, 280, 320)
  
  // Ground
  ctx.fillStyle = '#533483'
  ctx.fillRect(0, 310, 280, 10)
  
  // Draw idle bird
  drawBird(50, 160, 0)
}

const drawBird = (x, y, velocity) => {
  if (!ctx) return
  
  ctx.save()
  ctx.translate(x + 12, y + 9)
  ctx.rotate(Math.min(velocity * 0.04, 0.4))
  
  // Body
  ctx.fillStyle = '#ffd60a'
  ctx.beginPath()
  ctx.ellipse(0, 0, 12, 9, 0, 0, Math.PI * 2)
  ctx.fill()
  
  // Wing
  ctx.fillStyle = '#ff9f0a'
  ctx.beginPath()
  ctx.ellipse(-2, 2, 8, 5, 0.3, 0, Math.PI * 2)
  ctx.fill()
  
  // Eye
  ctx.fillStyle = 'white'
  ctx.beginPath()
  ctx.arc(6, -3, 5, 0, Math.PI * 2)
  ctx.fill()
  ctx.fillStyle = '#1a1a2e'
  ctx.beginPath()
  ctx.arc(7, -3, 2.5, 0, Math.PI * 2)
  ctx.fill()
  
  // Beak
  ctx.fillStyle = '#ff453a'
  ctx.beginPath()
  ctx.moveTo(10, 0)
  ctx.lineTo(18, 2)
  ctx.lineTo(10, 5)
  ctx.closePath()
  ctx.fill()
  
  ctx.restore()
}

const startGame = () => {
  bird = { x: 50, y: 160, velocity: 0, width: 24, height: 18 }
  pipes = []
  score.value = 0
  gameOver.value = false
  gameStarted.value = true
  isPaused.value = false
  
  // Cancel any existing animation
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  
  // Spawn first pipe after a delay
  setTimeout(() => {
    if (gameStarted.value && !gameOver.value) {
      spawnPipe()
    }
  }, 1500)
  
  // Start game loop
  gameLoop()
}

const spawnPipe = () => {
  const minHeight = 50
  const maxHeight = 160
  const topHeight = Math.random() * (maxHeight - minHeight) + minHeight
  
  pipes.push({
    x: 300,
    topHeight: topHeight,
    bottomY: topHeight + pipeGap,
    passed: false
  })
}

const gameLoop = () => {
  if (!gameStarted.value || gameOver.value) return
  
  if (!isPaused.value) {
    update()
  }
  
  animationId = requestAnimationFrame(gameLoop)
}

const update = () => {
  // Bird physics
  bird.velocity += gravity
  bird.y += bird.velocity
  
  // Ground collision
  if (bird.y + bird.height > 310) {
    bird.y = 310 - bird.height
    endGame()
    return
  }
  
  // Ceiling collision
  if (bird.y < 0) {
    bird.y = 0
    bird.velocity = 0
  }
  
  // Move and check pipes
  for (let i = pipes.length - 1; i >= 0; i--) {
    pipes[i].x -= pipeSpeed
    
    // Collision detection with bird hitbox
    const birdRight = bird.x + bird.width - 4
    const birdLeft = bird.x + 4
    const birdTop = bird.y + 4
    const birdBottom = bird.y + bird.height - 4
    
    const pipeLeft = pipes[i].x
    const pipeRight = pipes[i].x + pipeWidth
    
    if (birdRight > pipeLeft && birdLeft < pipeRight) {
      if (birdTop < pipes[i].topHeight || birdBottom > pipes[i].bottomY) {
        endGame()
        return
      }
    }
    
    // Score point
    if (!pipes[i].passed && pipes[i].x + pipeWidth < bird.x) {
      pipes[i].passed = true
      score.value++
    }
    
    // Remove off-screen pipes
    if (pipes[i].x + pipeWidth < 0) {
      pipes.splice(i, 1)
    }
  }
  
  // Spawn new pipes - increased spacing for easier game
  if (pipes.length === 0 || (pipes.length > 0 && pipes[pipes.length - 1].x < 100)) {
    spawnPipe()
  }
  
  draw()
}

const draw = () => {
  if (!ctx) return
  
  // Sky gradient
  const skyGradient = ctx.createLinearGradient(0, 0, 0, 320)
  skyGradient.addColorStop(0, '#1a1a2e')
  skyGradient.addColorStop(0.5, '#16213e')
  skyGradient.addColorStop(1, '#0f3460')
  ctx.fillStyle = skyGradient
  ctx.fillRect(0, 0, 280, 320)
  
  // Stars
  ctx.fillStyle = 'rgba(255, 255, 255, 0.5)'
  for (let i = 0; i < 15; i++) {
    ctx.beginPath()
    ctx.arc((i * 47 + 10) % 280, (i * 23 + 5) % 180, 1, 0, Math.PI * 2)
    ctx.fill()
  }
  
  // Draw pipes
  pipes.forEach(pipe => {
    // Pipe gradient
    const pipeGradient = ctx.createLinearGradient(pipe.x, 0, pipe.x + pipeWidth, 0)
    pipeGradient.addColorStop(0, '#32d74b')
    pipeGradient.addColorStop(0.5, '#28a745')
    pipeGradient.addColorStop(1, '#1e7e34')
    
    ctx.fillStyle = pipeGradient
    
    // Top pipe
    ctx.fillRect(pipe.x, 0, pipeWidth, pipe.topHeight)
    // Top pipe cap
    ctx.fillRect(pipe.x - 3, pipe.topHeight - 20, pipeWidth + 6, 20)
    
    // Bottom pipe
    ctx.fillRect(pipe.x, pipe.bottomY, pipeWidth, 320 - pipe.bottomY)
    // Bottom pipe cap
    ctx.fillRect(pipe.x - 3, pipe.bottomY, pipeWidth + 6, 20)
  })
  
  // Draw bird
  drawBird(bird.x, bird.y, bird.velocity)
  
  // Ground
  ctx.fillStyle = '#533483'
  ctx.fillRect(0, 310, 280, 10)
}

const flap = () => {
  if (!gameStarted.value || gameOver.value || isPaused.value) return
  bird.velocity = jumpForce
}

const endGame = () => {
  gameOver.value = true
  gameStarted.value = false
  if (score.value > highScore.value) {
    highScore.value = score.value
  }
  if (animationId) {
    cancelAnimationFrame(animationId)
    animationId = null
  }
}

const togglePause = () => {
  if (gameStarted.value && !gameOver.value) {
    isPaused.value = !isPaused.value
    if (!isPaused.value) {
      gameLoop()
    }
  }
}

const handleGameClick = (e) => {
  // Only flap if clicking on canvas area, not overlays
  if (gameStarted.value && !gameOver.value && !isPaused.value) {
    flap()
  }
}

const handleKeydown = (e) => {
  if (!isVisible.value) return
  
  if (e.code === 'Space' || e.key === ' ') {
    e.preventDefault()
    if (gameStarted.value && !gameOver.value && !isPaused.value) {
      flap()
    }
  }
}

watch(isVisible, async (newVal) => {
  if (newVal) {
    await nextTick()
    initCanvas()
  } else {
    // Clean up when hidden
    if (animationId) {
      cancelAnimationFrame(animationId)
      animationId = null
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
  if (animationId) cancelAnimationFrame(animationId)
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
  cursor: pointer;
}

.game-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  font-size: 0.8rem;
  color: white;
}

.score { color: #ffd60a; font-weight: 600; }
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
  cursor: default;
}

.game-over-text, .start-text, .pause-text {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  margin-bottom: 8px;
}

.final-score {
  color: #ffd60a;
  font-size: 1.1rem;
  margin-bottom: 16px;
}

.play-btn {
  background: linear-gradient(135deg, #ffd60a 0%, #ff9f0a 100%);
  border: none;
  color: #1a1a2e;
  padding: 10px 24px;
  border-radius: 20px;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 600;
}

.play-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px rgba(255, 214, 10, 0.4);
}

.controls-hint {
  margin-top: 12px;
  font-size: 0.75rem;
  color: var(--color-text-muted, #888);
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
