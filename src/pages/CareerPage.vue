<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import career from '@/data/career.json'

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}
</script>

<template>
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Careers</span>
      <h1>Build the future of <em>global HR</em></h1>
      <p class="service-hero-lede">{{ career.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in career.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <a href="#open-positions" class="btn-primary">
          {{ career.definition.primaryButtonText }} <span class="arrow">→</span>
        </a>
        <RouterLink to="/about-us" class="btn-secondary">
          {{ career.definition.secondaryButtonText }}
        </RouterLink>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="career.definition.image" :alt="career.definition.imageAlt" />
      <div class="service-hero-shape">↗</div>
      <div class="service-hero-badge">
        <strong>Remote-first</strong>
        <span>Work from anywhere in the world</span>
      </div>
    </div>
  </header>

  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in career.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- Benefits -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Why work here</span>
      <h2 class="section-title">Benefits that <em>matter</em></h2>
    </div>
    <div class="benefits-grid">
      <div v-for="(b, i) in career.benefits" :key="i" class="benefit-card">
        <span class="benefit-mark">{{ String(i + 1).padStart(2, '0') }}</span>
        <h3>{{ b.title }}</h3>
        <p>{{ b.description }}</p>
      </div>
    </div>
  </section>

  <!-- Open positions -->
  <section id="open-positions" class="positions-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Open roles</span>
        <h2 class="section-title">Current <em>openings</em></h2>
        <p class="section-lead">Join a mission-driven team building the infrastructure for the future of work.</p>
      </div>
      <div class="positions-list">
        <a
          v-for="(p, i) in career.openPositions"
          :key="i"
          :href="p.href"
          target="_blank"
          rel="noopener"
          class="position-card"
        >
          <div class="position-main">
            <div class="position-meta">
              <span>{{ p.department }}</span>
              <span>·</span>
              <span>{{ p.location }}</span>
              <span>·</span>
              <span>{{ p.type }}</span>
            </div>
            <h3>{{ p.title }}</h3>
            <p>{{ p.description }}</p>
          </div>
          <span class="position-apply">
            Apply <span aria-hidden>→</span>
          </span>
        </a>
      </div>
    </div>
  </section>

  <!-- FAQ -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ career.faqs.title }}</h2>
        <p class="section-lead">{{ career.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in career.faqs.items"
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
      <span class="cta-tag">Join us</span>
      <h2>Don't see the right <em>role?</em></h2>
      <p>We're always looking for talented people. Send us your details and we'll be in touch.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=careers" class="btn-primary">
          Get in touch <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/about-us" class="btn-secondary">About our culture</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.benefits-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.benefit-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px 28px;
}
.benefit-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 32px;
  color: var(--accent);
  margin-bottom: 14px;
}
.benefit-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
}
.benefit-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}

.positions-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
  scroll-margin-top: 80px;
}
.positions-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.position-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px 36px;
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 24px;
  align-items: center;
  transition: border-color 0.2s, transform 0.2s;
}
.position-card:hover {
  border-color: var(--accent);
  transform: translateY(-2px);
}
.position-meta {
  display: flex;
  gap: 8px;
  font-size: 12px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 10px;
}
.position-main h3 {
  font-family: var(--serif);
  font-size: 26px;
  font-weight: 400;
  margin-bottom: 10px;
}
.position-main p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
  max-width: 720px;
}
.position-apply {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 22px;
  border-radius: 999px;
  background: var(--ink);
  color: var(--bg);
  font-size: 13px;
  font-weight: 500;
  white-space: nowrap;
  transition: background 0.2s;
}
.position-card:hover .position-apply {
  background: var(--accent);
}

@media (max-width: 1024px) {
  .benefits-grid { grid-template-columns: 1fr 1fr; }
  .position-card { grid-template-columns: 1fr; }
}
@media (max-width: 640px) {
  .benefits-grid { grid-template-columns: 1fr; }
}
</style>
