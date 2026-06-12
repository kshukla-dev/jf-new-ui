<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import caseData from '@/data/case-studies.json'

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}
</script>

<template>
  <header class="container cs-hero">
    <span class="tag">Success stories</span>
    <h1 class="section-title">{{ caseData.hero.title }}</h1>
    <p>{{ caseData.hero.description }}</p>
  </header>

  <section class="section container">
    <div class="intro-block">
      <div><span class="tag">Why it matters</span></div>
      <div class="intro-text">
        <p v-for="(p, i) in caseData.intro.paragraphs" :key="i">{{ p }}</p>
      </div>
    </div>
  </section>

  <section class="section container">
    <div class="cs-grid">
      <RouterLink
        v-for="cs in caseData.caseStudies"
        :key="cs.id"
        :to="`/case-studies/${cs.slug}`"
        class="cs-card"
      >
        <div class="cs-card-img">
          <img :src="cs.image" :alt="cs.title" loading="lazy" />
        </div>
        <div class="cs-card-body">
          <div class="cs-card-meta">
            <span>{{ cs.industry }}</span>
          </div>
          <h3>{{ cs.title }}</h3>
          <p>{{ cs.excerpt }}</p>
          <span class="cs-card-cta">Read case study <span aria-hidden>→</span></span>
        </div>
      </RouterLink>
    </div>
  </section>

  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ caseData.faqs.title }}</h2>
        <p class="section-lead">{{ caseData.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in caseData.faqs.items"
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
      <h2>Write your own <em>success story</em></h2>
      <p>Tell us your target countries and timeline — we'll outline a tailored approach.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact" class="btn-primary">
          Get a custom proposal <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/testimonials" class="btn-secondary">Read testimonials</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.cs-hero {
  padding: 140px 32px 40px;
  text-align: center;
  max-width: 760px;
}
.cs-hero .tag { margin-bottom: 16px; }
.cs-hero .section-title { margin-bottom: 16px; }
.cs-hero p {
  font-size: 17px;
  color: var(--ink-soft);
  line-height: 1.6;
  max-width: 560px;
  margin: 0 auto;
}

.intro-block {
  display: grid;
  grid-template-columns: 1fr 1.4fr;
  gap: 80px;
}
.intro-text p {
  font-size: 16px;
  color: var(--ink-soft);
  line-height: 1.7;
  margin-bottom: 18px;
}

.cs-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
}
.cs-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  text-decoration: none;
  color: var(--ink);
  transition: transform 0.3s, box-shadow 0.3s;
}
.cs-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 60px -30px rgba(0, 0, 0, 0.25);
}
.cs-card-img {
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background: var(--accent-soft);
}
.cs-card-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s;
}
.cs-card:hover .cs-card-img img { transform: scale(1.04); }
.cs-card-body { padding: 32px; }
.cs-card-meta {
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 14px;
}
.cs-card-body h3 {
  font-family: var(--serif);
  font-size: 28px;
  font-weight: 400;
  line-height: 1.2;
  margin-bottom: 12px;
}
.cs-card-body p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
  margin-bottom: 18px;
}
.cs-card-cta {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
  font-weight: 500;
  color: var(--accent);
}

@media (max-width: 1024px) {
  .intro-block, .cs-grid { grid-template-columns: 1fr; gap: 32px; }
}
@media (max-width: 640px) {
  .cs-hero { padding-top: 110px; }
}
</style>
