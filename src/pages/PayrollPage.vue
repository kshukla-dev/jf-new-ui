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
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Service · Payroll</span>
      <h1>
        Global <em>payroll</em>,<br />simplified
      </h1>
      <p class="service-hero-lede">
        {{ payroll.definition.description }}
      </p>
      <div class="service-hero-features">
        <div v-for="(f, i) in payroll.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
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
    <div class="service-hero-visual">
      <img :src="payroll.definition.image" :alt="payroll.definition.imageAlt" />
      <div class="service-hero-shape">P</div>
      <div class="service-hero-badge">
        <strong>99.9% accurate</strong>
        <span>Across 160+ countries</span>
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
    <div class="section-head">
      <span class="tag">{{ payroll.eorComparison.subtitle }}</span>
      <h2 class="section-title">{{ payroll.eorComparison.title }}</h2>
      <p class="section-lead">{{ payroll.eorComparison.description }}</p>
    </div>
    <div class="comparison-grid comparison-grid-2">
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

/* Payroll-specific tweaks */
.services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}
.service-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px;
}
.service-card-num {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 36px;
  color: var(--accent);
  margin-bottom: 16px;
}
.service-card h3 {
  font-family: var(--serif);
  font-size: 26px;
  font-weight: 400;
  margin-bottom: 12px;
  color: var(--ink);
}
.service-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
  margin-bottom: 18px;
}
.service-card ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
  border-top: 1px solid var(--border);
  padding-top: 16px;
}
.service-card li {
  font-size: 13px;
  color: var(--ink-soft);
  position: relative;
  padding-left: 18px;
}
.service-card li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: var(--accent);
  font-weight: 600;
}

.how-grid-4 {
  grid-template-columns: repeat(4, 1fr);
}

.comparison-grid-2 {
  grid-template-columns: repeat(2, 1fr);
  max-width: 920px;
}

/* Technology strip */
.tech-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
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
