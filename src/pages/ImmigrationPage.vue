<script setup lang="ts">
import GlobalCTA from '@/components/sections/GlobalCTA.vue'
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import immigration from '@/data/immigration.json'

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
  <header class="service-hero immigration-premium-hero">
    <div class="immigration-premium-hero-inner">
      <div class="service-hero-copy">
        <h1>
          Hire, Relocate & Expand Globally <br /><em>Without Boundaries</em>
        </h1>
        <p class="service-hero-lede">
          From Employer of Record and Global Payroll to Immigration and Visa Support, Jackson & Frank helps businesses and talent move across borders with confidence.
        </p>
        <div class="cta-row">
          <RouterLink to="/contact?reason=immigration_services" class="btn-primary">
            Get Free Consultation <span class="arrow">→</span>
          </RouterLink>
          <RouterLink to="/services" class="btn-secondary">
            Explore Services
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
            <strong>99.5%</strong> visa approval rate across 160+ countries
          </span>
        </div>
      </div>
    </div>
  </header>

  <!-- ============= MAIN DESCRIPTION ============= -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">What is immigration services</span>
        <h2 class="section-title">{{ immigration.mainDescription.title }}</h2>
      </div>
      <div class="definition-text">
        <p>{{ immigration.mainDescription.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= STATS STRIP ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in immigration.definition.trustSignals.stats" :key="i" class="stat-item">
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
      <h2 class="section-title">{{ immigration.services.title }}</h2>
      <p v-if="immigration.services.description" class="section-lead">
        {{ immigration.services.description }}
      </p>
    </div>
    <div class="services-grid">
      <div v-for="(svc, i) in immigration.services.items" :key="i" class="service-card">
        <span class="service-card-num">0{{ i + 1 }}</span>
        <h3>{{ svc.title }}</h3>
        <p>{{ svc.description }}</p>
        <ul>
          <li v-for="f in svc.features" :key="f">{{ f }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= PROCESS ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">How it works</span>
      <h2 class="section-title">{{ immigration.process.title }}</h2>
      <p v-if="immigration.process.description" class="section-lead">
        {{ immigration.process.description }}
      </p>
    </div>
    <div class="how-grid how-grid-4">
      <div v-for="step in immigration.process.steps" :key="step.number" class="how-card">
        <span class="how-card-num">0{{ step.number }}</span>
        <h3>{{ step.title }}</h3>
        <p>{{ step.description }}</p>
        <ul>
          <li v-for="d in step.details" :key="d">{{ d }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= EOR BRIDGE ============= -->
  <section class="eor-bridge">
    <div class="container">
      <div class="section-head">
        <span class="tag">{{ immigration.eorBridge.subtitle }}</span>
        <h2 class="section-title">{{ immigration.eorBridge.title }}</h2>
        <p class="section-lead">{{ immigration.eorBridge.description }}</p>
      </div>
      <div class="eor-bridge-grid">
        <div v-for="(b, i) in immigration.eorBridge.benefits" :key="i" class="eor-bridge-card">
          <span class="eor-bridge-mark">{{ String(i + 1).padStart(2, '0') }}</span>
          <h3>{{ b.title }}</h3>
          <p>{{ b.description }}</p>
        </div>
      </div>
      <div class="eor-bridge-cta">
        <RouterLink :to="immigration.eorBridge.ctaHref" class="btn-primary">
          {{ immigration.eorBridge.ctaText }} <span class="arrow">→</span>
        </RouterLink>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ immigration.faqs.title }}</h2>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in immigration.faqs.items"
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
  <GlobalCTA title="Ready to streamline your immigration process?" />
</template>

<style scoped>
@import '@/styles/service-page.css';

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
.how-card::before {
  content: '';
  position: absolute;
  inset: 0;
  left: auto;
  width: 6px;
  background: linear-gradient(180deg, var(--accent), #b69a72);
  border-radius: 999px;
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

/* EOR bridge */
.eor-bridge {
  color: var(--ink);
  padding: 120px 0;
}
.eor-bridge .section-head {
  max-width: 720px;
  margin-bottom: 42px;
}
.eor-bridge .tag {
  color: var(--accent-warm);
}
.eor-bridge .section-title {
  color: var(--ink);
}
.eor-bridge .section-title em {
  color: var(--accent);
}
.eor-bridge .section-lead {
  color: var(--ink-soft);
  line-height: 1.75;
  max-width: 680px;
}
.eor-bridge-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 28px;
  margin-bottom: 52px;
}
.eor-bridge-card {
  background: rgba(255, 255, 255, 0.96);
  border: 1px solid rgba(130, 94, 58, 0.14);
  border-radius: var(--radius-lg);
  padding: 34px;
  box-shadow: 0 22px 55px rgba(77, 52, 30, 0.08);
  transition: transform 0.28s ease, box-shadow 0.28s ease;
}
.eor-bridge-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 28px 65px rgba(77, 52, 30, 0.12);
}
.eor-bridge-mark {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 52px;
  height: 52px;
  border-radius: 50%;
  font-family: var(--serif);
  font-style: italic;
  font-size: 20px;
  color: var(--bg);
  background: var(--accent);
  margin-bottom: 18px;
}
.eor-bridge-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 14px;
  color: var(--ink);
}
.eor-bridge-card p {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.75;
}
.eor-bridge-cta {
  text-align: center;
}
.eor-bridge-cta .btn-primary {
  background: var(--accent);
  color: var(--bg);
  border: 1px solid transparent;
}
.eor-bridge-cta .btn-primary:hover {
  background: #9d7b4f;
  color: var(--bg);
}
.eor-bridge-cta .btn-secondary {
  margin-left: 12px;
  border-color: rgba(26, 26, 26, 0.16);
  color: var(--ink);
}
.eor-bridge-cta .btn-secondary:hover {
  background: rgba(26, 26, 26, 0.04);
  color: var(--ink);
}

.immigration-premium-hero {
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  width: 100vw;
  box-sizing: border-box;
  padding: 30px 0 96px;
  display: block;
  background-color: #f4f1ec;
  background-image: linear-gradient(90deg, #f4f1ec 0%, rgb(244 241 236 / 0%) 30%, rgba(253, 251, 247, 0.6) 55%, rgba(253, 251, 247, 0) 100%), url(/case-study/immigration.png);
  background-size: 62% auto;
  background-position: right 32px center;
  background-repeat: no-repeat;
  color: #061639; /* ink */
  min-height: 700px;
  overflow: hidden;
  margin-bottom: 40px;
}

.immigration-premium-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.4), transparent 50%);
  pointer-events: none;
}

.immigration-premium-hero > * {
  position: relative;
  z-index: 1;
}

.immigration-premium-hero-inner {
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 32px;
}

.immigration-premium-hero .service-hero-copy {
  max-width: 720px;
  animation: fade-slide-up 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fade-slide-up {
  0% { opacity: 0; transform: translateY(30px); }
  100% { opacity: 1; transform: translateY(0); }
}

.immigration-premium-hero h1 {
  font-family: 'Libre Caslon Text', serif;
  font-size: clamp(48px, 5.8vw, 86px);
  line-height: 1.05;
  letter-spacing: -0.02em;
  font-weight: 400;
  margin-top: 0;
  margin-bottom: 24px;
}

.immigration-premium-hero h1 em {
  font-style: italic;
  color: #b09559;
}

.immigration-premium-hero .service-hero-lede {
  color: rgba(6, 22, 57, 0.7); /* ink-soft */
  font-size: 19px;
  margin-top: 24px;
  max-width: 520px;
  line-height: 1.6;
}

@media (max-width: 960px) {
  .immigration-premium-hero {
    padding: 72px 24px 72px;
    background-size: cover;
    background-position: center;
    background-image: linear-gradient(rgba(244, 241, 236, 0.8), rgba(244, 241, 236, 0.95)), url(/case-study/immigration.png);
  }
}

@media (max-width: 1024px) {
  .services-grid,
  .how-grid-4,
  .eor-bridge-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .immigration-premium-hero {
    min-height: auto;
    padding: 200px 20px 64px;
    background-image: none;
    background-color: #f4f1ec;
  }
  .immigration-premium-hero::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 220px;
    background-image: url(/case-study/immigration.png);
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    -webkit-mask-image: linear-gradient(to bottom, black 55%, transparent 100%);
    mask-image: linear-gradient(to bottom, black 55%, transparent 100%);
    pointer-events: none;
  }
  .immigration-premium-hero h1 {
    font-size: clamp(36px, 8vw, 48px);
  }
  .immigration-premium-hero .cta-row {
    flex-direction: column;
    gap: 16px;
  }
  .immigration-premium-hero .btn-primary,
  .immigration-premium-hero .btn-secondary {
    width: 100%;
    justify-content: center;
  }
}
</style>
