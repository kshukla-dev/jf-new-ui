<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import advantages from '@/data/advantages.json'

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}
</script>

<template>
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Our advantages</span>
      <h1>Why teams choose <em>Jackson &amp; Frank</em></h1>
      <p class="service-hero-lede">{{ advantages.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in advantages.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact" class="btn-primary">
          {{ advantages.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ advantages.definition.secondaryButtonText }}
        </RouterLink>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="advantages.definition.image" :alt="advantages.definition.imageAlt" />
      <div class="service-hero-shape">✦</div>
      <div class="service-hero-badge">
        <strong>50+ years</strong>
        <span>Of combined HR expertise</span>
      </div>
    </div>
  </header>

  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in advantages.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- Advantages list -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">What sets us apart</span>
      <h2 class="section-title">{{ advantages.advantages.title }}</h2>
      <p class="section-lead">{{ advantages.advantages.description }}</p>
    </div>
    <div class="adv-grid">
      <div v-for="(adv, i) in advantages.advantages.items" :key="i" class="adv-card">
        <span class="adv-num">{{ String(i + 1).padStart(2, '0') }}</span>
        <h3>{{ adv.title }}</h3>
        <ul>
          <li v-for="p in adv.points" :key="p">
            <span class="li-check" aria-hidden>✓</span> {{ p }}
          </li>
        </ul>
      </div>
    </div>
  </section>

  <!-- Certifications -->
  <section class="cert-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Trust &amp; compliance</span>
        <h2 class="section-title">{{ advantages.certifications.title }}</h2>
      </div>
      <div class="cert-grid">
        <div v-for="cert in advantages.certifications.items" :key="cert.title" class="cert-card">
          <h3>{{ cert.title }}</h3>
          <ul>
            <li v-for="p in cert.points" :key="p">
              <span class="li-check" aria-hidden>✓</span> {{ p }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- FAQ -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ advantages.faqs.title }}</h2>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in advantages.faqs.items"
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
      <h2>Ready to work with a <em>real partner?</em></h2>
      <p>See how our advantages translate into smoother global expansion for your team.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact" class="btn-primary">
          Talk to our team <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/testimonials" class="btn-secondary">Read client stories</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.adv-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}
.adv-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px;
}
.adv-num {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 34px;
  color: var(--accent);
  margin-bottom: 14px;
}
.adv-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 18px;
}
.adv-card ul,
.cert-card ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.adv-card li,
.cert-card li {
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

.cert-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.cert-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.cert-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px;
}
.cert-card h3 {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 18px;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--border);
}

@media (max-width: 1024px) {
  .adv-grid, .cert-grid { grid-template-columns: 1fr; }
}
</style>
