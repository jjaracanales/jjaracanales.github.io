<template>
  <section id="contact" class="section contact-section">
    <div class="container">
      <h2 class="section-title">Contacto</h2>
      <p class="section-subtitle">¿Tienes un proyecto en mente? ¡Hablemos!</p>
      
      <div class="contact-content">
        <div class="contact-info">
          <h3 class="info-title">Conectemos</h3>
          <p class="info-description">
            Estoy disponible para proyectos de desarrollo web y mobile, 
            consultoría técnica o simplemente para charlar sobre tecnología.
          </p>
          
          <div class="contact-methods">
            <a href="mailto:jose.jara@plutonia.cl" class="contact-method">
              <div class="method-icon">
                <MailOutlined />
              </div>
              <div class="method-info">
                <span class="method-label">Email</span>
                <span class="method-value">jose.jara@plutonia.cl</span>
              </div>
            </a>
            
            <a href="tel:+56959017230" class="contact-method">
              <div class="method-icon">
                <PhoneOutlined />
              </div>
              <div class="method-info">
                <span class="method-label">Teléfono</span>
                <span class="method-value">+569 5901 7230</span>
              </div>
            </a>
            
            <a href="https://www.linkedin.com/in/jjaracanales/" target="_blank" class="contact-method">
              <div class="method-icon">
                <LinkedinOutlined />
              </div>
              <div class="method-info">
                <span class="method-label">LinkedIn</span>
                <span class="method-value">/in/jjaracanales</span>
              </div>
            </a>
            
            <a href="https://github.com/jjaracanales" target="_blank" class="contact-method">
              <div class="method-icon">
                <GithubOutlined />
              </div>
              <div class="method-info">
                <span class="method-label">GitHub</span>
                <span class="method-value">@jjaracanales</span>
              </div>
            </a>
            
            <a href="https://plutonia.cl" target="_blank" class="contact-method">
              <div class="method-icon">
                <GlobalOutlined />
              </div>
              <div class="method-info">
                <span class="method-label">Website</span>
                <span class="method-value">plutonia.cl</span>
              </div>
            </a>
          </div>
          
          <div class="location">
            <EnvironmentOutlined />
            <span>Melipilla, Santiago, Chile</span>
          </div>
        </div>
        
        <div class="contact-form glass-card">
          <a-form 
            :model="formState" 
            layout="vertical"
            @finish="handleSubmit"
          >
            <a-form-item label="Nombre" name="name" :rules="[{ required: true, message: 'Por favor ingresa tu nombre' }]">
              <a-input v-model:value="formState.name" placeholder="Tu nombre" size="large" />
            </a-form-item>
            
            <a-form-item label="Email" name="email" :rules="[{ required: true, type: 'email', message: 'Por favor ingresa un email válido' }]">
              <a-input v-model:value="formState.email" placeholder="tu@email.com" size="large" />
            </a-form-item>
            
            <a-form-item label="Mensaje" name="message" :rules="[{ required: true, message: 'Por favor ingresa un mensaje' }]">
              <a-textarea 
                v-model:value="formState.message" 
                placeholder="Cuéntame sobre tu proyecto..." 
                :rows="4"
                size="large"
              />
            </a-form-item>
            
            <a-form-item>
              <a-button type="primary" html-type="submit" size="large" block :loading="isSubmitting">
                <SendOutlined /> Enviar Mensaje
              </a-button>
            </a-form-item>
          </a-form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { Form as AForm, FormItem as AFormItem, Input as AInput, Textarea as ATextarea, Button as AButton, message } from 'ant-design-vue'
import { MailOutlined, PhoneOutlined, LinkedinOutlined, GithubOutlined, GlobalOutlined, EnvironmentOutlined, SendOutlined } from '@ant-design/icons-vue'

const formState = reactive({
  name: '',
  email: '',
  message: ''
})

const isSubmitting = ref(false)

const handleSubmit = async () => {
  isSubmitting.value = true
  
  try {
    const response = await fetch('https://formsubmit.co/ajax/jose.jara@plutonia.cl', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify({
        name: formState.name,
        email: formState.email,
        message: formState.message,
        _subject: `Nuevo mensaje de ${formState.name} desde el Portfolio`
      })
    })
    
    if (response.ok) {
      message.success('¡Mensaje enviado con éxito! Te contactaré pronto.')
      formState.name = ''
      formState.email = ''
      formState.message = ''
    } else {
      throw new Error('Error al enviar')
    }
  } catch (error) {
    message.error('Error al enviar el mensaje. Intenta nuevamente o escríbeme directo a jose.jara@plutonia.cl')
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.contact-section {
  background: linear-gradient(180deg, rgba(13, 27, 42, 0.5) 0%, var(--color-bg-dark) 100%);
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 60px;
  align-items: start;
}

.info-title {
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 16px;
  color: var(--color-text-primary);
}

.info-description {
  color: var(--color-text-secondary);
  font-size: 1.05rem;
  margin-bottom: 32px;
  line-height: 1.7;
}

.contact-methods {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-bottom: 24px;
}

.contact-method {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px;
  background: var(--glass-bg);
  border: 1px solid var(--glass-border);
  border-radius: 12px;
  transition: all var(--transition-medium);
}

.contact-method:hover {
  border-color: var(--color-accent);
  transform: translateX(8px);
}

.method-icon {
  width: 48px;
  height: 48px;
  background: rgba(24, 144, 255, 0.1);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  color: var(--color-accent);
}

.method-info {
  display: flex;
  flex-direction: column;
}

.method-label {
  font-size: 0.85rem;
  color: var(--color-text-muted);
}

.method-value {
  font-size: 1rem;
  color: var(--color-text-primary);
  font-weight: 500;
}

.location {
  display: flex;
  align-items: center;
  gap: 8px;
  color: var(--color-text-secondary);
  font-size: 0.95rem;
}

.contact-form {
  padding: 40px;
}

.contact-form :deep(.ant-form-item-label > label) {
  color: var(--color-text-primary);
  font-weight: 500;
}

.contact-form :deep(.ant-input),
.contact-form :deep(.ant-input-textarea textarea) {
  background: rgba(0, 0, 0, 0.3) !important;
  border-color: var(--glass-border) !important;
}

.contact-form :deep(.ant-btn-primary) {
  height: 48px;
  font-size: 1rem;
  font-weight: 600;
}

@media (max-width: 992px) {
  .contact-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }
}
</style>
