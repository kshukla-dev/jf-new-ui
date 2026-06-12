<script setup lang="ts">
import { ref, computed } from 'vue'
import { RouterLink } from 'vue-router'
import ghg from '@/data/global-hiring.json'

interface Country {
  name: string
  status?: string
  address: string
  href: string
}

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

const allCountries = ghg.countries as Country[]
const available = computed(() => allCountries.filter((c) => c.status?.toLowerCase() !== 'coming soon'))
const comingSoon = computed(() => allCountries.filter((c) => c.status?.toLowerCase() === 'coming soon'))
</script>

<template>
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Resources · Global hiring guide</span>
      <h1>Hire anywhere,<br /><em>one partner</em></h1>
      <p class="service-hero-lede">{{ ghg.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in ghg.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact?reason=general_inquiry" class="btn-primary">
          Book a demo <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/employer-of-record" class="btn-secondary">Learn more</RouterLink>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="ghg.definition.image" :alt="ghg.definition.imageAlt" />
      <div class="service-hero-shape">G</div>
      <div class="service-hero-badge">
        <strong>160+ countries</strong>
        <span>Payroll under one roof</span>
      </div>
    </div>
  </header>

  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in ghg.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span class="stat-desc">{{ s.label }}</span>
      </div>
    </div>
  </section>

  <!-- Benefits -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Why global hiring</span>
      <h2 class="section-title">Built for <em>borderless</em> teams</h2>
    </div>
    <div class="ghg-benefits">
      <div v-for="(b, i) in ghg.benefits" :key="i" class="ghg-benefit">
        <span class="ghg-benefit-mark">{{ String(i + 1).padStart(2, '0') }}</span>
        <h3>{{ b.title }}</h3>
        <p>{{ b.description }}</p>
      </div>
    </div>
  </section>

  <!-- Process -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">How it works</span>
      <h2 class="section-title">From candidate to <em>compliant hire</em></h2>
    </div>
    <div class="how-grid how-grid-4">
      <div v-for="step in ghg.process" :key="step.step" class="how-card">
        <span class="how-card-num">{{ step.step }}</span>
        <h3>{{ step.title }}</h3>
        <p>{{ step.description }}</p>
      </div>
    </div>
  </section>

  <!-- Entities & locations -->
  <section class="entities-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Local infrastructure</span>
        <h2 class="section-title">Our entities &amp; <em>locations</em></h2>
        <p class="section-lead">Our global network spans multiple continents, bringing you local expertise wherever you need it.</p>
      </div>
      <div class="entities-grid">
        <RouterLink
          v-for="c in available"
          :key="c.name"
          :to="c.href || '/contact'"
          class="entity-card"
        >
          <h3>{{ c.name }}</h3>
          <p>{{ c.address }}</p>
          <span class="entity-cta">Learn more <span aria-hidden>→</span></span>
        </RouterLink>
      </div>

      <!-- Coming soon -->
      <div v-if="comingSoon.length" class="entities-soon">
        <h3 class="entities-soon-title">Coming soon</h3>
        <div class="entities-grid">
          <div v-for="c in comingSoon" :key="c.name" class="entity-card entity-card-soon">
            <span class="entity-soon-badge">Coming soon</span>
            <h3>{{ c.name }}</h3>
            <p>{{ c.address }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- FAQ -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ ghg.faqs.title }}</h2>
        <p class="section-lead">{{ ghg.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in ghg.faqs.items"
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

  <section class="cta-warm-wrap">
    <div class="cta-warm">
      <span class="cta-tag">Get started</span>
      <h2>Start hiring <em>globally</em> today</h2>
      <p>Join 700+ companies hiring across 160+ countries with Jackson &amp; Frank.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=general_inquiry" class="btn-primary">
          Book a demo <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/cost-calculator" class="btn-secondary">Estimate cost</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.how-grid-4 { grid-template-columns: repeat(4, 1fr); }

.ghg-benefits {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.ghg-benefit {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px 28px;
}
.ghg-benefit-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 32px;
  color: var(--accent);
  margin-bottom: 14px;
}
.ghg-benefit h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
}
.ghg-benefit p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}

.entities-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.entities-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}
.entity-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 24px;
  text-decoration: none;
  color: var(--ink);
  transition: border-color 0.2s, transform 0.2s;
  display: flex;
  flex-direction: column;
}
.entity-card:hover {
  border-color: var(--accent);
  transform: translateY(-2px);
}
.entity-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
}
.entity-card p {
  font-size: 12px;
  color: var(--ink-soft);
  line-height: 1.5;
  flex-grow: 1;
  margin-bottom: 14px;
}
.entity-cta {
  font-size: 13px;
  font-weight: 500;
  color: var(--accent);
}
.entities-soon { margin-top: 48px; }
.entities-soon-title {
  font-family: var(--serif);
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 24px;
}
.entity-card-soon {
  opacity: 0.75;
  cursor: default;
}
.entity-soon-badge {
  display: inline-block;
  align-self: flex-start;
  font-size: 10px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  background: var(--accent-soft);
  color: var(--accent);
  padding: 4px 10px;
  border-radius: 999px;
  margin-bottom: 12px;
}

@media (max-width: 1024px) {
  .how-grid-4, .ghg-benefits, .entities-grid { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 640px) {
  .how-grid-4, .ghg-benefits, .entities-grid { grid-template-columns: 1fr; }
}
</style>
