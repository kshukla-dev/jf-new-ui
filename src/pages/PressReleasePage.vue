<script setup lang="ts">
import { RouterLink } from 'vue-router'
import ev from '@/data/china-europe-event.json'
</script>

<template>
  <!-- HERO -->
  <header class="pr-hero">
    <div class="container">
      <span class="tag">Event · {{ ev.hero.date }}</span>
      <h1>{{ ev.hero.eventTitle }}</h1>
      <p class="pr-tagline">{{ ev.hero.tagline }}</p>
      <p class="pr-subline">{{ ev.hero.eventSubline }}</p>
      <div class="pr-meta">
        <span>📅 {{ ev.hero.date }}</span>
        <span>🕒 {{ ev.hero.time }}</span>
        <span>📍 {{ ev.hero.venue }}</span>
      </div>
      <div class="cta-row">
        <a :href="ev.footerCta.registerHref" class="btn-primary">
          {{ ev.hero.ctaText }} <span class="arrow">→</span>
        </a>
        <a href="#speakers" class="btn-secondary btn-on-dark">Meet the speakers</a>
      </div>
    </div>
  </header>

  <!-- BACKGROUND -->
  <section class="section container">
    <div class="definition-block">
      <div>
        <span class="tag">Background</span>
        <h2 class="section-title">{{ ev.eventBackground.title }}</h2>
      </div>
      <div class="definition-text">
        <p v-for="(p, i) in ev.eventBackground.paragraphs" :key="i">{{ p }}</p>
      </div>
    </div>
  </section>

  <!-- FRAMEWORK -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">{{ ev.eventBackground.frameworkTitle }}</span>
      <h2 class="section-title">{{ ev.eventBackground.frameworkSteps.join(' → ') }}</h2>
    </div>
    <div class="pr-framework">
      <div v-for="(card, i) in ev.eventBackground.cards" :key="i" class="pr-fw-card">
        <span class="pr-fw-num">0{{ i + 1 }}</span>
        <h3>{{ card.title }}</h3>
        <p>{{ card.content }}</p>
      </div>
    </div>
  </section>

  <!-- AGENDA -->
  <section class="agenda-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Agenda</span>
        <h2 class="section-title">The <em>day</em></h2>
      </div>
      <div class="agenda-list">
        <div v-for="(item, i) in ev.agenda" :key="i" class="agenda-row">
          <span class="agenda-time">{{ item.time }}</span>
          <div class="agenda-detail">
            <strong>{{ item.title }}</strong>
            <span v-if="item.description">{{ item.description }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- SPEAKERS -->
  <section id="speakers" class="section container">
    <div class="section-head">
      <span class="tag">Speakers</span>
      <h2 class="section-title">Learn from <em>local experts</em></h2>
    </div>
    <div class="speakers-grid">
      <div v-for="sp in ev.speakers" :key="sp.name" class="speaker-card">
        <div class="speaker-photo">
          <img :src="sp.image" :alt="sp.name" loading="lazy" />
        </div>
        <div class="speaker-body">
          <h3>{{ sp.name }}</h3>
          <span class="speaker-role">{{ sp.role }} · {{ sp.company }}</span>
          <p>{{ sp.bio }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- REGISTER CTA -->
  <section id="register" class="cta-warm-wrap">
    <div class="cta-warm">
      <span class="cta-tag">{{ ev.footerCta.subheading }}</span>
      <h2>{{ ev.footerCta.heading }}</h2>
      <p>Secure your spot for {{ ev.hero.date }} at {{ ev.hero.venue }}.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact?reason=general_inquiry" class="btn-primary">
          {{ ev.footerCta.registerButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/events" class="btn-secondary">All events</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

.pr-hero {
  background: var(--dark);
  color: var(--bg);
  padding: 140px 0 80px;
}
.pr-hero .tag { color: rgba(255,255,255,0.55); margin-bottom: 18px; }
.pr-hero h1 {
  font-family: var(--serif);
  font-size: clamp(40px, 5.5vw, 76px);
  font-weight: 400;
  line-height: 1.04;
  letter-spacing: -0.02em;
  margin-bottom: 18px;
  max-width: 900px;
}
.pr-tagline {
  font-family: var(--serif);
  font-style: italic;
  font-size: clamp(20px, 2.4vw, 30px);
  color: var(--accent-warm);
  margin-bottom: 14px;
}
.pr-subline {
  font-size: 16px;
  color: rgba(255,255,255,0.75);
  max-width: 620px;
  line-height: 1.6;
  margin-bottom: 28px;
}
.pr-meta {
  display: flex;
  gap: 24px;
  flex-wrap: wrap;
  font-size: 14px;
  color: rgba(255,255,255,0.85);
  margin-bottom: 32px;
}
.pr-hero .cta-row { display: flex; gap: 12px; flex-wrap: wrap; }
.btn-on-dark { border-color: rgba(255,255,255,0.5); color: var(--bg); }
.btn-on-dark:hover { background: rgba(255,255,255,0.9); color: var(--ink); }

.pr-framework {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.pr-fw-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 36px 32px;
}
.pr-fw-num {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 36px;
  color: var(--accent);
  margin-bottom: 14px;
}
.pr-fw-card h3 {
  font-family: var(--serif);
  font-size: 26px;
  font-weight: 400;
  margin-bottom: 12px;
}
.pr-fw-card p { font-size: 14px; color: var(--ink-soft); line-height: 1.6; }

.agenda-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.agenda-list { display: flex; flex-direction: column; }
.agenda-row {
  display: grid;
  grid-template-columns: 160px 1fr;
  gap: 32px;
  padding: 20px 0;
  border-bottom: 1px solid var(--border);
}
.agenda-row:last-child { border-bottom: none; }
.agenda-time {
  font-size: 13px;
  letter-spacing: 0.06em;
  color: var(--accent);
  font-weight: 500;
}
.agenda-detail strong {
  display: block;
  font-family: var(--serif);
  font-size: 19px;
  font-weight: 400;
  margin-bottom: 2px;
}
.agenda-detail span { font-size: 13px; color: var(--ink-muted); }

.speakers-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}
.speaker-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  display: grid;
  grid-template-columns: 160px 1fr;
}
.speaker-photo { background: var(--accent-soft); }
.speaker-photo img { width: 100%; height: 100%; object-fit: cover; }
.speaker-body { padding: 28px; }
.speaker-body h3 {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 4px;
}
.speaker-role {
  display: block;
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 14px;
}
.speaker-body p { font-size: 13px; color: var(--ink-soft); line-height: 1.6; }

@media (max-width: 1024px) {
  .pr-framework, .speakers-grid { grid-template-columns: 1fr; }
}
@media (max-width: 640px) {
  .agenda-row { grid-template-columns: 1fr; gap: 4px; }
  .speaker-card { grid-template-columns: 1fr; }
  .speaker-photo { aspect-ratio: 16 / 10; }
}
</style>
