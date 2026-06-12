<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'
import { useRoute, RouterLink } from 'vue-router'
import contact from '@/data/contact.json'
import { submitContactForm } from '@/services/contact'

const route = useRoute()

const form = reactive({
  firstName: '',
  lastName: '',
  email: '',
  phone: '',
  company: '',
  reason: '',
  message: '',
  consent: false,
})

const status = ref<'idle' | 'sending' | 'success' | 'error'>('idle')
const errorMessage = ref('')

// FAQ accordion
const openFaq = ref(0)
function toggleFaq(i: number) {
  openFaq.value = openFaq.value === i ? -1 : i
}

// Pre-select reason from ?reason= query (e.g. coming from a service page CTA)
onMounted(() => {
  const r = route.query.reason
  if (typeof r === 'string') {
    const match = contact.form.contactReasons.find((opt) => opt.value === r)
    if (match) form.reason = match.value
  }
})

async function handleSubmit(e: Event) {
  e.preventDefault()
  status.value = 'idle'
  errorMessage.value = ''

  if (!form.firstName || !form.lastName || !form.email || !form.message || !form.reason) {
    status.value = 'error'
    errorMessage.value = 'Please fill in all required fields.'
    return
  }
  if (!form.consent) {
    status.value = 'error'
    errorMessage.value = 'Please agree to the terms to continue.'
    return
  }

  status.value = 'sending'
  const result = await submitContactForm({
    first_name: form.firstName,
    last_name: form.lastName,
    work_email: form.email,
    phone_number: form.phone,
    company_name: form.company,
    help_reason: form.reason,
    message: form.message,
  })

  if (result.success) {
    status.value = 'success'
    Object.assign(form, {
      firstName: '', lastName: '', email: '', phone: '', company: '', reason: '', message: '', consent: false,
    })
  } else {
    status.value = 'error'
    errorMessage.value = result.error ?? 'Failed to send message. Please try again.'
  }
}
</script>

<template>
  <!-- ============= HERO ============= -->
  <header class="container contact-hero">
    <span class="tag">{{ contact.hero.badge }}</span>
    <h1 class="section-title">Get in <em>touch</em></h1>
    <p>{{ contact.hero.description }}</p>
  </header>

  <!-- ============= FORM + INFO ============= -->
  <section class="container contact-main">
    <!-- Form card -->
    <form class="contact-form" @submit="handleSubmit">
      <div class="contact-form-head">
        <h2>{{ contact.form.title }}</h2>
        <p>{{ contact.form.description }}</p>
      </div>

      <div v-if="status === 'success'" class="form-success">
        <span class="form-success-icon" aria-hidden>✓</span>
        <div>
          <strong>{{ contact.form.successMessage.title }}</strong>
          <p>{{ contact.form.successMessage.description }}</p>
        </div>
      </div>

      <template v-else>
        <div class="form-row">
          <label class="field">
            <span>First name *</span>
            <input v-model="form.firstName" type="text" placeholder="Jane" required />
          </label>
          <label class="field">
            <span>Last name *</span>
            <input v-model="form.lastName" type="text" placeholder="Doe" required />
          </label>
        </div>

        <div class="form-row">
          <label class="field">
            <span>Work email *</span>
            <input v-model="form.email" type="email" placeholder="jane@company.com" required />
          </label>
          <label class="field">
            <span>Phone</span>
            <input v-model="form.phone" type="tel" placeholder="+31 …" />
          </label>
        </div>

        <div class="form-row">
          <label class="field">
            <span>Company</span>
            <input v-model="form.company" type="text" placeholder="Company name" />
          </label>
          <label class="field">
            <span>What can we help with? *</span>
            <select v-model="form.reason" required>
              <option value="" disabled>Select a topic</option>
              <option v-for="r in contact.form.contactReasons" :key="r.value" :value="r.value">
                {{ r.label }}
              </option>
            </select>
          </label>
        </div>

        <label class="field">
          <span>Message *</span>
          <textarea
            v-model="form.message"
            rows="5"
            placeholder="Tell us about your global hiring needs…"
            required
          />
        </label>

        <label class="field-consent">
          <input v-model="form.consent" type="checkbox" />
          <span>
            By sending this form, I agree to the
            <RouterLink to="/privacy-policy">Privacy Policy</RouterLink>.
          </span>
        </label>

        <p v-if="status === 'error'" class="form-error">{{ errorMessage }}</p>

        <button type="submit" class="btn-primary" :disabled="status === 'sending'">
          {{ status === 'sending' ? 'Sending…' : 'Send request' }}
          <span class="arrow">→</span>
        </button>
      </template>
    </form>

    <!-- Contact info sidebar -->
    <aside class="contact-info">
      <span class="tag">{{ contact.contactInfo.title }}</span>
      <div v-for="item in contact.contactInfo.items" :key="item.title" class="contact-info-card">
        <h3>{{ item.title }}</h3>
        <p class="contact-info-desc">{{ item.description }}</p>
        <a v-if="item.href" :href="item.href" class="contact-info-value">{{ item.value }}</a>
        <span v-else-if="item.value" class="contact-info-value">{{ item.value }}</span>
      </div>

      <div class="contact-trust">
        <span v-for="t in contact.trustIndicators" :key="t.text" class="contact-trust-pill">
          {{ t.text }}
        </span>
      </div>
    </aside>
  </section>

  <!-- ============= WHY GET IN TOUCH ============= -->
  <section class="section container">
    <div class="intro-block">
      <div>
        <span class="tag">Why Jackson &amp; Frank</span>
        <h2 class="section-title">{{ contact.introSection.title }}</h2>
      </div>
      <div class="intro-text">
        <p v-for="(p, i) in contact.introSection.paragraphs" :key="i">{{ p }}</p>
      </div>
    </div>
  </section>

  <!-- ============= OFFICES ============= -->
  <section class="offices-strip">
    <div class="container">
      <div class="section-head">
        <span class="tag">Our offices</span>
        <h2 class="section-title">{{ contact.offices.title }}</h2>
        <p class="section-lead">{{ contact.offices.subtitle }}</p>
      </div>
      <div class="offices-grid">
        <!-- Main office -->
        <div class="office-card office-card-main">
          <span class="office-flag">HQ</span>
          <h3>{{ contact.offices.mainOffice.city }}, {{ contact.offices.mainOffice.country }}</h3>
          <p class="office-address">{{ contact.offices.mainOffice.address }}</p>
          <div class="office-meta">
            <a :href="`tel:${contact.offices.mainOffice.phone}`">{{ contact.offices.mainOffice.phone }}</a>
            <a :href="`mailto:${contact.offices.mainOffice.email}`">{{ contact.offices.mainOffice.email }}</a>
          </div>
        </div>
        <!-- Other offices -->
        <div v-for="office in contact.offices.otherOffices" :key="office.city" class="office-card">
          <h3>{{ office.city }}, {{ office.country }}</h3>
          <p class="office-address">{{ office.address }}</p>
          <div class="office-meta">
            <a :href="`tel:${office.phone}`">{{ office.phone }}</a>
            <a :href="`mailto:${office.email}`">{{ office.email }}</a>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============= FAQ ============= -->
  <section class="section container">
    <div class="faq-block">
      <div class="faq-head">
        <span class="tag">FAQs</span>
        <h2 class="section-title">{{ contact.faqs.title }}</h2>
        <p class="section-lead">{{ contact.faqs.subtitle }}</p>
      </div>
      <div class="faq-list">
        <button
          v-for="(item, i) in contact.faqs.items"
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
</template>

<style scoped>
@import '@/styles/service-page.css';

/* ============= HERO ============= */
.contact-hero {
  padding: 140px 32px 60px;
  text-align: center;
  max-width: 820px;
}
.contact-hero .tag { margin-bottom: 16px; }
.contact-hero .section-title { margin-bottom: 18px; }
.contact-hero p {
  font-size: 17px;
  color: var(--ink-soft);
  line-height: 1.6;
  max-width: 560px;
  margin: 0 auto;
}

/* ============= FORM + INFO ============= */
.contact-main {
  display: grid;
  grid-template-columns: 1.5fr 1fr;
  gap: 48px;
  padding-bottom: 80px;
  align-items: start;
}
.contact-form {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: 48px;
}
.contact-form-head {
  margin-bottom: 32px;
}
.contact-form-head h2 {
  font-family: var(--serif);
  font-size: 32px;
  font-weight: 400;
  margin-bottom: 8px;
}
.contact-form-head p {
  font-size: 14px;
  color: var(--ink-soft);
}
.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
.field {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 20px;
}
.field > span {
  font-size: 12px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--ink-muted);
  font-weight: 500;
}
.field input,
.field select,
.field textarea {
  font-family: var(--sans);
  font-size: 15px;
  padding: 13px 16px;
  border: 1px solid var(--border);
  border-radius: 10px;
  background: var(--bg);
  color: var(--ink);
  outline: none;
  transition: border-color 0.2s;
  width: 100%;
}
.field input:focus,
.field select:focus,
.field textarea:focus {
  border-color: var(--accent);
}
.field textarea {
  resize: vertical;
}
.field-consent {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  font-size: 13px;
  color: var(--ink-soft);
  margin-bottom: 24px;
}
.field-consent input {
  margin-top: 2px;
  accent-color: var(--accent);
}
.field-consent a {
  color: var(--accent);
  text-decoration: underline;
}
.form-error {
  color: #b54234;
  font-size: 14px;
  margin-bottom: 16px;
}
.form-success {
  display: flex;
  gap: 16px;
  align-items: center;
  padding: 32px;
  background: var(--accent-soft);
  border-radius: 12px;
}
.form-success-icon {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--accent);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  flex-shrink: 0;
}
.form-success strong {
  display: block;
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  margin-bottom: 4px;
}
.form-success p {
  font-size: 14px;
  color: var(--ink-soft);
}

/* Contact info sidebar */
.contact-info .tag { margin-bottom: 16px; }
.contact-info-card {
  padding: 24px 0;
  border-top: 1px solid var(--border);
}
.contact-info-card:last-of-type {
  border-bottom: 1px solid var(--border);
  margin-bottom: 24px;
}
.contact-info-card h3 {
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  margin-bottom: 6px;
}
.contact-info-desc {
  font-size: 13px;
  color: var(--ink-muted);
  line-height: 1.5;
  margin-bottom: 8px;
  white-space: pre-line;
}
.contact-info-value {
  font-size: 15px;
  font-weight: 500;
  color: var(--accent);
}
a.contact-info-value:hover {
  text-decoration: underline;
}
.contact-trust {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.contact-trust-pill {
  font-size: 12px;
  padding: 7px 14px;
  border-radius: 999px;
  background: var(--bg-card);
  border: 1px solid var(--border);
  color: var(--ink-soft);
}

/* ============= INTRO BLOCK ============= */
.intro-block {
  display: grid;
  grid-template-columns: 1fr 1.4fr;
  gap: 80px;
  align-items: start;
}
.intro-block .tag { margin-bottom: 16px; }
.intro-text p {
  font-size: 16px;
  color: var(--ink-soft);
  line-height: 1.7;
  margin-bottom: 18px;
}
.intro-text p:last-child { margin-bottom: 0; }

/* ============= OFFICES ============= */
.offices-strip {
  background: var(--bg-card);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 100px 0;
}
.offices-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.office-card {
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 32px;
}
.office-card-main {
  background: var(--ink);
  color: var(--bg);
  border-color: var(--ink);
}
.office-flag {
  display: inline-block;
  font-size: 11px;
  letter-spacing: 0.14em;
  background: var(--accent-warm);
  color: var(--ink);
  padding: 4px 10px;
  border-radius: 999px;
  margin-bottom: 16px;
  font-weight: 600;
}
.office-card h3 {
  font-family: var(--serif);
  font-size: 24px;
  font-weight: 400;
  margin-bottom: 12px;
}
.office-address {
  font-size: 13px;
  line-height: 1.6;
  color: var(--ink-soft);
  margin-bottom: 16px;
}
.office-card-main .office-address { color: rgba(255, 255, 255, 0.7); }
.office-meta {
  display: flex;
  flex-direction: column;
  gap: 6px;
  border-top: 1px solid var(--border);
  padding-top: 16px;
}
.office-card-main .office-meta { border-top-color: rgba(255, 255, 255, 0.15); }
.office-meta a {
  font-size: 13px;
  color: var(--accent);
}
.office-card-main .office-meta a { color: var(--accent-warm); }
.office-meta a:hover { text-decoration: underline; }

/* ============= RESPONSIVE ============= */
@media (max-width: 1024px) {
  .contact-main,
  .intro-block {
    grid-template-columns: 1fr;
    gap: 32px;
  }
  .offices-grid {
    grid-template-columns: 1fr;
  }
}
@media (max-width: 640px) {
  .contact-hero { padding-top: 110px; }
  .contact-form { padding: 32px 24px; }
  .form-row { grid-template-columns: 1fr; gap: 0; }
}
</style>
