<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue'
import { useRoute, RouterLink } from 'vue-router'
import { COUNTRY_CONFIG, loadCountryData } from '@/data/country-config'
import type { CountryData, ServiceType } from '@/data/country-config'

const route = useRoute()

const config = computed(() => COUNTRY_CONFIG[route.path])
const data = ref<CountryData | null>(null)

const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

function load() {
  openFaq.value = 0
  if (config.value) {
    data.value = loadCountryData(config.value.dataKey)
    if (data.value) {
      document.title = `${data.value.metadata.title} - Jackson & Frank`
    }
  } else {
    data.value = null
  }
}

onMounted(load)
watch(() => route.path, () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
  load()
})

// EOR vs contractor copy variants
const isEor = computed(() => config.value?.type === 'eor')

function howSteps(type: ServiceType, country: string) {
  if (type === 'eor') {
    return [
      { title: 'Agree scope', description: `We align on roles, salary, and benefits for your hires in ${country}.` },
      { title: 'Onboard compliantly', description: `Local ${country} employment contracts, payroll, and registrations ready for day one.` },
      { title: 'Run payroll & HR', description: `Monthly payroll, filings, and HR lifecycle support while you manage the work.` },
    ]
  }
  return [
    { title: 'Draft the agreement', description: `Compliant contractor agreements aligned to ${country} classification rules.` },
    { title: 'Process payments', description: `Accurate, on-time payments to contractors in ${country} in local currency.` },
    { title: 'Stay compliant', description: `We monitor classification and tax requirements to reduce misclassification risk.` },
  ]
}

const benefits = computed(() => {
  const c = config.value?.name ?? 'this country'
  return isEor.value
    ? [
        `Hire in ${c} without setting up a local entity`,
        'Onboard in 2–3 business days',
        'Locally compliant employment contracts',
        'Payroll, taxes, and benefits handled for you',
      ]
    : [
        `Engage contractors in ${c} compliantly`,
        'Automated, multi-currency payments',
        'Classification & misclassification risk managed',
        'Secure contracts and documentation',
      ]
})

const stats = [
  { value: '160+', label: 'countries', desc: 'in our network' },
  { value: '2–3 days', label: '', desc: 'typical onboarding' },
  { value: '12+', label: 'local offices', desc: 'in countries we operate' },
  { value: '1,000+', label: 'companies', desc: 'trust us' },
]
</script>

<template>
  <template v-if="config && data">
    <!-- ============= HERO ============= -->
    <header class="container service-hero">
      <div class="service-hero-copy">
        <span class="tag">
          <img v-if="config.flag" :src="config.flag" :alt="config.name + ' flag'" class="hero-flag" />
          {{ config.name }} · {{ isEor ? 'Employer of Record' : 'Contractor management' }}
        </span>
        <h1 v-if="isEor">
          Hire in <em>{{ config.name }}</em><br />without an entity
        </h1>
        <h1 v-else>
          Pay contractors in<br /><em>{{ config.name }}</em>
        </h1>
        <p class="service-hero-lede">{{ data.metadata.description }}</p>
        <div class="service-hero-features">
          <div v-for="(f, i) in benefits" :key="i" class="hero-feature">
            <span class="hero-feature-dot" />
            {{ f }}
          </div>
        </div>
        <div class="cta-row">
          <RouterLink
            :to="`/contact?reason=${isEor ? 'eor_services' : 'contractor_management'}`"
            class="btn-primary"
          >
            {{ isEor ? `Hire in ${config.name}` : 'Manage contractors' }}
            <span class="arrow">→</span>
          </RouterLink>
          <RouterLink to="/contact?reason=consultation" class="btn-secondary">
            Book a call
          </RouterLink>
        </div>
      </div>
      <div class="service-hero-visual">
        <img :src="config.image" :alt="`${config.name} - Jackson & Frank`" />
        <div class="service-hero-badge">
          <strong>{{ isEor ? '2–3 day onboarding' : '24h payouts' }}</strong>
          <span>{{ isEor ? `Compliant hiring in ${config.name}` : `Contractor payments in ${config.name}` }}</span>
        </div>
      </div>
    </header>

    <!-- ============= STATS ============= -->
    <section class="stats-strip">
      <div class="container stats-strip-inner">
        <div v-for="(s, i) in stats" :key="i" class="stat-item">
          <strong>{{ s.value }}</strong>
          <span v-if="s.label" class="stat-label">{{ s.label }}</span>
          <span class="stat-desc">{{ s.desc }}</span>
        </div>
      </div>
    </section>

    <!-- ============= INTRO ============= -->
    <section class="section container">
      <div class="definition-block">
        <div>
          <span class="tag">{{ isEor ? 'EOR' : 'Contractor management' }} in {{ config.name }}</span>
          <h2 class="section-title">
            {{ isEor ? 'Employ' : 'Engage' }} talent in <em>{{ config.name }}</em>
          </h2>
        </div>
        <div class="definition-text">
          <p v-if="isEor">
            Jackson &amp; Frank acts as your legal employer in {{ config.name }}, handling
            employment contracts, payroll, taxes, and statutory benefits - so you can hire
            without opening a local entity. Your team focuses on the work; we handle local
            compliance.
          </p>
          <p v-else>
            We help you engage and pay independent contractors in {{ config.name }} compliantly.
            From drafting locally-aligned agreements to processing multi-currency payments, we
            reduce misclassification risk and keep your contractor relationships clean.
          </p>
          <p>
            With local expertise and on-the-ground knowledge of {{ config.name }} employment
            and tax rules, we make global hiring straightforward and fully compliant.
          </p>
        </div>
      </div>
    </section>

    <!-- ============= HOW IT WORKS ============= -->
    <section class="section container">
      <div class="section-head">
        <span class="tag">How it works</span>
        <h2 class="section-title">
          {{ isEor ? 'Hiring' : 'Engaging contractors' }} in <em>{{ config.name }}</em>, in 3 steps
        </h2>
      </div>
      <div class="how-grid">
        <div
          v-for="(step, i) in howSteps(config.type, config.name)"
          :key="i"
          class="how-card"
        >
          <span class="how-card-num">0{{ i + 1 }}</span>
          <h3>{{ step.title }}</h3>
          <p>{{ step.description }}</p>
        </div>
      </div>
    </section>

    <!-- ============= FAQ ============= -->
    <section class="section container">
      <div class="faq-block">
        <div class="faq-head">
          <span class="tag">FAQs</span>
          <h2 class="section-title">{{ data.faqs.title }}</h2>
          <p class="section-lead">{{ data.faqs.subtitle }}</p>
        </div>
        <div class="faq-list">
          <button
            v-for="(item, i) in data.faqs.items"
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
        <h2>
          {{ isEor ? 'Start hiring in' : 'Pay contractors in' }} <em>{{ config.name }}</em>
        </h2>
        <p>
          Most {{ config.name }} {{ isEor ? 'first hires go live in 2–3 days' : 'contractor setups complete within a day' }}.
          Talk to our team for a custom quote.
        </p>
        <div class="cta-warm-buttons">
          <RouterLink
            :to="`/contact?reason=${isEor ? 'eor_services' : 'contractor_management'}`"
            class="btn-primary"
          >
            Get a quote <span class="arrow">→</span>
          </RouterLink>
          <RouterLink :to="isEor ? '/employer-of-record' : '/contractor'" class="btn-secondary">
            {{ isEor ? 'About EOR' : 'About contractor management' }}
          </RouterLink>
        </div>
      </div>
    </section>
  </template>

  <!-- Fallback if route/data missing -->
  <div v-else class="container country-missing">
    <span class="tag">Country not found</span>
    <h1 class="section-title">We couldn't load this <em>country page</em></h1>
    <RouterLink to="/employer-of-record" class="btn-primary">
      Browse all services <span class="arrow">→</span>
    </RouterLink>
  </div>
</template>

<style scoped>
@import '@/styles/service-page.css';

.hero-flag {
  display: inline-block;
  width: 22px;
  height: 15px;
  object-fit: cover;
  border-radius: 2px;
  vertical-align: middle;
  margin-right: 8px;
}
.tag {
  display: inline-flex;
  align-items: center;
}

.how-grid {
  grid-template-columns: repeat(3, 1fr);
}

.country-missing {
  padding: 200px 0 120px;
  text-align: center;
}
.country-missing .tag { margin-bottom: 12px; }
.country-missing h1 { margin-bottom: 28px; }

@media (max-width: 1024px) {
  .how-grid { grid-template-columns: 1fr; }
}
</style>
