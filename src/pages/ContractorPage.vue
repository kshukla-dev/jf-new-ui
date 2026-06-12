<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import contractor from '@/data/contractor.json'

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
      <span class="tag">Service · Contractor management</span>
      <h1>
        Global <em>contractors</em>,<br />compliantly managed
      </h1>
      <p class="service-hero-lede">{{ contractor.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in contractor.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact?reason=contractor_management" class="btn-primary">
          {{ contractor.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ contractor.definition.secondaryButtonText.trim() }}
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
          Pay contractors in <strong>50+</strong> currencies, across 160+ countries
        </span>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="contractor.definition.image" :alt="contractor.definition.imageAlt" />
      <div class="service-hero-shape">F</div>
      <div class="service-hero-badge">
        <strong>24h payouts</strong>
        <span>Automated payment processing</span>
      </div>
    </div>
  </header>

  <!-- ============= MAIN DESCRIPTION ============= -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">What is contractor management</span>
        <h2 class="section-title">{{ contractor.mainDescription.title }}</h2>
      </div>
      <div class="definition-text">
        <p>{{ contractor.mainDescription.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= STATS STRIP ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in contractor.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- ============= BENEFITS ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Why contractors</span>
      <h2 class="section-title">{{ contractor.benefits.title }}</h2>
      <p class="section-lead">{{ contractor.benefits.subtitle }}</p>
    </div>
    <div class="benefits-grid">
      <div v-for="(b, i) in contractor.benefits.items" :key="i" class="benefit-card">
        <span class="benefit-mark">{{ String(i + 1).padStart(2, '0') }}</span>
        <h3>{{ b.title }}</h3>
        <p>{{ b.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= SOLUTIONS (Management + Compliance) ============= -->
  <section class="solutions-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">{{ contractor.solutions.subtitle }}</span>
        <h2 class="section-title">{{ contractor.solutions.title }}</h2>
        <p class="section-lead">{{ contractor.solutions.description }}</p>
      </div>
      <div class="solutions-grid">
        <div class="solution-block">
          <div class="solution-head">
            <span class="solution-tag">Management</span>
            <h3>{{ contractor.solutions.management.title }}</h3>
            <p>{{ contractor.solutions.management.description }}</p>
          </div>
          <ul>
            <li v-for="item in contractor.solutions.management.items" :key="item.title">
              <span class="li-check" aria-hidden>✓</span>
              <div>
                <strong>{{ item.title }}</strong>
                <span>{{ item.description }}</span>
              </div>
            </li>
          </ul>
        </div>
        <div class="solution-block solution-block-dark">
          <div class="solution-head">
            <span class="solution-tag">Compliance</span>
            <h3>{{ contractor.solutions.compliance.title }}</h3>
            <p>{{ contractor.solutions.compliance.description }}</p>
          </div>
          <ul>
            <li v-for="item in contractor.solutions.compliance.items" :key="item.title">
              <span class="li-check" aria-hidden>✓</span>
              <div>
                <strong>{{ item.title }}</strong>
                <span>{{ item.description }}</span>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ contractor.faqs.title }}</h2>
        <p v-if="contractor.faqs.subtitle" class="section-lead">{{ contractor.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in contractor.faqs.items"
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
      <h2>{{ contractor.cta.title }}</h2>
      <p>{{ contractor.cta.description }}</p>
      <div class="cta-warm-features">
        <span v-for="f in contractor.cta.features" :key="f" class="cta-feature">
          <span class="cta-feature-mark" aria-hidden>✓</span> {{ f }}
        </span>
      </div>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=contractor_management" class="btn-primary">
          {{ contractor.cta.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/employer-of-record" class="btn-secondary">
          Compare with EOR
        </RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

/* Benefits cards (3-col) */
.benefits-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.benefit-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px 32px;
}
.benefit-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 36px;
  color: var(--accent);
  margin-bottom: 16px;
}
.benefit-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 12px;
}
.benefit-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}

/* Solutions split: Management (light) vs Compliance (dark) */
.solutions-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.solutions-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}
.solution-block {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 40px;
  display: flex;
  flex-direction: column;
  gap: 28px;
}
.solution-block.solution-block-dark {
  background: var(--ink);
  color: var(--bg);
  border-color: var(--ink);
}
.solution-tag {
  display: inline-block;
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 12px;
}
.solution-block-dark .solution-tag { color: var(--accent-warm); }
.solution-head h3 {
  font-family: var(--serif);
  font-size: clamp(26px, 2.6vw, 36px);
  font-weight: 400;
  line-height: 1.15;
  margin-bottom: 12px;
  color: inherit;
}
.solution-block-dark .solution-head h3 { color: var(--bg); }
.solution-head p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}
.solution-block-dark .solution-head p { color: rgba(255, 255, 255, 0.7); }
.solution-block ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
  border-top: 1px solid var(--border);
  padding-top: 24px;
}
.solution-block-dark ul {
  border-top-color: rgba(255, 255, 255, 0.1);
}
.solution-block li {
  display: grid;
  grid-template-columns: 20px 1fr;
  gap: 12px;
}
.li-check {
  color: var(--accent);
  font-weight: 600;
  margin-top: 1px;
}
.solution-block-dark .li-check { color: var(--accent-warm); }
.solution-block li strong {
  display: block;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 2px;
  color: inherit;
}
.solution-block-dark li strong { color: var(--bg); }
.solution-block li span {
  font-size: 13px;
  color: var(--ink-soft);
  line-height: 1.5;
}
.solution-block-dark li span { color: rgba(255, 255, 255, 0.65); }

@media (max-width: 1024px) {
  .benefits-grid,
  .solutions-grid {
    grid-template-columns: 1fr;
  }
}
</style>
