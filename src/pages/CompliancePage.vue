<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import compliance from '@/data/compliance.json'
import { DollarSign, Scale, AlertTriangle, TrendingDown } from 'lucide-vue-next'

const icons = {
  DollarSign,
  Scale,
  AlertTriangle,
  TrendingDown
}

const getIcon = (iconName: string) => icons[iconName as keyof typeof icons]

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

const trustAvatars = [
  '/testimonials/lina.jpg',
  '/testimonials/Anya.jpg',
  '/testimonials/priya.jpg',
]
</script>

<template>
  <!-- ============= HERO ============= -->
  <header class="service-hero compliance-hero">
    <div class="compliance-hero-inner">
      <div class="service-hero-copy">
        <h1>
          Stay <em>compliant</em>,<br />everywhere
        </h1>
        <p class="service-hero-lede">{{ compliance.definition.description }}</p>
        <ul class="service-hero-features">
          <li v-for="(f, i) in compliance.definition.keyFeatures" :key="i" class="hero-feature">
            <span class="hero-feature-check">✓</span>
            {{ f }}
          </li>
        </ul>
        <div class="cta-row">
          <RouterLink to="/contact?reason=compliance_questions" class="btn-primary">
            {{ compliance.definition.primaryButtonText }} <span class="arrow">→</span>
          </RouterLink>
          <RouterLink to="/contact?reason=consultation" class="btn-secondary">
            {{ compliance.definition.secondaryButtonText }}
          </RouterLink>
        </div>
        <div class="trust-row">
          <div class="avatars">
            <div
              v-for="(src, i) in trustAvatars"
              :key="i"
              class="avatar"
              :style="{ backgroundImage: `url('${src}')` }"
            />
          </div>
          <span class="trust-text">
            <strong>99.9%</strong> compliance rate across 160+ countries
          </span>
        </div>
      </div>
    </div>
  </header>

  <!-- ============= MAIN DESCRIPTION ============= -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">What is compliance services</span>
        <h2 class="section-title">{{ compliance.mainDescription.title }}</h2>
      </div>
      <div class="definition-text">
        <p>{{ compliance.mainDescription.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= STATS STRIP ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in compliance.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- ============= SERVICES ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">What we deliver</span>
      <h2 class="section-title">{{ compliance.services.title }}</h2>
      <p v-if="compliance.services.description" class="section-lead">
        {{ compliance.services.description }}
      </p>
    </div>
    <div class="services-grid">
      <div v-for="(svc, i) in compliance.services.items" :key="i" class="service-card">
        <span class="service-card-num">0{{ i + 1 }}</span>
        <h3>{{ svc.title }}</h3>
        <p>{{ svc.description }}</p>
        <ul>
          <li v-for="f in svc.features" :key="f">{{ f }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= COVERAGE ============= -->
  <section class="coverage-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">{{ compliance.coverage.subtitle }}</span>
        <h2 class="section-title">{{ compliance.coverage.title }}</h2>
        <p class="section-lead">{{ compliance.coverage.description }}</p>
      </div>
      <div class="coverage-grid">
        <div v-for="cat in compliance.coverage.categories" :key="cat.category" class="coverage-card">
          <span class="coverage-mark" aria-hidden>◆</span>
          <h3>{{ cat.category }}</h3>
          <ul>
            <li v-for="item in cat.items" :key="item">
              <span class="li-check" aria-hidden>✓</span> {{ item }}
            </li>
          </ul>
        </div>
      </div>
      <div class="coverage-guarantee">
        <span class="tag">Guarantee</span>
        <p>{{ compliance.coverage.guarantee }}</p>
      </div>
    </div>
  </section>

  <!-- ============= PROCESS ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">How it works</span>
      <h2 class="section-title">{{ compliance.process.title }}</h2>
      <p v-if="compliance.process.description" class="section-lead">
        {{ compliance.process.description }}
      </p>
    </div>
    <div class="how-grid how-grid-4">
      <div v-for="step in compliance.process.steps" :key="step.number" class="how-card">
        <span class="how-card-num">0{{ step.number }}</span>
        <h3>{{ step.title }}</h3>
        <p>{{ step.description }}</p>
        <ul>
          <li v-for="d in step.details" :key="d">{{ d }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= RISKS ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">{{ compliance.risks.subtitle }}</span>
      <h2 class="section-title">{{ compliance.risks.title }}</h2>
      <p class="section-lead">{{ compliance.risks.description }}</p>
    </div>
    <div class="risks-grid">
      <div v-for="risk in compliance.risks.risks" :key="risk.title" class="risk-card">
        <span class="risk-pill">{{ risk.impact }}</span>
        
        <div class="risk-icon-wrapper">
          <component :is="getIcon(risk.icon)" class="risk-icon" />
        </div>
        
        <h3>{{ risk.title }}</h3>
        <p>{{ risk.description }}</p>
        
        <div class="risk-footer">
          <span class="risk-footer-text">IMPACT: {{ risk.impact.toUpperCase() }}</span>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= EOR BRIDGE ============= -->
  <section class="eor-bridge">
    <div class="container">
      <div class="section-head">
        <h2 class="section-title">{{ compliance.eorBridge.title }}</h2>
        <p class="section-lead">{{ compliance.eorBridge.description }}</p>
      </div>
      <div class="eor-bridge-grid">
        <div v-for="(b, i) in compliance.eorBridge.benefits" :key="i" class="eor-bridge-card">
          <span class="eor-bridge-mark">{{ String(i + 1).padStart(2, '0') }}</span>
          <h3>{{ b.title }}</h3>
          <p>{{ b.description }}</p>
        </div>
      </div>
      <div class="eor-bridge-cta">
        <RouterLink to="/employer-of-record" class="btn-primary">
          Explore EOR services <span class="arrow">→</span>
        </RouterLink>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ compliance.faqs.title }}</h2>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in compliance.faqs.items"
          :key="i"
          class="faq-item"
          :class="{ open: openFaq === i }"
          @click="toggleFaq(i)"
          :aria-expanded="openFaq === i"
        >
          <span class="faq-q">{{ item.question }}</span>
          <span class="faq-toggle" aria-hidden>{{ openFaq === i ? '−' : '+' }}</span>
          <p v-show="openFaq === i" class="faq-a">{{ item.answer }}</p>
        </button>
      </div>
    </div>
  </section>

  <!-- ============= WARM CTA ============= -->
  <section class="cta-warm-wrap">
    <div class="cta-warm">
      <span class="cta-tag">Get started</span>
      <h2>{{ compliance.cta.title }}</h2>
      <p>{{ compliance.cta.description }}</p>
      <div class="cta-warm-features">
        <span v-for="f in compliance.cta.features" :key="f" class="cta-feature">
          <span class="cta-feature-mark" aria-hidden>✓</span> {{ f }}
        </span>
      </div>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=compliance_questions" class="btn-primary">
          {{ compliance.cta.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/employer-of-record" class="btn-secondary">Explore EOR</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.compliance-hero {
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  width: 100vw;
  box-sizing: border-box;
  padding: 88px 0 96px;
  display: block;
background-color: #f4f1ec;
    background-image: linear-gradient(90deg, #f4f1ec 0%, rgb(244 241 236) 30%, rgba(253, 251, 247, 0.4) 60%, rgba(253, 251, 247, 0) 100%), url(/compliance-hero-bg.png);background-size: 60% auto;
  background-position: right 0% center;
  background-repeat: no-repeat;
  color: var(--ink);
  min-height: 700px;
  overflow: hidden;
}

.compliance-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.4), transparent 50%);
  pointer-events: none;
}

.compliance-hero > * {
  position: relative;
  z-index: 1;
}

.compliance-hero-inner {
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 32px;
}

.compliance-hero .service-hero-copy {
  max-width: 720px;
  animation: fade-slide-up 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fade-slide-up {
  0% { opacity: 0; transform: translateY(30px); }
  100% { opacity: 1; transform: translateY(0); }
}

.compliance-hero .tag {
  color: var(--accent);
  background: rgba(176, 149, 89, 0.1);
  border: 1px solid rgba(176, 149, 89, 0.2);
  margin-bottom: 24px;
}

.compliance-hero h1 {
  color: var(--ink);
  text-shadow: none;
}

.compliance-hero .service-hero-copy h1 em {
  color: var(--accent, #b09559);
}

.compliance-hero .service-hero-lede {
  color: var(--ink-soft);
  text-shadow: none;
  font-size: 19px;
}

.compliance-hero .service-hero-features {
  display: flex;
  flex-direction: column;
  gap: 14px;
  margin-top: 32px;
  list-style: none;
  padding: 0;
}

.compliance-hero .hero-feature {
  display: flex;
  align-items: center;
  gap: 12px;
  color: var(--ink);
  font-size: 16px;
  font-weight: 500;
  letter-spacing: 0.02em;
  text-shadow: none;
}

.compliance-hero .hero-feature-check {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: rgba(176, 149, 89, 0.15);
  border: 1px solid rgba(176, 149, 89, 0.4);
  color: var(--accent, #b09559);
  font-size: 13px;
  font-weight: 800;
  flex-shrink: 0;
  box-shadow: 0 0 12px rgba(176, 149, 89, 0.2);
}

.compliance-hero .cta-row {
  margin-top: 40px;
}

.compliance-hero .btn-primary {
  background: var(--accent, #b09559);
  color: #ffffff;
  border: none;
  box-shadow: 0 8px 24px rgba(176, 149, 89, 0.3);
}

.compliance-hero .btn-primary:hover {
  background: #9a824e;
  transform: translateY(-2px);
  box-shadow: 0 12px 32px rgba(176, 149, 89, 0.4);
}

.compliance-hero .btn-secondary {
  background: rgba(0, 0, 0, 0.04);
  color: var(--ink);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.compliance-hero .btn-secondary:hover {
  background: rgba(0, 0, 0, 0.08);
  color: var(--ink);
}

.compliance-hero .trust-row {
  margin-top: 48px;
}

.compliance-hero .trust-text {
  color: var(--ink-soft);
}

.compliance-hero .avatar {
  border-color: #fdfbf7;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.08);
}

@media (max-width: 960px) {
  .compliance-hero {
    padding: 72px 24px 72px;
  }
}

@media (max-width: 640px) {
  .compliance-hero {
    padding: 56px 20px 64px;
  }
  .compliance-hero .service-hero-copy h1 {
    font-size: clamp(36px, 8vw, 48px);
  }
  .compliance-hero .cta-row {
    flex-direction: column;
    gap: 16px;
  }
  .compliance-hero .btn-primary,
  .compliance-hero .btn-secondary {
    width: 100%;
    justify-content: center;
  }
}

/* Compliance-specific tweaks */
.services-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 28px;
}
.service-card {
  background: linear-gradient(180deg, #fffaf3 0%, #f8f1e7 100%);
  border: 1px solid rgba(160, 125, 77, 0.18);
  border-radius: var(--radius-lg);
  padding: 34px 30px;
  box-shadow: 0 20px 40px rgba(115, 78, 42, 0.08);
  transition: transform 0.28s ease, box-shadow 0.28s ease;
}
.service-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 24px 50px rgba(115, 78, 42, 0.12);
}
.service-card-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 54px;
  height: 54px;
  border-radius: 50%;
  background: var(--accent);
  color: var(--bg);
  font-family: var(--serif);
  font-style: italic;
  font-size: 20px;
  margin-bottom: 22px;
}
.service-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 14px;
  color: var(--ink);
}
.service-card p {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.75;
  margin-bottom: 20px;
}
.service-card ul {
  list-style: none;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 12px 20px;
  border-top: 1px solid rgba(160, 125, 77, 0.12);
  padding-top: 18px;
  margin-top: 18px;
}
.service-card li {
  font-size: 13px;
  color: var(--ink-soft);
  position: relative;
  padding-left: 22px;
}
.service-card li::before {
  content: '✓';
  position: absolute;
  left: 0;
  top: 2px;
  color: var(--accent);
  font-weight: 700;
}

.how-grid-4 {
  grid-template-columns: repeat(4, minmax(0, 1fr));
}
.how-card {
  background: rgba(255, 255, 255, 0.95);
  border: 1px solid rgba(158, 118, 80, 0.14);
  border-radius: var(--radius-lg);
  padding: 34px 32px;
  box-shadow: 0 18px 40px rgba(105, 72, 41, 0.08);
  position: relative;
  overflow: hidden;
  transition: transform 0.28s ease, box-shadow 0.28s ease;
}
.how-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 24px 48px rgba(105, 72, 41, 0.12);
}
.how-card-num {
  position: relative;
  z-index: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--bg);
  color: var(--accent);
  font-family: var(--serif);
  font-style: italic;
  font-size: 20px;
  margin-bottom: 18px;
}
.how-card h3 {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 12px;
  color: var(--ink);
  position: relative;
  z-index: 1;
}
.how-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.75;
  margin-bottom: 18px;
  position: relative;
  z-index: 1;
}
.how-card ul {
  list-style: none;
  padding: 0;
  display: grid;
  gap: 10px;
  position: relative;
  z-index: 1;
}
.how-card li {
  font-size: 13px;
  color: var(--ink-soft);
  position: relative;
  padding-left: 20px;
}
.how-card li::before {
  content: '•';
  position: absolute;
  left: 0;
  top: 3px;
  color: var(--accent);
  font-size: 18px;
}

/* Coverage strip */
.coverage-strip {
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.coverage-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  margin-bottom: 32px;
}
.coverage-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px;
}
.coverage-mark {
  display: inline-flex;
  width: 36px;
  height: 36px;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  background: var(--accent-soft);
  color: var(--accent);
  font-size: 16px;
  margin-bottom: 16px;
}
.coverage-card h3 {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 16px;
}
.coverage-card ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.coverage-card li {
  font-size: 14px;
  color: var(--ink-soft);
  display: flex;
  gap: 10px;
  line-height: 1.5;
}
.li-check {
  color: var(--accent);
  flex-shrink: 0;
}
.coverage-guarantee {
  background: var(--ink);
  color: var(--bg);
  padding: 28px 36px;
  border-radius: var(--radius);
  display: flex;
  align-items: center;
  gap: 24px;
}
.coverage-guarantee .tag { color: var(--accent-warm); margin: 0; }
.coverage-guarantee p {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin: 0;
  line-height: 1.4;
}

/* Risks */
.risks-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 32px;
  margin-top: 48px;
}
.risk-card {
  background: #ffffff;
  border: 1px solid rgba(176, 149, 89, 0.15);
  border-radius: var(--radius-lg);
  padding: 40px 32px 32px;
  box-shadow: 0 16px 32px rgba(176, 149, 89, 0.04);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  position: relative;
  display: flex;
  flex-direction: column;
}
.risk-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 48px rgba(176, 149, 89, 0.08);
}
.risk-pill {
  position: absolute;
  top: -14px;
  left: 32px;
  background: var(--accent);
  color: #ffffff;
  font-size: 13px;
  letter-spacing: 0.02em;
  padding: 6px 16px;
  border-radius: 999px;
  font-weight: 500;
  box-shadow: 0 4px 12px rgba(176, 149, 89, 0.2);
}
.risk-icon-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 56px;
  height: 56px;
  background: rgba(176, 149, 89, 0.08);
  border-radius: 12px;
  margin-bottom: 24px;
}
.risk-icon {
  width: 28px;
  height: 28px;
  color: var(--accent);
}
.risk-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 500;
  margin-bottom: 14px;
  color: var(--ink);
}
.risk-card p {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.6;
  flex-grow: 1;
}
.risk-footer {
  margin-top: 32px;
  padding-top: 24px;
  border-top: 1px solid rgba(176, 149, 89, 0.15);
}
.risk-footer-text {
  font-size: 13px;
  font-weight: 600;
  color: var(--accent);
  letter-spacing: 0.05em;
}

/* EOR bridge */
.eor-bridge {
  border-top: 1px solid rgba(176, 149, 89, 0.15);
  border-bottom: 1px solid rgba(176, 149, 89, 0.15);
  padding: 100px 0;
}
.eor-bridge .tag { 
  color: var(--accent);
  background: rgba(176, 149, 89, 0.1);
  border: 1px solid rgba(176, 149, 89, 0.2);
}
.eor-bridge .section-title { color: var(--ink); }
.eor-bridge .section-title em { color: var(--accent); }
.eor-bridge .section-lead { color: var(--ink-soft); }
.eor-bridge-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  margin-bottom: 48px;
}
.eor-bridge-card {
  background: #ffffff;
  border: 1px solid rgba(176, 149, 89, 0.18);
  box-shadow: 0 16px 32px rgba(176, 149, 89, 0.04);
  border-radius: var(--radius-lg);
  padding: 36px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.eor-bridge-card:hover {
  box-shadow: 0 24px 48px rgba(176, 149, 89, 0.08);
  transform: translateY(-2px);
}
.eor-bridge-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 36px;
  color: var(--accent);
  margin-bottom: 16px;
}
.eor-bridge-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 12px;
  color: var(--ink);
}
.eor-bridge-card p {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.6;
}
.eor-bridge-cta {
  text-align: center;
}
.eor-bridge-cta .btn-primary {
  background: var(--accent);
  color: #ffffff;
}
.eor-bridge-cta .btn-primary:hover {
  background: #9a824e;
}

@media (max-width: 1024px) {
  .services-grid,
  .how-grid-4,
  .coverage-grid,
  .risks-grid,
  .eor-bridge-grid {
    grid-template-columns: 1fr;
  }
  .coverage-guarantee {
    flex-direction: column;
    align-items: flex-start;
    gap: 12px;
  }
}
</style>
