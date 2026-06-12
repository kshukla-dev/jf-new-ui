<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import eor from '@/data/eor.json'

// FAQ accordion state
const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

// Trust avatars from real testimonials
const trustAvatars = [
  '/testimonials/lina.jpg',
  '/testimonials/Anya.jpg',
  '/testimonials/priya.jpg',
]

const heroImage = eor.definition.image
</script>

<template>
  <!-- ============= HERO ============= -->
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">Service · Employer of Record</span>
      <h1>
        Hire <em>globally</em>,<br />without local entities
      </h1>
      <p class="service-hero-lede">
        {{ eor.definition.description }}
      </p>
      <div class="service-hero-features">
        <div v-for="(f, i) in eor.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact?reason=eor_services" class="btn-primary">
          {{ eor.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ eor.definition.secondaryButtonText }}
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
          Trusted by <strong>700+</strong> teams across 160+ countries
        </span>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="heroImage" :alt="eor.definition.imageAlt" />
      <div class="service-hero-shape">E</div>
      <div class="service-hero-badge">
        <strong>48–72 hour</strong>
        <span>Typical first hire go-live</span>
      </div>
    </div>
  </header>

  <!-- ============= DEFINITION / WHAT IS EOR ============= -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">What is EOR</span>
        <h2 class="section-title">{{ eor.mainDescription.title }}</h2>
      </div>
      <div class="definition-text">
        <p v-for="(p, i) in eor.mainDescription.paragraphs" :key="i">{{ p }}</p>
      </div>
    </div>
  </section>

  <!-- ============= TRUST SIGNALS STATS ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in eor.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- ============= HOW IT WORKS ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">{{ eor.howItWorks.subtitle || 'How it works' }}</span>
      <h2 class="section-title">{{ eor.howItWorks.title }}</h2>
      <p v-if="eor.howItWorks.description" class="section-lead">
        {{ eor.howItWorks.description }}
      </p>
    </div>
    <div class="how-grid">
      <div v-for="step in eor.howItWorks.steps" :key="step.number" class="how-card">
        <span class="how-card-num">0{{ step.number }}</span>
        <h3>{{ step.title }}</h3>
        <p>{{ step.description }}</p>
        <ul>
          <li v-for="d in step.details" :key="d">{{ d }}</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- ============= WHAT'S INCLUDED ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">What's included</span>
      <h2 class="section-title">{{ eor.included.title }}</h2>
      <p v-if="eor.included.description" class="section-lead">
        {{ eor.included.description }}
      </p>
    </div>
    <div class="included-grid">
      <div v-for="(f, i) in eor.included.features" :key="i" class="included-card">
        <span class="included-card-mark">{{ String(i + 1).padStart(2, '0') }}</span>
        <h3>{{ f.title }}</h3>
        <p>{{ f.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= COMPARISON ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">{{ eor.comparison.subtitle }}</span>
      <h2 class="section-title">{{ eor.comparison.title }}</h2>
      <p class="section-lead">{{ eor.comparison.description }}</p>
    </div>
    <div class="comparison-grid">
      <div
        v-for="opt in eor.comparison.options"
        :key="opt.type"
        class="comparison-card"
        :class="{ 'is-featured': opt.type.includes('Employer of Record') }"
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

  <!-- ============= COUNTRIES STRIP ============= -->
  <section class="countries-strip">
    <div class="container">
      <div class="countries-strip-head">
        <span class="tag">{{ eor.countries.subtitle }}</span>
        <h2 class="section-title">{{ eor.countries.title }}</h2>
        <p class="section-lead">{{ eor.countries.description }}</p>
      </div>
      <div class="countries-grid">
        <div v-for="c in eor.countries.items" :key="c.name" class="country-chip">
          <img :src="c.flag" :alt="c.name + ' flag'" class="country-flag" loading="lazy" />
          <span>{{ c.name }}</span>
        </div>
      </div>
      <p class="countries-note">{{ eor.countries.note }}</p>
    </div>
  </section>

  <!-- ============= WHO NEEDS EOR ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Who it's for</span>
      <h2 class="section-title">{{ eor.whoNeeds.title }}</h2>
      <p class="section-lead">{{ eor.whoNeeds.description }}</p>
    </div>
    <div class="whoneeds-grid">
      <div v-for="(a, i) in eor.whoNeeds.audiences" :key="i" class="whoneeds-card">
        <span class="whoneeds-icon" aria-hidden>◆</span>
        <h4>{{ a.userType }}</h4>
        <div class="whoneeds-pain">
          <span class="tag-small">Pain</span>
          <p>{{ a.pain }}</p>
        </div>
        <div class="whoneeds-solution">
          <span class="tag-small">Solution</span>
          <p>{{ a.solution }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= COMPLIANCE TRUST ============= -->
  <section class="section container">
    <div class="compliance-block">
      <div>
        <span class="tag">Trust &amp; compliance</span>
        <h2 class="section-title">{{ eor.complianceTrust.title }}</h2>
        <p v-for="(p, i) in eor.complianceTrust.intro" :key="i" class="section-lead compliance-para">
          {{ p }}
        </p>
      </div>
      <div class="compliance-bullets">
        <div v-for="(b, i) in eor.complianceTrust.bullets" :key="i" class="compliance-bullet">
          <span class="compliance-mark" aria-hidden>✓</span>
          <p>{{ b }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ eor.faqs.title }}</h2>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in eor.faqs.items"
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
      <h2>{{ eor.cta.title }}</h2>
      <p>{{ eor.cta.description }}</p>
      <div class="cta-warm-features">
        <span v-for="f in eor.cta.features" :key="f" class="cta-feature">
          <span class="cta-feature-mark" aria-hidden>✓</span> {{ f }}
        </span>
      </div>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=eor_services" class="btn-primary">
          {{ eor.cta.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/cost-calculator" class="btn-secondary">Estimate cost</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

/* ============= COUNTRIES STRIP (EOR-specific) ============= */
.countries-strip {
  padding: 100px 0;
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
}
.countries-strip-head { max-width: 760px; margin-bottom: 48px; }
.countries-strip-head .tag { margin-bottom: 16px; }
.countries-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 14px;
}
.country-chip {
  display: flex;
  align-items: center;
  gap: 14px;
  background: var(--bg);
  border-radius: 12px;
  padding: 14px 18px;
  border: 1px solid var(--border);
  transition: border-color 0.2s, transform 0.2s;
}
.country-chip:hover {
  border-color: var(--accent);
  transform: translateY(-2px);
}
.country-flag {
  width: 32px;
  height: 22px;
  object-fit: cover;
  border-radius: 3px;
  flex-shrink: 0;
}
.country-chip span {
  font-size: 14px;
  font-weight: 500;
}
.countries-note {
  margin-top: 24px;
  font-size: 13px;
  color: var(--ink-muted);
  font-style: italic;
}

/* ============= WHO NEEDS ============= */
.whoneeds-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}
.whoneeds-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px;
}
.whoneeds-icon {
  display: inline-flex;
  width: 36px;
  height: 36px;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  background: var(--accent-soft);
  color: var(--accent);
  font-size: 16px;
  margin-bottom: 16px;
}
.whoneeds-card h4 {
  font-family: var(--serif);
  font-size: 19px;
  font-weight: 400;
  margin-bottom: 16px;
  color: var(--ink);
}
.whoneeds-pain,
.whoneeds-solution {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 12px 0;
}
.whoneeds-pain { border-top: 1px solid var(--border); }
.whoneeds-pain p,
.whoneeds-solution p {
  font-size: 13px;
  color: var(--ink-soft);
  line-height: 1.55;
}

/* ============= COMPLIANCE (EOR-specific) ============= */
.compliance-block {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 80px;
  align-items: start;
}
.compliance-block .tag { margin-bottom: 16px; }
.compliance-para {
  margin-bottom: 16px;
}
.compliance-bullets {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.compliance-bullet {
  display: flex;
  gap: 16px;
  padding: 20px 24px;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
}
.compliance-mark {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--accent);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  flex-shrink: 0;
}
.compliance-bullet p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.55;
}

@media (max-width: 1024px) {
  .compliance-block {
    grid-template-columns: 1fr;
    gap: 32px;
  }
}
@media (max-width: 640px) {
  .countries-strip {
    padding: 60px 0;
  }
}
</style>
