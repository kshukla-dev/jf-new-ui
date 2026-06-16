<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import payroll from '@/data/payroll.json'

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
  <header class="service-hero payroll-hero">
    <div class="payroll-hero-inner">
      <div class="service-hero-copy">
        <h1>
        Global <em>payroll</em>,<br />simplified
      </h1>
      <p class="service-hero-lede">
        {{ payroll.definition.description }}
      </p>
      <ul class="service-hero-features">
        <li v-for="(f, i) in payroll.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-check">✓</span>
          {{ f }}
        </li>
      </ul>
      <div class="cta-row">
        <RouterLink to="/contact?reason=payroll_services" class="btn-primary">
          {{ payroll.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ payroll.definition.secondaryButtonText }}
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
          Processing payroll for <strong>700+</strong> teams worldwide
        </span>
      </div>
    </div>
  </div>
  </header>

  <!-- ============= MAIN DESCRIPTION ============= -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">What is payroll services</span>
        <h2 class="section-title">{{ payroll.mainDescription.title }}</h2>
      </div>
      <div class="definition-text">
        <p>{{ payroll.mainDescription.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= STATS STRIP ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in payroll.definition.trustSignals.stats" :key="i" class="stat-item">
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
      <h2 class="section-title">{{ payroll.services.title }}</h2>
      <p v-if="payroll.services.description" class="section-lead">
        {{ payroll.services.description }}
      </p>
    </div>
    <div class="services-grid">
      <div v-for="(svc, i) in payroll.services.items" :key="i" class="service-card">
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
      <h2 class="section-title">{{ payroll.process.title }}</h2>
      <p v-if="payroll.process.description" class="section-lead">
        {{ payroll.process.description }}
      </p>
    </div>
    <div class="how-grid how-grid-4">
      <div v-for="step in payroll.process.steps" :key="step.number" class="how-card">
        <span class="how-card-num">0{{ step.number }}</span>
        <h3>{{ step.title }}</h3>
        <p>{{ step.description }}</p>
        <ul>
          <li v-for="d in step.details" :key="d">{{ d }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= EOR COMPARISON ============= -->
  <section class="section container">
    <div class="section-head text-center">
      <span class="tag">{{ payroll.eorComparison.subtitle }}</span>
      <h2 class="section-title">{{ payroll.eorComparison.title }}</h2>
      <p class="section-lead mx-auto">{{ payroll.eorComparison.description }}</p>
    </div>
    <div class="comparison-grid comparison-grid-2 mx-auto">
      <div
        v-for="opt in payroll.eorComparison.options"
        :key="opt.type"
        class="comparison-card"
        :class="{ 'is-featured': opt.type.toLowerCase().includes('employer of record') }"
      >
        <div class="comparison-card-head">
          <h3>{{ opt.type }}</h3>
          <p>{{ opt.description }}</p>
        </div>
        <ul>
          <li v-for="f in opt.features" :key="f">
            <span class="li-mark" aria-hidden>✓</span> {{ f }}
          </li>
        </ul>
        <div class="comparison-bestfor">
          <span>Best for</span>
          <strong>{{ opt.bestFor }}</strong>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= TECHNOLOGY ============= -->
  <section class="tech-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">{{ payroll.technology.subtitle }}</span>
        <h2 class="section-title">{{ payroll.technology.title }}</h2>
        <p class="section-lead">{{ payroll.technology.description }}</p>
      </div>
      <div class="tech-grid">
        <div v-for="(f, i) in payroll.technology.features" :key="i" class="tech-card">
          <span class="tech-mark">{{ String(i + 1).padStart(2, '0') }}</span>
          <h3>{{ f.title }}</h3>
          <p>{{ f.description }}</p>
        </div>
      </div>
      <div class="tech-benefits">
        <span class="tag">Also includes</span>
        <div class="tech-benefits-pills">
          <span v-for="b in payroll.technology.benefits" :key="b" class="tech-pill">
            {{ b }}
          </span>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ payroll.faqs.title }}</h2>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in payroll.faqs.items"
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
      <h2>{{ payroll.cta.title }}</h2>
      <p>{{ payroll.cta.description }}</p>
      <div class="cta-warm-features">
        <span v-for="f in payroll.cta.features" :key="f" class="cta-feature">
          <span class="cta-feature-mark" aria-hidden>✓</span> {{ f }}
        </span>
      </div>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=payroll_services" class="btn-primary">
          {{ payroll.cta.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/cost-calculator" class="btn-secondary">Estimate cost</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.payroll-hero {
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  width: 100vw;
  box-sizing: border-box;
  padding: 88px 0 96px;
  display: block;
  background-image: linear-gradient(90deg, rgba(10, 15, 30, 0.85) 0%, rgba(10, 15, 30, 0.6) 40%, rgba(10, 15, 30, 0.2) 100%), url('https://cdn.craft.cloud/019cb01a-1d95-731f-9bc4-3a2fec394116/assets/images/Z_DELETE/Temp/608c78a14f189da3e50e174bca035c7fbd051996.jpg?fit=cover&format=webp&width=1280&s=cK_YnjDwH_igbP_kzd4CFxjdweGJjgJGK6Vq_7weuxE');
  background-size: cover;
  background-position: center 20%;
  color: #ffffff;
  min-height: 700px;
  overflow: hidden;
}

.payroll-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.08), transparent 34%);
  pointer-events: none;
}

.payroll-hero > * {
  position: relative;
  z-index: 1;
}

.payroll-hero-inner {
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 32px;
}

.payroll-hero .service-hero-copy {
  max-width: 720px;
  animation: fade-slide-up 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fade-slide-up {
  0% { opacity: 0; transform: translateY(30px); }
  100% { opacity: 1; transform: translateY(0); }
}

.payroll-hero .tag {
  color: rgba(255, 255, 255, 0.9);
  background: rgba(255, 255, 255, 0.12);
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.payroll-hero h1 {
  color: #ffffff;
  text-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
}

.payroll-hero .service-hero-copy h1 em {
  color: var(--accent, #b09559);
}

.payroll-hero .service-hero-lede {
  color: rgba(255, 255, 255, 0.9);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.3);
  font-size: 19px;
}

.payroll-hero .service-hero-features {
  display: flex;
  flex-direction: column;
  gap: 14px;
  margin-top: 32px;
  list-style: none;
  padding: 0;
}

.payroll-hero .hero-feature {
  display: flex;
  align-items: center;
  gap: 12px;
  color: #ffffff;
  font-size: 16px;
  font-weight: 500;
  letter-spacing: 0.02em;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.payroll-hero .hero-feature-check {
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

.payroll-hero .cta-row {
  margin-top: 40px;
}

.payroll-hero .btn-primary {
  background: var(--accent, #b09559);
  color: #ffffff;
  border: none;
  box-shadow: 0 8px 24px rgba(176, 149, 89, 0.3);
}

.payroll-hero .btn-primary:hover {
  background: #9a824e;
  transform: translateY(-2px);
  box-shadow: 0 12px 32px rgba(176, 149, 89, 0.4);
}

.payroll-hero .btn-secondary {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  color: #ffffff;
  border-color: rgba(255, 255, 255, 0.3);
}

.payroll-hero .btn-secondary:hover {
  background: rgba(255, 255, 255, 0.2);
  color: #ffffff;
}

.payroll-hero .trust-row {
  margin-top: 48px;
}

.payroll-hero .trust-text {
  color: rgba(255, 255, 255, 0.82);
}

.payroll-hero .avatar {
  border-color: rgba(255, 255, 255, 0.16);
  box-shadow: 0 18px 40px rgba(0, 0, 0, 0.24);
}

.payroll-hero .service-hero-visual {
  display: none;
}

@media (max-width: 960px) {
  .payroll-hero {
    padding: 72px 24px 72px;
  }

  .payroll-hero .service-hero-features {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .payroll-hero {
    min-height: auto;
    padding: 100px 20px 60px;
  }

  .payroll-hero .service-hero-copy h1 {
    font-size: clamp(36px, 8vw, 48px);
  }

  .payroll-hero .cta-row {
    flex-direction: column;
    gap: 16px;
  }

  .payroll-hero .btn-primary,
  .payroll-hero .btn-secondary {
    width: 100%;
    justify-content: center;
  }
}

/* Payroll-specific tweaks */
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

/* ============= PREMIUM COMPARISON CARDS ============= */
.section-head.text-center {
  text-align: center;
  margin: 0 auto 56px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.section-head.text-center .section-lead.mx-auto {
  margin-left: auto;
  margin-right: auto;
}

.comparison-grid-2 {
  grid-template-columns: repeat(2, 1fr);
  max-width: 960px;
  margin: 0 auto;
  gap: 32px;
}

.comparison-card {
  background: linear-gradient(180deg, #ffffff 0%, #fcfbf9 100%);
  border: 1px solid rgba(176, 149, 89, 0.2);
  border-radius: var(--radius-lg);
  padding: 44px 38px;
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.04);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.comparison-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 56px rgba(0, 0, 0, 0.08);
}

.comparison-card.is-featured {
  background: rgba(176, 149, 89, 0.04);
  color: var(--ink);
  border: 1.5px solid var(--accent);
  box-shadow: 0 24px 48px rgba(176, 149, 89, 0.1);
}

.comparison-card-head h3 {
  font-family: var(--serif);
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 12px;
}

.comparison-card-head p {
  font-size: 15px;
  opacity: 0.85;
  line-height: 1.6;
}

.comparison-card ul {
  margin-top: 12px;
  display: flex;
  flex-direction: column;
  gap: 14px;
  flex-grow: 1;
}

.comparison-card li {
  font-size: 15px;
  display: flex;
  align-items: flex-start;
  gap: 12px;
}

.comparison-card .li-mark {
  color: var(--accent);
  font-weight: 700;
  font-size: 18px;
  margin-top: -2px;
}

.comparison-bestfor {
  margin-top: 12px;
  border-top: 1px solid rgba(176, 149, 89, 0.15);
  padding-top: 24px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.comparison-card.is-featured .comparison-bestfor {
  border-top: 1px solid rgba(176, 149, 89, 0.3);
}

.comparison-bestfor span {
  font-size: 12px;
  letter-spacing: 0.15em;
  color: var(--accent);
  text-transform: uppercase;
}

.comparison-bestfor strong {
  font-size: 16px;
  font-weight: 500;
}

/* Technology strip */
.tech-strip {
  background: #f4f1ec;
 
  padding: 100px 0;
}
.tech-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-bottom: 48px;
}
.tech-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px;
}
.tech-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 32px;
  color: var(--accent);
  margin-bottom: 14px;
}
.tech-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
  color: var(--ink);
}
.tech-card p {
  font-size: 13px;
  color: var(--ink-soft);
  line-height: 1.55;
}
.tech-benefits .tag { margin-bottom: 16px; }
.tech-benefits-pills {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}
.tech-pill {
  font-size: 13px;
  padding: 8px 16px;
  border-radius: 999px;
  background: var(--bg);
  border: 1px solid var(--border);
  color: var(--ink-soft);
}

@media (max-width: 1024px) {
  .services-grid,
  .how-grid-4,
  .comparison-grid-2,
  .tech-grid {
    grid-template-columns: 1fr 1fr;
  }
}
@media (max-width: 640px) {
  .services-grid,
  .how-grid-4,
  .comparison-grid-2,
  .tech-grid {
    grid-template-columns: 1fr;
  }
}
</style>
