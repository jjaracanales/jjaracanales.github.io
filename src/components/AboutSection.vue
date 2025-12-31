<template>
  <section id="about" class="section about-section">
    <div class="container">
      <h2 class="section-title">Sobre Mí</h2>
      <p class="section-subtitle">Conoce un poco más sobre mi trayectoria</p>
      
      <div class="about-content">
        <div class="about-image">
          <div class="image-frame">
            <img src="../assets/foto.jpg" alt="José Jara Canales - Full Stack Developer y CEO de Plutonia SpA" class="profile-photo" loading="lazy" />
            <div class="frame-decoration"></div>
          </div>
        </div>
        
        <div class="about-text">
          <h3 class="about-heading">
            Desarrollador Full-Stack apasionado por crear 
            <span class="highlight">soluciones tecnológicas innovadoras</span>
          </h3>
          
          <p class="about-description">
            Soy CEO y desarrollador principal en <strong>Plutonia SpA</strong>, una consultora tecnológica 
            especializada en soluciones web, mobile y análisis de datos. Gestiono el ciclo completo 
            de desarrollo desde el levantamiento de requerimientos hasta el despliegue y mantenimiento.
          </p>
          
          <p class="about-description">
            Actualmente curso Ingeniería en Informática, combinando habilidades técnicas con 
            más de 15 años de experiencia en gestión de personas y certificación avanzada en 
            remuneraciones, lo que me permite crear soluciones con profundo entendimiento de 
            las necesidades del usuario.
          </p>
          
          <div class="about-stats">
            <div class="stat-item">
              <span class="stat-number">30+</span>
              <span class="stat-label">Proyectos completados</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">12</span>
              <span class="stat-label">Apps en Play Store</span>
            </div>
            <div class="stat-item">
              <span class="stat-number">10+</span>
              <span class="stat-label">Sitios web activos</span>
            </div>
          </div>
          
          <a href="#contact" class="btn-primary">
            <MessageOutlined /> Hablemos
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { MessageOutlined } from '@ant-design/icons-vue'
</script>

<style scoped>
.about-section {
  background: linear-gradient(180deg, transparent 0%, rgba(13, 27, 42, 0.5) 100%);
}

.about-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 60px;
  align-items: center;
}

.about-image {
  display: flex;
  justify-content: center;
}

.image-frame {
  position: relative;
  width: 300px;
  height: 350px;
}

.profile-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 16px;
  position: relative;
  z-index: 1;
  border: 1px solid var(--glass-border);
}

.frame-decoration {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid var(--color-accent);
  border-radius: 16px;
  top: 20px;
  left: 20px;
  z-index: 0;
}

.about-heading {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 24px;
  line-height: 1.4;
}

.highlight {
  color: var(--color-accent);
}

.about-description {
  color: var(--color-text-secondary);
  font-size: 1.05rem;
  margin-bottom: 16px;
  line-height: 1.8;
}

.about-description strong {
  color: var(--color-accent);
}

.about-stats {
  display: flex;
  gap: 40px;
  margin: 40px 0;
}

.stat-item {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 2.5rem;
  font-weight: 800;
  color: var(--color-accent);
  line-height: 1;
  margin-bottom: 8px;
}

.stat-label {
  font-size: 0.9rem;
  color: var(--color-text-secondary);
}

@media (max-width: 992px) {
  .about-content {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 40px;
  }
  
  .about-stats {
    justify-content: center;
  }
  
  .image-frame {
    width: 250px;
    height: 300px;
  }
}

@media (max-width: 576px) {
  .about-stats {
    flex-direction: column;
    gap: 24px;
  }
  
  .about-heading {
    font-size: 1.6rem;
  }
}
</style>
