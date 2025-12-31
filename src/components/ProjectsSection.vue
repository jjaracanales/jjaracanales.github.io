<template>
  <section id="projects" class="section projects-section">
    <div class="container">
      <h2 class="section-title">Proyectos</h2>
      <p class="section-subtitle">Aplicaciones móviles y sitios web que he desarrollado</p>
      
      <!-- Tabs -->
      <div class="project-tabs">
        <button 
          :class="['tab-btn', { active: activeTab === 'apps' }]"
          @click="activeTab = 'apps'"
        >
          <MobileOutlined /> Apps Móviles
        </button>
        <button 
          :class="['tab-btn', { active: activeTab === 'web' }]"
          @click="activeTab = 'web'"
        >
          <GlobalOutlined /> Sitios Web
        </button>
      </div>
      
      <!-- Apps Grid -->
      <div v-show="activeTab === 'apps'" class="projects-grid">
        <div 
          v-for="(project, index) in mobileApps" 
          :key="project.title"
          class="project-card glass-card"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          <div class="project-icon">
            <component :is="project.icon" />
          </div>
          <h3 class="project-title">{{ project.title }}</h3>
          <p class="project-description">{{ project.description }}</p>
          <div class="project-tech">
            <a-tag v-for="tech in project.technologies" :key="tech" size="small">
              {{ tech }}
            </a-tag>
          </div>
          <a v-if="project.playStore" :href="project.playStore" target="_blank" class="project-link">
            <AndroidOutlined /> Play Store
          </a>
        </div>
      </div>
      
      <!-- Web Grid -->
      <div v-show="activeTab === 'web'" class="projects-grid web-grid">
        <div 
          v-for="(project, index) in webProjects" 
          :key="project.title"
          class="web-project-card glass-card"
          :style="{ animationDelay: `${index * 0.1}s` }"
        >
          <div class="web-preview">
            <img v-if="project.image" :src="project.image" :alt="project.title" />
            <div v-else class="web-preview-placeholder">
              <GlobalOutlined />
            </div>
            <div class="web-overlay">
              <a :href="project.url" target="_blank" class="preview-link">
                <LinkOutlined /> Visitar
              </a>
            </div>
          </div>
          <div class="web-info">
            <h3 class="project-title">{{ project.title }}</h3>
            <p class="project-description">{{ project.description }}</p>
            <div v-if="project.technologies" class="project-tech">
              <a-tag v-for="tech in project.technologies" :key="tech" size="small">
                {{ tech }}
              </a-tag>
            </div>
          </div>
        </div>
      </div>
      
      <div class="projects-cta">
        <a href="https://github.com/jjaracanales" target="_blank" class="btn-secondary">
          <GithubOutlined /> Ver más en GitHub
        </a>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { Tag as ATag } from 'ant-design-vue'
import { 
  MobileOutlined, 
  GlobalOutlined, 
  GithubOutlined,
  AndroidOutlined,
  LinkOutlined,
  ScissorOutlined,
  CalculatorOutlined,
  ClockCircleOutlined,
  MedicineBoxOutlined,
  TrophyOutlined,
  RocketOutlined,
  AppstoreOutlined,
  AimOutlined
} from '@ant-design/icons-vue'

// Web images
import plutoniaImg from '../assets/web/plutonia.png'

const activeTab = ref('apps')

const mobileApps = [
  {
    title: 'Beauty Control',
    description: 'Gestión completa para peluquerías: citas, clientes, inventario y reportes.',
    technologies: ['React Native', 'Expo', 'Firebase'],
    icon: ScissorOutlined,
    playStore: '#'
  },
  {
    title: 'LiquidaChile',
    description: 'Calculadora de liquidaciones de sueldo según la legislación chilena.',
    technologies: ['React Native', 'Expo'],
    icon: CalculatorOutlined,
    playStore: '#'
  },
  {
    title: 'Calculadora Horas Extras',
    description: 'Calcula horas extras según normativa laboral chilena.',
    technologies: ['React Native', 'Expo'],
    icon: ClockCircleOutlined,
    playStore: '#'
  },
  {
    title: 'Farmacias de Turno',
    description: 'Encuentra farmacias de turno cercanas en todo Chile.',
    technologies: ['React Native', 'API'],
    icon: MedicineBoxOutlined,
    playStore: '#'
  },
  {
    title: 'Tabata Timer',
    description: 'Timer para entrenamientos de alta intensidad HIT.',
    technologies: ['React Native', 'Expo'],
    icon: ClockCircleOutlined,
    playStore: '#'
  },
  {
    title: 'Multimarcador Deportivo',
    description: 'Marcador multipropósito para diversos deportes.',
    technologies: ['React Native'],
    icon: TrophyOutlined,
    playStore: '#'
  },
  {
    title: 'Astro Jump',
    description: 'Juego móvil de plataformas espaciales.',
    technologies: ['React Native', 'Game Dev'],
    icon: RocketOutlined,
    playStore: '#'
  },
  {
    title: 'Minimalist Sudoku',
    description: 'Sudoku con diseño minimalista y múltiples niveles.',
    technologies: ['React Native'],
    icon: AppstoreOutlined,
    playStore: '#'
  },
  {
    title: 'Orbital',
    description: 'Juego móvil de estrategia espacial.',
    technologies: ['React Native', 'Game Dev'],
    icon: AimOutlined,
    playStore: '#'
  }
]

const webProjects = [
  {
    title: 'Plutonia.cl',
    description: 'Sitio corporativo de mi agencia de desarrollo web y apps.',
    url: 'https://plutonia.cl',
    image: plutoniaImg,
    technologies: ['Vue.js', 'Ant Design']
  },
  {
    title: 'SubControl.cl',
    description: 'Plataforma de gestión de subcontratos.',
    url: 'https://subcontrol.cl',
    image: null,
    technologies: []
  },
  {
    title: 'Empleando.cl',
    description: 'Portal de empleo en Chile.',
    url: 'https://empleando.cl',
    image: null,
    technologies: []
  },
  {
    title: 'SantaBernardita.cl',
    description: 'Sitio e-commerce para tienda local.',
    url: 'https://santabernardita.cl',
    image: null,
    technologies: []
  },
  {
    title: 'ConectaColombia.cl',
    description: 'Portal comunitario para colombianos en Chile.',
    url: 'https://conectacolombia.cl',
    image: null,
    technologies: []
  },
  {
    title: 'ColombianosEnChile.cl',
    description: 'Sitio informativo para la comunidad colombiana.',
    url: 'https://colombianosenchile.cl',
    image: null,
    technologies: []
  }
]
</script>

<style scoped>
.projects-section {
  background: linear-gradient(180deg, transparent 0%, rgba(13, 27, 42, 0.5) 100%);
}

.project-tabs {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-bottom: 40px;
}

.tab-btn {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 14px 28px;
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 30px;
  color: var(--color-text-secondary);
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all var(--transition-medium);
}

.tab-btn:hover {
  border-color: var(--color-accent);
  color: var(--color-accent);
}

.tab-btn.active {
  background: var(--color-accent);
  border-color: var(--color-accent);
  color: white;
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  margin-bottom: 50px;
}

.project-card {
  text-align: center;
  animation: fadeInUp 0.5s ease forwards;
  opacity: 0;
}

.project-icon {
  width: 70px;
  height: 70px;
  margin: 0 auto 20px;
  background: linear-gradient(135deg, var(--color-accent) 0%, var(--color-accent-dark) 100%);
  border-radius: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  color: white;
}

.web-icon {
  background: linear-gradient(135deg, #52c41a 0%, #237804 100%);
}

.project-title {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 12px;
  color: var(--color-text-primary);
}

.project-description {
  color: var(--color-text-secondary);
  font-size: 0.9rem;
  margin-bottom: 16px;
  line-height: 1.5;
}

.project-tech {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 6px;
  margin-bottom: 16px;
}

.project-tech :deep(.ant-tag) {
  background: rgba(24, 144, 255, 0.1);
  border: 1px solid rgba(24, 144, 255, 0.2);
  color: var(--color-accent);
  border-radius: 4px;
  font-size: 0.8rem;
}

.project-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: var(--color-accent);
  font-weight: 500;
  transition: all var(--transition-fast);
}

.project-link:hover {
  color: var(--color-accent-light);
  transform: translateX(4px);
}

.projects-cta {
  text-align: center;
}

/* Web Project Cards with Images */
.web-grid {
  grid-template-columns: repeat(2, 1fr);
  gap: 30px;
}

.web-project-card {
  padding: 0;
  overflow: hidden;
  animation: fadeInUp 0.5s ease forwards;
  opacity: 0;
  border-radius: 16px;
}

.web-preview {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background: linear-gradient(135deg, rgba(24, 144, 255, 0.1) 0%, rgba(82, 196, 26, 0.1) 100%);
}

.web-preview img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top center;
  transition: transform 0.4s ease;
}

.web-project-card:hover .web-preview img {
  transform: scale(1.05);
}

.web-preview-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 4rem;
  color: rgba(82, 196, 26, 0.3);
  background: linear-gradient(135deg, rgba(30, 30, 30, 0.8) 0%, rgba(50, 50, 50, 0.8) 100%);
}

.web-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.web-project-card:hover .web-overlay {
  opacity: 1;
}

.preview-link {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  background: var(--color-accent);
  color: white;
  border-radius: 25px;
  font-weight: 600;
  transition: all 0.3s ease;
}

.preview-link:hover {
  transform: scale(1.1);
  box-shadow: 0 10px 30px rgba(24, 144, 255, 0.4);
}

.web-info {
  padding: 20px;
  text-align: center;
}

.web-info .project-title {
  margin-bottom: 8px;
}

.web-info .project-description {
  margin-bottom: 12px;
}

.web-info .project-tech {
  margin-top: 0;
}

@media (max-width: 992px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 576px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
  
  .web-grid {
    grid-template-columns: 1fr;
  }
  
  .project-tabs {
    flex-direction: column;
  }
}
</style>
