<template>
  <header class="navbar" :class="{ 'navbar-scrolled': isScrolled }">
    <div class="container navbar-container">
      <div class="navbar-logo">
        <span class="logo-text">JJC</span>
      </div>
      
      <nav class="navbar-menu" :class="{ 'menu-open': isMenuOpen }">
        <a href="#hero" @click="closeMenu">Inicio</a>
        <a href="#about" @click="closeMenu">Sobre Mí</a>
        <a href="#skills" @click="closeMenu">Habilidades</a>
        <a href="#projects" @click="closeMenu">Proyectos</a>
        <a href="#contact" @click="closeMenu">Contacto</a>
      </nav>
      
      <button class="menu-toggle" @click="toggleMenu" :class="{ 'is-open': isMenuOpen }">
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)
const isMenuOpen = ref(false)

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}

const closeMenu = () => {
  isMenuOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  padding: 20px 0;
  transition: all var(--transition-medium);
}

.navbar-scrolled {
  background: var(--glass-bg);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border-bottom: 1px solid var(--glass-border);
  padding: 12px 0;
}

.navbar-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navbar-logo {
  display: flex;
  align-items: center;
}

.logo-text {
  font-size: 1.8rem;
  font-weight: 800;
  background: linear-gradient(135deg, var(--color-accent) 0%, var(--color-accent-light) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.navbar-menu {
  display: flex;
  gap: 32px;
}

.navbar-menu a {
  color: var(--color-text-secondary);
  font-weight: 500;
  padding: 8px 0;
  position: relative;
  transition: color var(--transition-fast);
}

.navbar-menu a::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--color-accent);
  transition: width var(--transition-medium);
}

.navbar-menu a:hover {
  color: var(--color-text-primary);
}

.navbar-menu a:hover::after {
  width: 100%;
}

.menu-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
}

.menu-toggle span {
  width: 25px;
  height: 2px;
  background: var(--color-text-primary);
  transition: all var(--transition-fast);
}

.menu-toggle.is-open span:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.menu-toggle.is-open span:nth-child(2) {
  opacity: 0;
}

.menu-toggle.is-open span:nth-child(3) {
  transform: rotate(-45deg) translate(5px, -5px);
}

@media (max-width: 768px) {
  .menu-toggle {
    display: flex;
  }
  
  .navbar-menu {
    position: fixed;
    top: 70px;
    left: 0;
    right: 0;
    background: var(--color-bg-primary);
    flex-direction: column;
    padding: 24px;
    gap: 16px;
    transform: translateX(-100%);
    transition: transform var(--transition-medium);
    border-bottom: 1px solid var(--glass-border);
  }
  
  .navbar-menu.menu-open {
    transform: translateX(0);
  }
}
</style>
