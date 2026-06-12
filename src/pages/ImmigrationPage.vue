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
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Service · Immigration</span>
      <h1>
        Work visas,<br /><em>handled</em> for you
      </h1>
      <p class="service-hero-lede">{{ immigration.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in immigration.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact?reason=immigration_services" class="btn-primary">
          {{ immigration.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ immigration.definition.secondaryButtonText }}
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
    <div class="service-hero-visual">
      <img :src="immigration.definition.image" :alt="immigration.definition.imageAlt" />
      <div class="service-hero-shape">I</div>
      <div class="service-hero-badge">
        <strong>14 days average</strong>
        <span>Visa processing time</span>
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

/* EOR bridge */
.eor-bridge {
  background: var(--ink);
  color: var(--bg);
  padding: 100px 0;
}
.eor-bridge .tag { color: var(--accent-warm); }
.eor-bridge .section-title { color: var(--bg); }
.eor-bridge .section-title em { color: var(--accent-warm); }
.eor-bridge .section-lead { color: rgba(255, 255, 255, 0.7); }
.eor-bridge-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  margin-bottom: 48px;
}
.eor-bridge-card {
  background: var(--dark-soft);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radius);
  padding: 32px;
}
.eor-bridge-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 32px;
  color: var(--accent-warm);
  margin-bottom: 14px;
}
.eor-bridge-card h3 {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 10px;
  color: var(--bg);
}
.eor-bridge-card p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.6;
}
.eor-bridge-cta {
  text-align: center;
}
.eor-bridge-cta .btn-primary {
  background: var(--bg);
  color: var(--ink);
}
.eor-bridge-cta .btn-primary:hover {
  background: var(--accent-warm);
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
