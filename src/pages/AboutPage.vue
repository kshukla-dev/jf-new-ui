<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import about from '@/data/about-us.json'

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

// W.O.R.L.D values (ported from jf_website_2.0)
const worldValues = [
  { letter: 'W', title: 'Winners', description: 'We measure our success by our clients’ outcomes. If our clients win, we win.' },
  { letter: 'O', title: 'Ownership', description: 'We take responsibility from start to finish. Accountability is non-negotiable.' },
  { letter: 'R', title: 'Responsive', description: 'We are always available for our clients. We act fast and communicate clearly.' },
  { letter: 'L', title: 'Leading', description: 'We lead in workforce solutions, combining local expertise with a global perspective to keep our clients ahead.' },
  { letter: 'D', title: 'Down to Earth', description: 'We provide clear, practical advice. We explain what matters, what to do next, and why it works.' },
]

// Story milestones (ported from jf_website_2.0)
const milestones = [
  { year: '2013', description: "Founded Macee BV (Jackson & Frank's holding company) in the Netherlands, focusing on providing HR solutions to the Dutch market." },
  { year: '2014', description: 'Achieved a significant milestone of employing 100+ people, establishing a strong foundation.' },
  { year: '2015', description: 'Secured the prestigious NeN Staffing License, demonstrating our commitment to compliance and high standards.' },
  { year: '2016-2017', description: 'Expanded operations into Belgium and the Czech Republic, broadening our European presence and capabilities.' },
  { year: '2018', description: 'Reached 500+ employees by supporting 250+ companies globally, marking a new phase of growth and operational scale.' },
  { year: '2022', description: "Launched Jackson and Frank as Macee's sister company, opening entities in the Netherlands, Germany, and Italy." },
  { year: '2023', description: 'Extended our reach to Poland, France and UK, achieving a workforce of 1,400+ employees across multiple countries.' },
  { year: '2024', description: 'Worked with our 700th client while expanding into Belgium, Czech Republic, and Spain.' },
  { year: '2025', description: 'Expanded into China by launching our HR Boutique and opening offices in Shanghai and Hong Kong.' },
]

const team = [
  { name: 'Maarten Koekebakker', role: 'Partner', image: '/leadership/Maarten.webp', bio: 'With a wealth of experience in navigating the intricate landscapes of international expansion strategy and human resources, Maarten stands as a seasoned professional adept at driving organizational growth and success on a global scale.' },
  { name: 'Pawel Michalkiewicz', role: 'Managing partner', image: '/leadership/pawel2.webp', bio: 'Pawel brings over two decades of hands-on experience in international business development and human resource management, having held pivotal roles in multinational corporations across various industries.' },
]
</script>

<template>
  <!-- ============= HERO ============= -->
  <header class="container service-hero">
    <div class="service-hero-copy">
      <span class="tag">About us</span>
      <h1>Global hiring,<br />made <em>human</em></h1>
      <p class="service-hero-lede">{{ about.definition.description }}</p>
      <div class="service-hero-features">
        <div v-for="(f, i) in about.definition.keyFeatures" :key="i" class="hero-feature">
          <span class="hero-feature-dot" />
          {{ f }}
        </div>
      </div>
      <div class="cta-row">
        <RouterLink to="/contact" class="btn-primary">
          {{ about.definition.primaryButtonText }} <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/contact?reason=consultation" class="btn-secondary">
          {{ about.definition.secondaryButtonText }}
        </RouterLink>
      </div>
    </div>
    <div class="service-hero-visual">
      <img :src="about.definition.image" :alt="about.definition.imageAlt" />
      <div class="service-hero-shape">J</div>
      <div class="service-hero-badge">
        <strong>Since 2013</strong>
        <span>Over a decade of global HR</span>
      </div>
    </div>
  </header>

  <!-- ============= STATS ============= -->
  <section class="stats-strip">
    <div class="container stats-strip-inner">
      <div v-for="(s, i) in about.definition.trustSignals.stats" :key="i" class="stat-item">
        <strong>{{ s.value }}</strong>
        <span v-if="s.label" class="stat-label">{{ s.label }}</span>
        <span class="stat-desc">{{ s.description }}</span>
      </div>
    </div>
  </section>

  <!-- ============= STORY ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Our story</span>
      <h2 class="section-title">From Arnhem to <em>160+ countries</em></h2>
      <p class="section-lead">
        The milestones that shaped Jackson &amp; Frank into a global workforce solutions
        provider, from our founding in the Netherlands to our expansion worldwide.
      </p>
    </div>
    <div class="timeline">
      <div v-for="m in milestones" :key="m.year" class="timeline-item">
        <span class="timeline-year">{{ m.year }}</span>
        <p>{{ m.description }}</p>
      </div>
    </div>
  </section>

  <!-- ============= VALUES ============= -->
  <section class="values-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Our values</span>
        <h2 class="section-title">We operate by <em>W.O.R.L.D</em></h2>
        <p class="section-lead">
          The values that make us and our clients successful.
        </p>
      </div>
      <div class="values-grid">
        <div v-for="v in worldValues" :key="v.title" class="value-card">
          <span class="value-letter">{{ v.letter }}</span>
          <h3>{{ v.title }}</h3>
          <p>{{ v.description }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= VISION / MISSION ============= -->
  <section class="section container">
    <div class="vm-grid">
      <div class="vm-card">
        <span class="tag">Vision</span>
        <p>A world in which international employability has no barriers.</p>
      </div>
      <div class="vm-card vm-card-dark">
        <span class="tag">Mission</span>
        <p>Enabling international labour mobility for organizations and people.</p>
      </div>
    </div>
  </section>

  <!-- ============= TEAM ============= -->
  <section class="section container">
    <div class="section-head">
      <span class="tag">Leadership</span>
      <h2 class="section-title">Meet our <em>leadership team</em></h2>
      <p class="section-lead">
        The visionary leaders behind our mission to make global hiring simple and accessible.
      </p>
    </div>
    <div class="team-grid">
      <div v-for="member in team" :key="member.name" class="team-card">
        <div class="team-photo">
          <img :src="member.image" :alt="member.name" />
        </div>
        <div class="team-body">
          <h3>{{ member.name }}</h3>
          <span class="team-role">{{ member.role }}</span>
          <p>{{ member.bio }}</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ about.faqs.title }}</h2>
        <p class="section-lead">{{ about.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in about.faqs.items"
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

  <!-- ============= CTA ============= -->
  <section class="cta-warm-wrap">
    <div class="cta-warm">
      <span class="cta-tag">Get started</span>
      <h2>Let's build your global team <em>together</em></h2>
      <p>Join 700+ companies who trust Jackson &amp; Frank to make global hiring simple.</p>
      <div class="cta-warm-buttons">
        <RouterLink to="/contact" class="btn-primary">
          Start the conversation <span class="arrow">→</span>
        </RouterLink>
        <RouterLink to="/advantages" class="btn-secondary">Why choose us</RouterLink>
      </div>
    </div>
  </section>
</template>

<style scoped>
@import '@/styles/service-page.css';

/* Timeline */
.timeline {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.timeline-item {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 28px;
  position: relative;
}
.timeline-year {
  display: inline-block;
  font-family: var(--serif);
  font-style: italic;
  font-size: 30px;
  color: var(--accent);
  margin-bottom: 12px;
}
.timeline-item p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}

/* Values */
.values-strip {
  background: var(--ink);
  color: var(--bg);
  padding: 100px 0;
}
.values-strip .tag { color: var(--accent-warm); }
.values-strip .section-title { color: var(--bg); }
.values-strip .section-title em { color: var(--accent-warm); }
.values-strip .section-lead { color: rgba(255, 255, 255, 0.7); }
.values-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 16px;
}
.value-card {
  background: var(--dark-soft);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: var(--radius);
  padding: 28px 24px;
}
.value-letter {
  display: flex;
  width: 44px;
  height: 44px;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  background: var(--accent);
  color: white;
  font-family: var(--serif);
  font-style: italic;
  font-size: 22px;
  margin-bottom: 18px;
}
.value-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 10px;
  color: var(--bg);
}
.value-card p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.65);
  line-height: 1.55;
}

/* Vision / Mission */
.vm-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
}
.vm-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 56px 48px;
}
.vm-card .tag { margin-bottom: 20px; }
.vm-card p {
  font-family: var(--serif);
  font-size: clamp(26px, 3vw, 38px);
  font-weight: 400;
  line-height: 1.2;
  color: var(--ink);
}
.vm-card-dark {
  background: var(--accent);
  color: white;
  border-color: var(--accent);
}
.vm-card-dark .tag { color: rgba(255, 255, 255, 0.7); }
.vm-card-dark p { color: white; }

/* Team */
.team-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
}
.team-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  display: grid;
  grid-template-columns: 200px 1fr;
}
.team-photo {
  background: var(--accent-soft);
}
.team-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.team-body {
  padding: 32px;
}
.team-body h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 4px;
}
.team-role {
  display: block;
  font-size: 12px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--accent);
  margin-bottom: 16px;
}
.team-body p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.6;
}

@media (max-width: 1024px) {
  .timeline { grid-template-columns: repeat(2, 1fr); }
  .values-grid { grid-template-columns: repeat(2, 1fr); }
  .vm-grid, .team-grid { grid-template-columns: 1fr; }
}
@media (max-width: 640px) {
  .timeline { grid-template-columns: 1fr; }
  .values-grid { grid-template-columns: 1fr; }
  .team-card { grid-template-columns: 1fr; }
  .team-photo { aspect-ratio: 16 / 10; }
}
</style>
