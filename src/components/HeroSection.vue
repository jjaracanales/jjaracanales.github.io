<template>
  <section id="hero" class="hero-section">
    <!-- Animated Background -->
    <div class="hero-bg">
      <!-- Grid Lines -->
      <div class="grid-lines"></div>
      
      <!-- Floating Particles -->
      <div class="hero-particles">
        <div v-for="n in 30" :key="n" class="particle" :style="getParticleStyle(n)"></div>
      </div>
      
      <!-- Gradient Orbs -->
      <div class="gradient-orb orb-1"></div>
      <div class="gradient-orb orb-2"></div>
      <div class="gradient-orb orb-3"></div>
    </div>
    
    <div class="container hero-content">
      <div class="hero-text">
        <p class="hero-greeting animate-fadeInUp">
          <span class="greeting-icon">👋</span> Hola, soy
        </p>
        <h1 class="hero-name animate-fadeInUp" style="animation-delay: 0.1s">
          <span class="name-text" data-text="José Jara Canales">José Jara Canales</span>
        </h1>
        <h2 class="hero-title animate-fadeInUp" style="animation-delay: 0.2s">
          <span class="typing-text">{{ displayedText }}</span>
          <span class="cursor">|</span>
        </h2>
        <p class="hero-description animate-fadeInUp" style="animation-delay: 0.3s">
          CEO & Full-Stack Developer en Plutonia SpA. 
          +30 proyectos completados, 12 apps publicadas en Play Store.
          Especializado en soluciones web, mobile y e-commerce.
        </p>
        
        <!-- Stats Cards -->
        <div class="hero-stats animate-fadeInUp" style="animation-delay: 0.35s">
          <div class="stat-card">
            <span class="stat-value">30+</span>
            <span class="stat-label">Proyectos</span>
          </div>
          <div class="stat-card">
            <span class="stat-value">12</span>
            <span class="stat-label">Apps</span>
          </div>
          <div class="stat-card">
            <span class="stat-value">5+</span>
            <span class="stat-label">Años Exp.</span>
          </div>
        </div>
        
        <div class="hero-buttons animate-fadeInUp" style="animation-delay: 0.4s">
          <a href="#projects" class="btn-primary btn-glow" aria-label="Ver mis proyectos">
            <ProjectOutlined /> Ver Proyectos
          </a>
          <a href="/cv-jose-jara.pdf" target="_blank" rel="noopener noreferrer" class="btn-secondary" aria-label="Descargar mi currículum vitae en PDF">
            <DownloadOutlined /> Descargar CV
          </a>
        </div>
      </div>
      
      <div class="hero-visual animate-fadeInUp" style="animation-delay: 0.5s">
        <div class="hero-image">
          <!-- Orbital Rings -->
          <div class="orbital-ring ring-1"></div>
          <div class="orbital-ring ring-2"></div>
          <div class="orbital-ring ring-3"></div>
          
          <div class="image-glow"></div>
          
          <!-- Mac Desktop -->
          <MacDesktop
            :windowState="windowState"
            :calcState="calcState"
            :finderState="finderState"
            :gamesState="gamesState"
            :snakeState="snakeState"
            :flappyState="flappyState"
            @open-window="openWindow"
            @open-finder="openFinder"
            @open-calculator="openCalculator"
            @open-github="openGitHub"
            @open-linkedin="openLinkedIn"
            @open-email="openEmail"
            @open-games="openGames"
            @open-snake="openSnake"
            @open-flappy="openFlappy"
          />
          
          <!-- Code Window -->
          <CodeWindow
            :state="windowState"
            @close="closeWindow"
            @minimize="minimizeWindow"
            @maximize="maximizeWindow"
          />
          
          <!-- Calculator Window -->
          <CalculatorWindow
            :state="calcState"
            @close="closeCalculator"
            @minimize="minimizeCalculator"
            @maximize="maximizeCalculator"
          />
          
          <!-- Finder Window -->
          <FinderWindow
            :state="finderState"
            @close="closeFinder"
            @minimize="minimizeFinder"
            @maximize="maximizeFinder"
            @open-ia="showIATierra = true"
            @open-marciano="showMarciano = true"
          />
          
          <!-- Games Folder -->
          <GamesFolder
            :state="gamesState"
            @close="closeGames"
            @minimize="minimizeGames"
            @maximize="maximizeGames"
            @open-snake="openSnake"
            @open-flappy="openFlappy"
          />
          
          <!-- Snake Game -->
          <SnakeGame
            :state="snakeState"
            @close="closeSnake"
            @minimize="minimizeSnake"
            @maximize="maximizeSnake"
          />
          
          <!-- Flappy Bird Game -->
          <FlappyBirdGame
            :state="flappyState"
            @close="closeFlappy"
            @minimize="minimizeFlappy"
            @maximize="maximizeFlappy"
          />
        </div>
      </div>
    </div>
    
    <!-- Modals -->
    <MarcianoModal 
      :visible="showMarciano" 
      @close="showMarciano = false" 
    />
    
    <IAJosefinaModal 
      :visible="showIATierra" 
      @close="showIATierra = false" 
    />
    
    <div class="scroll-indicator" role="button" aria-label="Desplazarse hacia abajo">
      <div class="mouse">
        <div class="wheel"></div>
      </div>
      <div class="scroll-arrows">
        <DownOutlined class="scroll-icon" />
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { ProjectOutlined, DownOutlined, DownloadOutlined } from '@ant-design/icons-vue'

// Import sub-components
import MacDesktop from './hero/MacDesktop.vue'
import CodeWindow from './hero/CodeWindow.vue'
import CalculatorWindow from './hero/CalculatorWindow.vue'
import FinderWindow from './hero/FinderWindow.vue'
import GamesFolder from './hero/GamesFolder.vue'
import MarcianoModal from './hero/MarcianoModal.vue'
import IAJosefinaModal from './hero/IASofiModal.vue'
import SnakeGame from './hero/SnakeGame.vue'
import FlappyBirdGame from './hero/FlappyBirdGame.vue'

// Typing effect
const titles = ['Full-Stack Developer', 'Mobile Developer', 'CEO de Plutonia', 'React Expert', 'RRHH Expert']
const displayedText = ref('')
let titleIndex = 0
let charIndex = 0
let isDeleting = false
let typingInterval = null
let pauseTimeout = null

// Window States
const windowState = ref('closed')
const calcState = ref('closed')
const finderState = ref('closed')

// Modal States
const showMarciano = ref(false)
const showIATierra = ref(false)

// Window Controls
const closeWindow = () => { windowState.value = 'closed' }
const minimizeWindow = () => { windowState.value = 'minimized' }
const maximizeWindow = () => { windowState.value = windowState.value === 'maximized' ? 'normal' : 'maximized' }
const openWindow = () => { windowState.value = 'normal' }

// Calculator Controls
const closeCalculator = () => { calcState.value = 'closed' }
const minimizeCalculator = () => { calcState.value = 'minimized' }
const maximizeCalculator = () => { calcState.value = calcState.value === 'maximized' ? 'normal' : 'maximized' }
const openCalculator = () => { calcState.value = 'normal' }

// Finder Controls
const closeFinder = () => { finderState.value = 'closed' }
const minimizeFinder = () => { finderState.value = 'minimized' }
const maximizeFinder = () => { finderState.value = finderState.value === 'maximized' ? 'normal' : 'maximized' }
const openFinder = () => { finderState.value = 'normal' }

// Game States
const gamesState = ref('closed')
const snakeState = ref('closed')
const flappyState = ref('closed')

// Games Folder Controls
const closeGames = () => { gamesState.value = 'closed' }
const minimizeGames = () => { gamesState.value = 'minimized' }
const maximizeGames = () => { gamesState.value = gamesState.value === 'maximized' ? 'normal' : 'maximized' }
const openGames = () => { gamesState.value = 'normal' }

// Snake Controls
const closeSnake = () => { snakeState.value = 'closed' }
const minimizeSnake = () => { snakeState.value = 'minimized' }
const maximizeSnake = () => { snakeState.value = snakeState.value === 'maximized' ? 'normal' : 'maximized' }
const openSnake = () => { snakeState.value = 'normal' }

// Flappy Bird Controls
const closeFlappy = () => { flappyState.value = 'closed' }
const minimizeFlappy = () => { flappyState.value = 'minimized' }
const maximizeFlappy = () => { flappyState.value = flappyState.value === 'maximized' ? 'normal' : 'maximized' }
const openFlappy = () => { flappyState.value = 'normal' }

// External Links
const openGitHub = () => { window.open('https://github.com/jjaracanales', '_blank') }
const openLinkedIn = () => { window.open('https://www.linkedin.com/in/jjaracanales/', '_blank') }
const openEmail = () => { window.location.href = 'mailto:jose.jara@plutonia.cl' }

// Particle Effect
const getParticleStyle = (n) => {
  const size = Math.random() * 6 + 2
  const isGlowing = Math.random() > 0.7
  return {
    left: `${Math.random() * 100}%`,
    top: `${Math.random() * 100}%`,
    width: `${size}px`,
    height: `${size}px`,
    animationDelay: `${Math.random() * 5}s`,
    animationDuration: `${Math.random() * 15 + 10}s`,
    boxShadow: isGlowing ? `0 0 ${size * 2}px var(--color-accent)` : 'none'
  }
}

// Typing Animation
const typeText = () => {
  const currentTitle = titles[titleIndex]
  
  if (!isDeleting) {
    displayedText.value = currentTitle.substring(0, charIndex + 1)
    charIndex++
    
    if (charIndex === currentTitle.length) {
      clearInterval(typingInterval)
      pauseTimeout = setTimeout(() => {
        isDeleting = true
        typingInterval = setInterval(typeText, 50)
      }, 2000)
    }
  } else {
    displayedText.value = currentTitle.substring(0, charIndex - 1)
    charIndex--
    
    if (charIndex === 0) {
      isDeleting = false
      titleIndex = (titleIndex + 1) % titles.length
      clearInterval(typingInterval)
      pauseTimeout = setTimeout(() => {
        typingInterval = setInterval(typeText, 100)
      }, 500)
    }
  }
}

onMounted(() => {
  typingInterval = setInterval(typeText, 100)
})

onUnmounted(() => {
  if (typingInterval) clearInterval(typingInterval)
  if (pauseTimeout) clearTimeout(pauseTimeout)
})
</script>

<style scoped>
.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  padding-top: 80px;
  background: radial-gradient(ellipse at 50% 50%, rgba(24, 144, 255, 0.08) 0%, transparent 50%);
}

.hero-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

/* Animated Grid Lines */
.grid-lines {
  position: absolute;
  inset: 0;
  background-image: 
    linear-gradient(rgba(24, 144, 255, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(24, 144, 255, 0.03) 1px, transparent 1px);
  background-size: 50px 50px;
  animation: gridMove 20s linear infinite;
}

@keyframes gridMove {
  0% { transform: translate(0, 0); }
  100% { transform: translate(50px, 50px); }
}

/* Gradient Orbs */
.gradient-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.5;
  animation: orbFloat 15s ease-in-out infinite;
}

.orb-1 {
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, rgba(24, 144, 255, 0.4) 0%, transparent 70%);
  top: -100px;
  right: -100px;
  animation-delay: 0s;
}

.orb-2 {
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(99, 102, 241, 0.3) 0%, transparent 70%);
  bottom: -50px;
  left: -50px;
  animation-delay: -5s;
}

.orb-3 {
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(16, 185, 129, 0.3) 0%, transparent 70%);
  top: 50%;
  left: 30%;
  animation-delay: -10s;
}

@keyframes orbFloat {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(30px, -30px) scale(1.1); }
  50% { transform: translate(-20px, 20px) scale(0.9); }
  75% { transform: translate(20px, 30px) scale(1.05); }
}

.hero-particles {
  position: absolute;
  inset: 0;
}

.particle {
  position: absolute;
  background: var(--color-accent);
  border-radius: 50%;
  opacity: 0.4;
  animation: floatParticle linear infinite;
}

@keyframes floatParticle {
  0% {
    transform: translateY(100vh) rotate(0deg);
    opacity: 0;
  }
  10% {
    opacity: 0.4;
  }
  90% {
    opacity: 0.4;
  }
  100% {
    transform: translateY(-100vh) rotate(720deg);
    opacity: 0;
  }
}

.hero-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  position: relative;
  z-index: 1;
}

/* Greeting with emoji */
.hero-greeting {
  color: var(--color-accent);
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.greeting-icon {
  display: inline-block;
  animation: wave 2.5s ease-in-out infinite;
  transform-origin: 70% 70%;
}

@keyframes wave {
  0%, 100% { transform: rotate(0deg); }
  10% { transform: rotate(14deg); }
  20% { transform: rotate(-8deg); }
  30% { transform: rotate(14deg); }
  40% { transform: rotate(-4deg); }
  50% { transform: rotate(10deg); }
  60%, 100% { transform: rotate(0deg); }
}

/* Animated Gradient Name */
.hero-name {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 16px;
  line-height: 1.2;
}

.name-text {
  background: linear-gradient(
    90deg,
    var(--color-text-primary) 0%,
    var(--color-accent-light) 25%,
    var(--color-accent) 50%,
    var(--color-accent-light) 75%,
    var(--color-text-primary) 100%
  );
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: gradientShift 4s ease-in-out infinite;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% center; }
  50% { background-position: 100% center; }
}

.hero-title {
  font-size: 1.8rem;
  font-weight: 600;
  color: var(--color-text-secondary);
  margin-bottom: 24px;
  min-height: 2.2rem;
}

.typing-text {
  color: var(--color-accent);
}

.cursor {
  color: var(--color-accent);
  animation: blink 1s infinite;
  font-weight: 300;
}

@keyframes blink {
  0%, 50% { opacity: 1; }
  51%, 100% { opacity: 0; }
}

.hero-description {
  color: var(--color-text-secondary);
  font-size: 1.1rem;
  max-width: 500px;
  margin-bottom: 24px;
  line-height: 1.8;
}

/* Hero Stats */
.hero-stats {
  display: flex;
  gap: 20px;
  margin-bottom: 32px;
}

.stat-card {
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 12px;
  padding: 16px 24px;
  text-align: center;
  transition: all 0.3s ease;
}

.stat-card:hover {
  border-color: var(--color-accent);
  transform: translateY(-4px);
  box-shadow: 0 10px 30px rgba(24, 144, 255, 0.2);
}

.stat-value {
  display: block;
  font-size: 1.8rem;
  font-weight: 800;
  color: var(--color-accent);
  line-height: 1;
}

.stat-label {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  margin-top: 4px;
}

.hero-buttons {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
}

/* Glowing Button */
.btn-glow {
  position: relative;
  overflow: hidden;
}

.btn-glow::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  transform: rotate(45deg);
  animation: btnShine 3s ease-in-out infinite;
}

@keyframes btnShine {
  0% { transform: translateX(-100%) rotate(45deg); }
  100% { transform: translateX(100%) rotate(45deg); }
}

.hero-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}

.hero-image {
  position: relative;
}

/* Orbital Rings */
.orbital-ring {
  position: absolute;
  border: 1px solid rgba(24, 144, 255, 0.2);
  border-radius: 50%;
  top: 50%;
  left: 50%;
}

.ring-1 {
  width: 450px;
  height: 450px;
  margin-left: -225px;
  margin-top: -225px;
  animation: orbit 20s linear infinite;
}

.ring-1::after {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  background: var(--color-accent);
  border-radius: 50%;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  box-shadow: 0 0 20px var(--color-accent);
}

.ring-2 {
  width: 520px;
  height: 520px;
  margin-left: -260px;
  margin-top: -260px;
  animation: orbit 25s linear infinite reverse;
  border-style: dashed;
}

.ring-3 {
  width: 380px;
  height: 380px;
  margin-left: -190px;
  margin-top: -190px;
  animation: orbit 15s linear infinite;
  opacity: 0.5;
}

@keyframes orbit {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.image-glow {
  position: absolute;
  width: 350px;
  height: 350px;
  background: radial-gradient(circle, var(--color-accent-glow) 0%, transparent 70%);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: -1;
  animation: pulseGlow 4s ease-in-out infinite;
}

@keyframes pulseGlow {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 0.8; transform: translate(-50%, -50%) scale(1.1); }
}

/* Scroll Indicator */
.scroll-indicator {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  cursor: pointer;
}

.mouse {
  width: 26px;
  height: 40px;
  border: 2px solid var(--color-accent);
  border-radius: 20px;
  position: relative;
  opacity: 0.7;
}

.wheel {
  width: 4px;
  height: 8px;
  background: var(--color-accent);
  border-radius: 2px;
  position: absolute;
  top: 8px;
  left: 50%;
  transform: translateX(-50%);
  animation: scrollWheel 2s infinite;
}

@keyframes scrollWheel {
  0% { opacity: 1; top: 8px; }
  100% { opacity: 0; top: 20px; }
}

.scroll-arrows {
  animation: bounce 2s infinite;
}

.scroll-icon {
  font-size: 20px;
  color: var(--color-accent);
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-8px); }
  60% { transform: translateY(-4px); }
}

/* Animations */
.animate-fadeInUp {
  opacity: 0;
  animation: fadeInUp 0.8s ease forwards;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsive */
@media (max-width: 992px) {
  .hero-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 40px;
  }
  
  .hero-greeting {
    justify-content: center;
  }
  
  .hero-name {
    font-size: 2.5rem;
  }
  
  .hero-description {
    margin: 0 auto 24px;
  }
  
  .hero-stats {
    justify-content: center;
  }
  
  .hero-buttons {
    justify-content: center;
  }
  
  .orbital-ring {
    display: none;
  }
}

@media (max-width: 480px) {
  .hero-name {
    font-size: 2rem;
  }
  
  .hero-title {
    font-size: 1.4rem;
  }
  
  .hero-stats {
    flex-wrap: wrap;
  }
  
  .stat-card {
    padding: 12px 16px;
  }
}
</style>
