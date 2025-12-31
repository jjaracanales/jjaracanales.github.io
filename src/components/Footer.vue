<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-brand">
          <span class="logo-text">José Jara</span>
          <p class="footer-tagline">
            Full-Stack Developer | CEO de Plutonia SpA
          </p>
          <p class="footer-location">
            <EnvironmentOutlined /> Melipilla, Santiago, Chile
          </p>
        </div>
        
        <div class="footer-links">
          <h4>Navegación</h4>
          <a href="#hero">Inicio</a>
          <a href="#about">Sobre Mí</a>
          <a href="#skills">Habilidades</a>
          <a href="#projects">Proyectos</a>
          <a href="#contact">Contacto</a>
        </div>
        
        <div class="footer-social">
          <h4>Sígueme</h4>
          <div class="social-icons">
            <a href="https://github.com/jjaracanales" target="_blank" aria-label="GitHub">
              <GithubOutlined />
            </a>
            <a href="https://www.linkedin.com/in/jjaracanales/" target="_blank" aria-label="LinkedIn">
              <LinkedinOutlined />
            </a>
            <a href="https://plutonia.cl" target="_blank" aria-label="Plutonia">
              <GlobalOutlined />
            </a>
            <a href="mailto:jose.jara@plutonia.cl" aria-label="Email">
              <MailOutlined />
            </a>
            <a href="https://wa.me/56959017230" target="_blank" aria-label="WhatsApp">
              <WhatsAppOutlined />
            </a>
          </div>
        </div>
      </div>
      
      <div class="footer-bottom">
        <p>© {{ currentYear }} José Jara Canales. Todos los derechos reservados.</p>
        <p class="made-with">
          Desarrollado con <HeartFilled class="heart-icon" /> usando Vue.js + Ant Design
        </p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'
import { GithubOutlined, LinkedinOutlined, GlobalOutlined, MailOutlined, WhatsAppOutlined, HeartFilled, EnvironmentOutlined } from '@ant-design/icons-vue'

const currentYear = computed(() => new Date().getFullYear())
</script>

<style scoped>
.footer {
  background: var(--color-bg-dark);
  border-top: 1px solid var(--glass-border);
  padding: 60px 0 24px;
}

.footer-content {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 60px;
  margin-bottom: 40px;
}

.footer-brand .logo-text {
  font-size: 1.8rem;
  font-weight: 800;
  background: linear-gradient(135deg, var(--color-accent) 0%, var(--color-accent-light) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.footer-tagline {
  color: var(--color-text-secondary);
  margin-top: 12px;
  font-size: 0.95rem;
}

.footer-location {
  color: var(--color-text-muted);
  margin-top: 8px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 6px;
}

.footer-links h4,
.footer-social h4 {
  color: var(--color-text-primary);
  font-weight: 600;
  margin-bottom: 20px;
}

.footer-links {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.footer-links a {
  color: var(--color-text-secondary);
  transition: color var(--transition-fast);
}

.footer-links a:hover {
  color: var(--color-accent);
}

.social-icons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.social-icons a {
  width: 44px;
  height: 44px;
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: var(--color-text-secondary);
  transition: all var(--transition-medium);
}

.social-icons a:hover {
  background: var(--color-accent);
  border-color: var(--color-accent);
  color: white;
  transform: translateY(-3px);
}

.footer-bottom {
  border-top: 1px solid var(--glass-border);
  padding-top: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: var(--color-text-muted);
  font-size: 0.9rem;
}

.made-with {
  display: flex;
  align-items: center;
  gap: 4px;
}

.heart-icon {
  color: #ff4d4f;
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: 1fr;
    gap: 32px;
    text-align: center;
  }
  
  .footer-links {
    align-items: center;
  }
  
  .footer-location {
    justify-content: center;
  }
  
  .social-icons {
    justify-content: center;
  }
  
  .footer-bottom {
    flex-direction: column;
    gap: 12px;
    text-align: center;
  }
}
</style>
