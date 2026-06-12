<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import tp from '@/data/testimonials-page.json'
import testimonials from '@/data/testimonials.json'

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

// All testimonials (grid below the featured spotlights)
const allReviews = testimonials.testimonials
</script>

<template>
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">{{ tp.definition.subtitle }}</span>
      <h1>Loved by teams <em>hiring globally</em></h1>
      <p class="service-hero-lede">{{ tp.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in tp.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact" class="btn-primary">
          {{ tp.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ tp.definition.secondaryButtonText }}
        </RouterLink>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="tp.definition.image" :alt="tp.definition.imageAlt" />
      <div class="service-hero-shape">★</div>
      <div class="service-hero-badge">
        <strong>4.9 / 5 rating</strong>
        <span>Across 700+ companies</span>
      </div>
    </div>
  </header>

  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in tp.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- Featured spotlights -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">{{ tp.featured.subtitle }}</span>
      <h2 class="section-title">{{ tp.featured.title }}</h2>
      <p class="section-lead">{{ tp.featured.description }}</p>
    </div>
    <div class="spotlight-grid">
      <div v-for="item in tp.featured.items" :key="item.id" class="spotlight-card">
        <span class="spotlight-metric">{{ item.metric }}</span>
        <blockquote>"{{ item.review }}"</blockquote>
        <div class="spotlight-author">
          <img :src="item.image" :alt="item.name" />
          <div>
            <strong>{{ item.name }}</strong>
            <span>{{ item.role }} · {{ item.country }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Highlights -->
  <section class="highlights-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">{{ tp.highlights.subtitle }}</span>
        <h2 class="section-title">{{ tp.highlights.title }}</h2>
        <p class="section-lead">{{ tp.highlights.description }}</p>
      </div>
      <div class="highlights-grid">
        <div v-for="(h, i) in tp.highlights.items" :key="i" class="highlight-card">
          <span class="highlight-mark">{{ String(i + 1).padStart(2, '0') }}</span>
          <h3>{{ h.title }}</h3>
          <p>{{ h.description }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- All reviews grid -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">More reviews</span>
      <h2 class="section-title">In our clients' <em>own words</em></h2>
    </div>
    <div class="reviews-grid">
      <div v-for="r in allReviews" :key="r.id" class="review-card">
        <div class="review-stars">★★★★★</div>
        <p>"{{ r.review }}"</p>
        <div class="review-author">
          <img :src="r.image" :alt="r.name" />
          <div>
            <strong>{{ r.name }}</strong>
            <span>{{ r.role }}</span>
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
        <h2 class="section-title">{{ tp.faqs.title }}</h2>
        <p class="section-lead">{{ tp.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in tp.faqs.items"
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
      <h2>Become our next <em>success story</em></h2>
      <p>Join 700+ companies expanding globally with Jackson &amp; Frank.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact" class="btn-primary">
          Start the conversation <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/case-studies" class="btn-secondary">See case studies</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.spotlight-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.spotlight-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.spotlight-metric {
  display: inline-block;
  align-self: flex-start;
  font-size: 12px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--accent);
  background: var(--accent-soft);
  padding: 6px 14px;
  border-radius: 999px;
  font-weight: 500;
}
.spotlight-card blockquote {
  font-family: var(--serif);
  font-size: 18px;
  line-height: 1.5;
  color: var(--ink);
  flex-grow: 1;
}
.spotlight-author,
.review-author {
  display: flex;
  align-items: center;
  gap: 12px;
  border-top: 1px solid var(--border);
  padding-top: 16px;
}
.spotlight-author img,
.review-author img {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  object-fit: cover;
}
.spotlight-author strong,
.review-author strong {
  display: block;
  font-size: 14px;
  font-weight: 600;
}
.spotlight-author span,
.review-author span {
  font-size: 12px;
  color: var(--ink-muted);
}

.highlights-strip {
  background: var(--ink);
  color: var(--bg);
  padding: 100px 0;
}
.highlights-strip .tag { color: var(--accent-warm); }
.highlights-strip .section-title { color: var(--bg); }
.highlights-strip .section-lead { color: rgba(255, 255, 255, 0.7); }
.highlights-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.highlight-card {
  background: var(--dark-soft);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radius);
  padding: 28px;
}
.highlight-mark {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 30px;
  color: var(--accent-warm);
  margin-bottom: 14px;
}
.highlight-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
  color: var(--bg);
}
.highlight-card p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.65);
  line-height: 1.55;
}

.reviews-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.review-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.review-stars {
  color: var(--accent-warm);
  letter-spacing: 2px;
  font-size: 14px;
}
.review-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
  flex-grow: 1;
}

@media (max-width: 1024px) {
  .spotlight-grid, .reviews-grid { grid-template-columns: 1fr 1fr; }
  .highlights-grid { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 640px) {
  .spotlight-grid, .reviews-grid, .highlights-grid { grid-template-columns: 1fr; }
}
</style>
