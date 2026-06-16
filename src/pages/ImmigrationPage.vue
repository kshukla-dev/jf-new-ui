<script setup lang="ts">
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
  <header class="container service-hero immigration-hero hero-premium">
    <div class="service-hero-copy">
      <h1>
        Hire, Relocate & Expand Globally <br /><em>Without Boundaries</em>
      </h1>
      <p class="service-hero-lede">
        From Employer of Record and Global Payroll to Immigration and Visa Support, Jackson & Frank helps businesses and talent move across borders with confidence.
      </p>
          <!--  <div class="hero-stat-grid">
              <div class="hero-stat-card">
                <strong>10,000+</strong>
                <span>Successful applications</span>
              </div>
              <div class="hero-stat-card">
                <strong>25+</strong>
                <span>Countries covered</span>
              </div>
              <div class="hero-stat-card">
                <strong>98%</strong>
                <span>Client satisfaction</span>
              </div>
            </div>
            -->
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
    <div class="service-hero-visual hero-visual-bg"></div>
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
  <section class="cta-warm-wrap">
    <div class="cta-warm">
      <span class="cta-tag">Get started</span>
      <h2>{{ immigration.cta.title }}</h2>
      <p>{{ immigration.cta.description }}</p>
      <div class="cta-warm-features">
        <span v-for="f in immigration.cta.features" :key="f" class="cta-feature">
          <span class="cta-feature-mark" aria-hidden>✓</span> {{ f }}
        </span>
      </div>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=immigration_services" class="btn-primary">
          {{ immigration.cta.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/employer-of-record" class="btn-secondary">Explore EOR</RouterLink>
      </div>
    </div>
  </section>
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

@media (max-width: 1024px) {
  .services-grid,
  .how-grid-4,
  .eor-bridge-grid {
    grid-template-columns: 1fr;
  }
}

</style>
