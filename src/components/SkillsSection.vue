<template>
  <section id="skills" class="section skills-section">
    <div class="container">
      <h2 class="section-title">Habilidades</h2>
      <p class="section-subtitle">Tecnologías y herramientas que domino</p>
      
      <!-- Tech Icons Banner -->
      <div class="tech-icons-banner">
        <div class="tech-icons-track">
          <div v-for="(tech, index) in techIcons" :key="index" class="tech-icon">
            <img :src="tech.icon" :alt="tech.name" />
          </div>
          <!-- Duplicate for seamless loop -->
          <div v-for="(tech, index) in techIcons" :key="'dup-' + index" class="tech-icon">
            <img :src="tech.icon" :alt="tech.name" />
          </div>
        </div>
      </div>
      
      <div class="skills-categories">
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <DesktopOutlined /> Desarrollo Frontend
          </h3>
          <div class="skill-tags">
            <span v-for="skill in frontendSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <MobileOutlined /> Desarrollo Mobile
          </h3>
          <div class="skill-tags">
            <span v-for="skill in mobileSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <CloudServerOutlined /> Desarrollo Backend
          </h3>
          <div class="skill-tags">
            <span v-for="skill in backendSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <DatabaseOutlined /> Bases de Datos
          </h3>
          <div class="skill-tags">
            <span v-for="skill in databaseSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <ToolOutlined /> Herramientas & Metodologías
          </h3>
          <div class="skill-tags">
            <span v-for="skill in toolsSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <BarChartOutlined /> Data & Analytics
          </h3>
          <div class="skill-tags">
            <span v-for="skill in dataSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <HighlightOutlined /> Diseño & 3D
          </h3>
          <div class="skill-tags">
            <span v-for="skill in designSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
        
        <div class="skill-category glass-card">
          <h3 class="category-title">
            <AppstoreOutlined /> Sistemas ERP & Gestión
          </h3>
          <div class="skill-tags">
            <span v-for="skill in erpSkills" :key="skill" class="skill-tag">{{ skill }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { 
  DesktopOutlined,
  MobileOutlined,
  CloudServerOutlined,
  DatabaseOutlined,
  ToolOutlined,
  BarChartOutlined,
  HighlightOutlined,
  AppstoreOutlined
} from '@ant-design/icons-vue'

// Tech Icons for banner
const techIcons = [
  { name: 'HTML5', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg' },
  { name: 'CSS3', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg' },
  { name: 'JavaScript', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg' },
  { name: 'TypeScript', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/typescript/typescript-original.svg' },
  { name: 'React', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg' },
  { name: 'Vue.js', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vuejs/vuejs-original.svg' },
  { name: 'Node.js', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg' },
  { name: 'PHP', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/php/php-original.svg' },
  { name: 'Laravel', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/laravel/laravel-original.svg' },
  { name: 'Python', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg' },
  { name: 'MySQL', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg' },
  { name: 'MongoDB', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mongodb/mongodb-original.svg' },
  { name: 'Firebase', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/firebase/firebase-plain.svg' },
  { name: 'Git', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg' },
  { name: 'Kotlin', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kotlin/kotlin-original.svg' },
  { name: 'Swift', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/swift/swift-original.svg' },
  { name: 'Android', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/android/android-original.svg' },
  { name: 'Docker', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg' },
  { name: 'Figma', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/figma/figma-original.svg' },
  { name: 'Blender', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/blender/blender-original.svg' },
  { name: 'Illustrator', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/illustrator/illustrator-plain.svg' },
  { name: 'WordPress', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/wordpress/wordpress-plain.svg' },
  { name: 'Google Cloud', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon/icons/googlecloud/googlecloud-original.svg' },
  { name: 'Tailwind', icon: 'https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/tailwindcss/tailwindcss-original.svg' },
]

// Desarrollo Frontend
const frontendSkills = ['HTML5', 'CSS3', 'JavaScript', 'TypeScript', 'React', 'Vue.js', 'Tailwind CSS', 'WordPress']

// Desarrollo Mobile
const mobileSkills = ['React Native', 'Expo', 'Ionic', 'Kotlin', 'Swift']

// Desarrollo Backend
const backendSkills = ['Node.js', 'PHP', 'Laravel', 'Express.js']

// Bases de Datos
const databaseSkills = ['MySQL', 'MongoDB', 'Oracle SQL', 'Firestore', 'Supabase']

// Herramientas & Metodologías
const toolsSkills = ['Git', 'GitHub', 'Scrum', 'Kanban', 'Google Cloud Platform', 'cPanel', 'Postman']

// Data & Analytics
const dataSkills = ['Power BI', 'Python', 'Excel Avanzado', 'VBA']

// Diseño & 3D
const designSkills = ['Blender', 'SketchUp (V-Ray)', 'AutoCAD', 'Adobe Illustrator', 'Inkscape']

// Sistemas ERP & Gestión
const erpSkills = ['BUK', 'GeoVictoria', 'Talana', 'Iconstruye', 'Websal', 'Pronexo']
</script>

<style scoped>
.skills-section {
  background: linear-gradient(180deg, rgba(13, 27, 42, 0.5) 0%, transparent 100%);
}

/* Tech Icons Banner */
.tech-icons-banner {
  width: 100%;
  overflow: hidden;
  margin-bottom: 48px;
  padding: 24px 0;
  background: rgba(24, 144, 255, 0.03);
  border-radius: 20px;
  border: 1px solid rgba(24, 144, 255, 0.1);
  position: relative;
}

.tech-icons-banner::before,
.tech-icons-banner::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100px;
  z-index: 2;
  pointer-events: none;
}

.tech-icons-banner::before {
  left: 0;
  background: linear-gradient(to right, var(--color-bg-primary), transparent);
}

.tech-icons-banner::after {
  right: 0;
  background: linear-gradient(to left, var(--color-bg-primary), transparent);
}

.tech-icons-track {
  display: flex;
  gap: 32px;
  animation: scroll 30s linear infinite;
  width: fit-content;
}

.tech-icons-track:hover {
  animation-play-state: paused;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

.tech-icon {
  flex-shrink: 0;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 10px;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
}

.tech-icon:hover {
  background: rgba(24, 144, 255, 0.2);
  transform: translateY(-5px) scale(1.1);
  box-shadow: 0 10px 25px rgba(24, 144, 255, 0.3);
}

.tech-icon img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  filter: grayscale(30%);
  transition: filter 0.3s ease;
}

.tech-icon:hover img {
  filter: grayscale(0%) drop-shadow(0 0 8px rgba(24, 144, 255, 0.5));
}

.skills-categories {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}

.skill-category {
  text-align: center;
  animation: fadeInUp 0.6s ease forwards;
  opacity: 0;
}

.skill-category:nth-child(1) { animation-delay: 0.1s; }
.skill-category:nth-child(2) { animation-delay: 0.15s; }
.skill-category:nth-child(3) { animation-delay: 0.2s; }
.skill-category:nth-child(4) { animation-delay: 0.25s; }
.skill-category:nth-child(5) { animation-delay: 0.3s; }
.skill-category:nth-child(6) { animation-delay: 0.35s; }
.skill-category:nth-child(7) { animation-delay: 0.4s; }
.skill-category:nth-child(8) { animation-delay: 0.45s; }

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

.category-title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-accent);
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.skill-tags {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
}

.skill-tag {
  background: rgba(24, 144, 255, 0.1);
  border: 1px solid rgba(24, 144, 255, 0.25);
  color: var(--color-text-primary);
  padding: 6px 12px;
  border-radius: 16px;
  font-size: 0.8rem;
  transition: all var(--transition-medium);
  cursor: default;
}

.skill-tag:hover {
  background: var(--color-accent);
  border-color: var(--color-accent);
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 5px 15px rgba(24, 144, 255, 0.3);
}

@media (max-width: 1200px) {
  .skills-categories {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 992px) {
  .skills-categories {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 576px) {
  .skills-categories {
    grid-template-columns: 1fr;
  }
  
  .category-title {
    font-size: 1.1rem;
  }
  
  .skill-tag {
    font-size: 0.85rem;
    padding: 8px 14px;
  }
  
  .tech-icons-banner {
    padding: 16px 0;
    margin-bottom: 32px;
  }
  
  .tech-icon {
    width: 40px;
    height: 40px;
    padding: 8px;
  }
  
  .tech-icons-track {
    gap: 20px;
  }
}
</style>
