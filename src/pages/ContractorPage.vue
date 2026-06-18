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
  <header class="service-hero contractor-hero">
    <div class="contractor-hero-inner">
      <div class="service-hero-copy">
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

/* ============================================================
   HERO
   ============================================================ */
.contractor-hero {
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
  background-image: linear-gradient(90deg, #f4f1ec 0%, rgb(244 241 236 / 0%) 30%, rgba(253, 251, 247, 0.4) 60%, rgba(253, 251, 247, 0) 100%), url(/services/service-page/contractor-hero.jpg);
  background-size: 50% auto;
  background-position: right 5% center;
  background-repeat: no-repeat;
  color: var(--ink);
  min-height: 700px;
  overflow: hidden;
}

.contractor-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at top right, rgba(255, 255, 255, 0.4), transparent 50%);
  pointer-events: none;
}

.contractor-hero > * {
  position: relative;
  z-index: 1;
}

.contractor-hero-inner {
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 32px;
}

.contractor-hero .service-hero-copy {
  max-width: 720px;
  animation: fade-slide-up 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fade-slide-up {
  0% { opacity: 0; transform: translateY(30px); }
  100% { opacity: 1; transform: translateY(0); }
}

.contractor-hero .tag {
  color: var(--accent);
  background: rgba(176, 149, 89, 0.1);
  border: 1px solid rgba(176, 149, 89, 0.2);
  margin-bottom: 24px;
}

.contractor-hero h1 {
  color: var(--ink);
  text-shadow: none;
}

.contractor-hero .service-hero-copy h1 em {
  color: var(--accent, #b09559);
}

.contractor-hero .service-hero-lede {
  color: var(--ink-soft);
  text-shadow: none;
  font-size: 19px;
}

.contractor-hero .service-hero-features {
  display: flex;
  flex-direction: column;
  gap: 14px;
  margin-top: 32px;
  list-style: none;
  padding: 0;
}

.contractor-hero .hero-feature {
  display: flex;
  align-items: center;
  gap: 12px;
  color: var(--ink);
  font-size: 16px;
  font-weight: 500;
  letter-spacing: 0.02em;
  text-shadow: none;
}

.contractor-hero .hero-feature-dot {
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

.contractor-hero .hero-feature-dot::after {
  content: '✓';
}

.contractor-hero .cta-row {
  margin-top: 40px;
}

.contractor-hero .btn-primary {
  background: var(--accent, #b09559);
  color: #ffffff;
  border: none;
  box-shadow: 0 8px 24px rgba(176, 149, 89, 0.3);
}

.contractor-hero .btn-primary:hover {
  background: #9a824e;
  transform: translateY(-2px);
  box-shadow: 0 12px 32px rgba(176, 149, 89, 0.4);
}

.contractor-hero .btn-secondary {
  background: rgba(0, 0, 0, 0.04);
  color: var(--ink);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.contractor-hero .btn-secondary:hover {
  background: rgba(0, 0, 0, 0.08);
  color: var(--ink);
}

.contractor-hero .trust-row {
  margin-top: 48px;
}

.contractor-hero .trust-text {
  color: var(--ink-soft);
}

.contractor-hero .avatar {
  border-color: #fdfbf7;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.08);
}

@media (max-width: 960px) {
  .contractor-hero {
    padding: 72px 24px 72px;
    background-size: 70% auto;
        background-position: top 30% right;
  }
}

@media (max-width: 640px) {
  .contractor-hero {
    min-height: auto;
    padding: 260px 20px 60px;
    background-image: none;
    background-color: #f4f1ec;
  }
  .contractor-hero::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 280px;
    background-image: url(/services/service-page/contractor-hero.jpg);
    background-size: 100% auto;
    background-position: top center;
    background-repeat: no-repeat;

    -webkit-mask-image: linear-gradient(to bottom, black 50%, transparent 100%);
    mask-image: linear-gradient(to bottom, black 50%, transparent 100%);
    pointer-events: none;
  }
  .contractor-hero-inner {
    padding-left: 0;
    padding-right: 0;
  }
  .contractor-hero .service-hero-copy h1 {
    font-size: clamp(36px, 8vw, 48px);
  }
  .contractor-hero .cta-row {
    flex-direction: column;
    gap: 16px;
  }
  .contractor-hero .btn-primary,
  .contractor-hero .btn-secondary {
    width: 100%;
    justify-content: center;
  }
}


/* Benefits cards (3-col) */
.benefits-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.benefit-card {
  background: linear-gradient(180deg, #fffaf3 0%, #f8f1e7 100%);
  border: 1px solid rgba(160, 125, 77, 0.18);
  border-radius: var(--radius-lg);
  padding: 36px 32px;
  box-shadow: 0 20px 40px rgba(115, 78, 42, 0.08);
  transition: transform 0.28s ease, box-shadow 0.28s ease;
  position: relative;
  overflow: hidden;
}
.benefit-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 24px 50px rgba(115, 78, 42, 0.12);
}
.benefit-mark {
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
.benefit-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 14px;
  color: var(--ink);
}
.benefit-card p {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.75;
}

/* Solutions split: Management (light) vs Compliance (dark) */
.solutions-strip {
  background: #fdfbf7;
  border-top: 1px solid rgba(176, 149, 89, 0.15);
  border-bottom: 1px solid rgba(176, 149, 89, 0.15);
  padding: 100px 0;
}
.solutions-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}
.solution-block {
  background: #ffffff;
  border: 1px solid rgba(176, 149, 89, 0.18);
  box-shadow: 0 16px 32px rgba(176, 149, 89, 0.04);
  border-radius: var(--radius-lg);
  padding: 40px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.solution-block:hover {
  transform: translateY(-2px);
  box-shadow: 0 24px 48px rgba(176, 149, 89, 0.08);
}
.solution-block.solution-block-dark {
  background: var(--ink);
  color: var(--bg);
  border-color: var(--ink);
  box-shadow: 0 16px 32px rgba(0, 0, 0, 0.2);
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
