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

// Pre-select reason from ?reason= query
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
  <div class="contact-page-container">
    
    <!-- ============= HERO & FORM SECTION ============= -->
    <section class="contact-hero-section">
      <!-- Background Map Image -->
      <div class="hero-bg-map" style="background-image: url('/case-study/contact-bg.png');"></div>

      <div class="container hero-grid">
        <!-- Left Content -->
        <div class="hero-left-content">
          <span class="tag-eyebrow">GET IN TOUCH</span>
          <h1>Let's build your global success <span class="highlight-gold">together.</span></h1>
          <p class="hero-desc">Have questions about global employment, expansion, or our services? Our experts are here to help.</p>

          <!-- Info Cards -->
          <div class="contact-info-cards">
            <!-- Email -->
            <a href="mailto:hello@jacksonandfrank.com" class="info-card">
              <div class="icon-circle">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline points="22,6 12,13 2,6"></polyline></svg>
              </div>
              <div class="info-card-text">
                <span class="label">Email us</span>
                <strong class="value">hello@jacksonandfrank.com</strong>
              </div>
            </a>

            <!-- Call -->
            <a href="tel:+442045722467" class="info-card">
              <div class="icon-circle">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
              </div>
              <div class="info-card-text">
                <span class="label">Call us</span>
                <strong class="value">+44 20 4572 2467</strong>
              </div>
            </a>

            <!-- Hours -->
            <div class="info-card no-hover">
              <div class="icon-circle">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
              </div>
              <div class="info-card-text">
                <span class="label">Business hours</span>
                <strong class="value">Mon – Fri, 9:00 AM – 6:00 PM (CET)</strong>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Floating Form -->
        <div class="hero-right-form">
          <form class="floating-contact-form" @submit="handleSubmit">
            <h2>Send us a message</h2>

            <!-- Success State -->
            <div v-if="status === 'success'" class="form-success-card">
              <div class="success-icon-circle">✓</div>
              <strong>Message Sent!</strong>
              <p>Thank you for contacting us. We'll get back to you within 24 hours.</p>
            </div>

            <!-- Form Fields -->
            <template v-else>
              <div class="form-grid-row">
                <div class="form-field">
                  <input v-model="form.firstName" type="text" placeholder="First name" required aria-label="First Name" />
                </div>
                <div class="form-field">
                  <input v-model="form.lastName" type="text" placeholder="Last name" required aria-label="Last Name" />
                </div>
              </div>

              <div class="form-field">
                <input v-model="form.email" type="email" placeholder="Work email" required aria-label="Work Email" />
              </div>

              <div class="form-field">
                <input v-model="form.company" type="text" placeholder="Company name" aria-label="Company Name" />
              </div>

              <div class="form-field select-field">
                <select v-model="form.reason" required aria-label="What can we help you with?">
                  <option value="" disabled selected>How can we help you?</option>
                  <option v-for="r in contact.form.contactReasons" :key="r.value" :value="r.value">
                    {{ r.label }}
                  </option>
                </select>
                <div class="select-arrow">
                  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"></polyline></svg>
                </div>
              </div>

              <div class="form-field">
                <textarea v-model="form.message" placeholder="Your message" rows="4" required aria-label="Your Message"></textarea>
              </div>

              <div class="form-consent-row">
                <label class="custom-checkbox">
                  <input v-model="form.consent" type="checkbox" required />
                  <span class="checkmark"></span>
                </label>
                <span class="consent-text">
                  I agree to the <RouterLink to="/privacy-policy">Privacy Policy</RouterLink> and Terms of Service.
                </span>
              </div>

              <p v-if="status === 'error'" class="error-msg-banner">{{ errorMessage }}</p>

              <button type="submit" class="btn-primary submit-btn" :disabled="status === 'sending'">
                {{ status === 'sending' ? 'Sending...' : 'Send Message' }}
                <span class="arrow">→</span>
              </button>

              <div class="form-footer-lock">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect><path d="M7 11V7a5 5 0 0 1 10 0v4"></path></svg>
                <span>Your information is secure and confidential.</span>
              </div>
            </template>
          </form>
        </div>
      </div>
    </section>

    <!-- ============= WHY GET IN TOUCH ============= -->
    <section class="why-us-section">
      <div class="container why-us-grid">
        <!-- Left info -->
        <div class="why-us-left">
          <span class="tag-eyebrow">WHY GET IN TOUCH WITH JACKSON &amp; FRANK</span>
          <h2>Your trusted partner for <span class="highlight-gold">global expansion</span></h2>
          <p>We combine local expertise with global experience to make your expansion simple, compliant, and successful.</p>
          <RouterLink to="/about-us" class="btn-secondary learn-more-btn">
            Learn more about us <span class="arrow">→</span>
          </RouterLink>
        </div>

        <!-- Right 3x2 cards -->
        <div class="why-us-cards-grid">
          <!-- Card 1 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"></circle><line x1="2" y1="12" x2="22" y2="12"></line><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path></svg>
            </div>
            <h5>Local Expertise, Global Reach</h5>
            <p>Our teams in 40+ countries understand local markets, laws, and business culture.</p>
          </div>

          <!-- Card 2 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path></svg>
            </div>
            <h5>Compliance You Can Trust</h5>
            <p>Stay compliant with local laws, tax regulations, and labor requirements.</p>
          </div>

          <!-- Card 3 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"></polygon></svg>
            </div>
            <h5>Fast &amp; Efficient</h5>
            <p>Most first hires go live within 48–72 hours.</p>
          </div>

          <!-- Card 4 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
            </div>
            <h5>End-to-End Support</h5>
            <p>From entity setup to payroll and HR, we handle it all.</p>
          </div>

          <!-- Card 5 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="19" y1="5" x2="5" y2="19"></line><circle cx="6.5" cy="6.5" r="2.5"></circle><circle cx="17.5" cy="17.5" r="2.5"></circle></svg>
            </div>
            <h5>Cost Effective</h5>
            <p>Save time and costs with our streamlined solutions.</p>
          </div>

          <!-- Card 6 -->
          <div class="why-us-card">
            <div class="card-icon-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 18v-6a9 9 0 0 1 18 0v6"></path><path d="M21 19a2 2 0 0 1-2 2h-1a2 2 0 0 1-2-2v-3a2 2 0 0 1 2-2h3zM3 19a2 2 0 0 0 2 2h1a2 2 0 0 0 2-2v-3a2 2 0 0 0-2-2H3z"></path></svg>
            </div>
            <h5>Human Support</h5>
            <p>Real people, real support, whenever you need us.</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ============= OUR OFFICES ============= -->
    <section class="offices-section">
      <div class="container">
        <div class="offices-header-row">
          <div>
            <span class="tag-eyebrow">OUR OFFICES</span>
            <h2>A global presence, <span class="highlight-gold">wherever you grow.</span></h2>
          </div>
          <RouterLink to="/sitemaps" class="btn-secondary view-locations-btn">
            View all locations <span class="arrow">→</span>
          </RouterLink>
        </div>

        <div class="offices-card-grid">
          <!-- London -->
          <div class="office-location-card">
            <img src="/countries/eor-uk.webp" alt="London skyline" class="office-img" />
            <div class="card-inner-body">
              <div class="country-row">
                <span class="flag">🇬🇧</span>
                <span class="country-name">London</span>
                <span class="badge-hq">Head Office</span>
              </div>
              <p class="address">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                25 Wilton Road, London, SW1V 1LW, United Kingdom
              </p>
              <a href="tel:+442045722467" class="phone-link">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                +44 20 4572 2467
              </a>
            </div>
          </div>

          <!-- Warsaw -->
          <div class="office-location-card">
            <img src="/countries/eor-poland.webp" alt="Warsaw skyline" class="office-img" />
            <div class="card-inner-body">
              <div class="country-row">
                <span class="flag">🇵🇱</span>
                <span class="country-name">Warsaw</span>
              </div>
              <p class="address">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                ul. Zlota 59, 00-120 Warsaw, Poland
              </p>
              <a href="tel:+48222082700" class="phone-link">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                +48 22 208 27 00
              </a>
            </div>
          </div>

          <!-- Amsterdam -->
          <div class="office-location-card">
            <img src="/countries/eor-netherlands.webp" alt="Amsterdam canals" class="office-img" />
            <div class="card-inner-body">
              <div class="country-row">
                <span class="flag">🇳🇱</span>
                <span class="country-name">Amsterdam</span>
              </div>
              <p class="address">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                Herengracht 124, 1015 BT Amsterdam, Netherlands
              </p>
              <a href="tel:+31208082967" class="phone-link">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                +31 20 808 2967
              </a>
            </div>
          </div>

          <!-- Singapore -->
          <div class="office-location-card">
            <img src="/countries/eor-hong-kong.webp" alt="Singapore Skyline" class="office-img" />
            <div class="card-inner-body">
              <div class="country-row">
                <span class="flag">🇸🇬</span>
                <span class="country-name">Singapore</span>
              </div>
              <p class="address">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                10 Anson Road, #10-11, Singapore 079903, Singapore
              </p>
              <a href="tel:+6569502185" class="phone-link">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                +65 6950 2185
              </a>
            </div>
          </div>

          <!-- New York -->
          <div class="office-location-card">
            <img src="/countries/eor-spain.webp" alt="New York skyline" class="office-img" />
            <div class="card-inner-body">
              <div class="country-row">
                <span class="flag">🇺🇸</span>
                <span class="country-name">New York</span>
              </div>
              <p class="address">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                375 Park Avenue, 9th Floor, New York, NY 10152, USA
              </p>
              <a href="tel:+16469939004" class="phone-link">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="loc-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                +1 646 993 9004
              </a>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============= MAP OVERLAY STRIP ============= -->
    <section class="map-overlay-section">
      <div class="container map-strip-container">
        <!-- World map with connection lines behind/under the card -->
        <div class="dotted-map-backdrop" style="background-image: url('/case-study/contact-bg.png');">
          <!-- Pin overlays positioned on the map -->
          <div class="map-pin pin-us" style="top: 35%; left: 22%"></div>
          <div class="map-pin pin-uk" style="top: 25%; left: 46%"></div>
          <div class="map-pin pin-nl" style="top: 24%; left: 49%"></div>
          <div class="map-pin pin-pl" style="top: 26%; left: 53%"></div>
          <div class="map-pin pin-in" style="top: 52%; left: 68%"></div>
          <div class="map-pin pin-sg" style="top: 66%; left: 74%"></div>
        </div>
        
        <!-- Right side Stats overlay card -->
        <div class="stats-overlay-card">
          <span class="operating-label">Operating in</span>
          <div class="countries-count">40+</div>
          <span class="countries-label">countries</span>
          <p class="desc">Supporting companies worldwide with local expertise.</p>
        </div>
      </div>
    </section>

  </div>
</template>

<style scoped>
.contact-page-container {
  background-color: #faf9f6;
  font-family: var(--sans);
  color: var(--ink);
  overflow: hidden;
}

/* ============= HERO & FORM ============= */
.contact-hero-section {
  position: relative;
  padding: 160px 0 100px;
  background-color: #fcfbf8;
  border-bottom: 1px solid var(--border);
  overflow: hidden;
}
.hero-bg-map {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  opacity: 0.15;
  pointer-events: none;
}
.hero-grid {
  position: relative;
  z-index: 2;
  display: grid;
  grid-template-columns: 1fr;
  gap: 48px;
  align-items: center;
}
@media (min-width: 1024px) {
  .hero-grid {
    grid-template-columns: 1.1fr 0.9fr;
    gap: 80px;
  }
}

.hero-left-content {
  max-width: 580px;
}
.tag-eyebrow {
  display: inline-block;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.16em;
  color: var(--accent);
  margin-bottom: 16px;
  text-transform: uppercase;
}
.hero-left-content h1 {
  font-family: var(--serif);
  font-size: clamp(34px, 4.5vw, 54px);
  line-height: 1.12;
  font-weight: 400;
  letter-spacing: -0.015em;
  margin-bottom: 24px;
}
.highlight-gold {
  color: var(--accent);
  font-style: italic;
}
.hero-desc {
  font-size: 16px;
  line-height: 1.6;
  color: var(--ink-soft);
  margin-bottom: 40px;
}

.contact-info-cards {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.info-card {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px 20px;
  background-color: #ffffff;
  border: 1px solid var(--border);
  border-radius: 12px;
  text-decoration: none;
  color: inherit;
  transition: all 0.25s ease;
  box-shadow: 0 4px 12px rgba(0,0,0,0.01);
}
.info-card:not(.no-hover):hover {
  transform: translateY(-2px);
  border-color: var(--accent);
  box-shadow: 0 10px 24px -10px rgba(176, 149, 89, 0.15);
}
.info-card .icon-circle {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background-color: var(--accent-soft);
  color: var(--accent);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.info-card .icon-circle svg {
  width: 18px;
  height: 18px;
}
.info-card-text {
  display: flex;
  flex-direction: column;
}
.info-card-text .label {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--ink-muted);
  margin-bottom: 2px;
}
.info-card-text .value {
  font-size: 14px;
  font-weight: 600;
  color: var(--ink);
}

/* Form block */
.hero-right-form {
  min-width: 0;
}
.floating-contact-form {
  background-color: #ffffff;
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 36px;
  box-shadow: 0 15px 45px -10px rgba(20, 51, 105, 0.04);
}
.floating-contact-form h2 {
  font-family: var(--serif);
  font-size: 26px;
  font-weight: 400;
  color: var(--ink);
  margin-bottom: 24px;
}

.form-grid-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}
.form-field {
  margin-bottom: 16px;
}
.form-field input,
.form-field select,
.form-field textarea {
  width: 100%;
  padding: 13px 16px;
  border: 1px solid var(--border);
  border-radius: 8px;
  background-color: #faf9f6;
  font-family: var(--sans);
  font-size: 14px;
  color: var(--ink);
  outline: none;
  transition: all 0.2s;
}
.form-field input:focus,
.form-field select:focus,
.form-field textarea:focus {
  background-color: #ffffff;
  border-color: var(--accent);
}
.form-field textarea {
  resize: vertical;
}

/* Select custom dropdown arrow */
.select-field {
  position: relative;
}
.select-field select {
  appearance: none;
  -webkit-appearance: none;
  padding-right: 40px;
}
.select-arrow {
  position: absolute;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  pointer-events: none;
  color: var(--ink-muted);
}
.select-arrow svg {
  width: 16px;
  height: 16px;
}

/* Custom Checkbox */
.form-consent-row {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin-bottom: 24px;
}
.custom-checkbox {
  position: relative;
  display: inline-block;
  width: 18px;
  height: 18px;
  flex-shrink: 0;
  margin-top: 2px;
}
.custom-checkbox input {
  opacity: 0;
  width: 0;
  height: 0;
}
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #faf9f6;
  border: 1px solid var(--border);
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}
.custom-checkbox input:checked + .checkmark {
  background-color: var(--accent);
  border-color: var(--accent);
}
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 6px;
  top: 2px;
  width: 4px;
  height: 8px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}
.custom-checkbox input:checked + .checkmark:after {
  display: block;
}
.consent-text {
  font-size: 13px;
  color: var(--ink-soft);
  line-height: 1.4;
}
.consent-text a {
  color: var(--accent);
  text-decoration: underline;
}

.submit-btn {
  width: 100%;
  justify-content: center;
  padding: 14px 24px;
  font-size: 15px;
}
.error-msg-banner {
  color: #b54234;
  font-size: 13px;
  margin-bottom: 16px;
  font-weight: 500;
}

.form-footer-lock {
  display: flex;
  align-items: center;
  gap: 8px;
  justify-content: center;
  margin-top: 20px;
  color: var(--ink-muted);
}
.form-footer-lock svg {
  width: 14px;
  height: 14px;
}
.form-footer-lock span {
  font-size: 11px;
}

.form-success-card {
  text-align: center;
  padding: 32px 16px;
}
.success-icon-circle {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background-color: var(--accent);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  margin: 0 auto 16px;
}
.form-success-card strong {
  display: block;
  font-family: var(--serif);
  font-size: 22px;
  color: var(--ink);
  margin-bottom: 8px;
}
.form-success-card p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.5;
}

/* ============= WHY JACKSON & FRANK ============= */
.why-us-section {
  padding: 100px 0;
  background-color: #faf9f6;
}
.why-us-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 48px;
}
@media (min-width: 1024px) {
  .why-us-grid {
    grid-template-columns: 1fr 1.3fr;
    gap: 80px;
  }
}
.why-us-left {
  max-width: 480px;
}
.why-us-left h2 {
  font-family: var(--serif);
  font-size: clamp(30px, 3.8vw, 46px);
  line-height: 1.15;
  font-weight: 400;
  margin-bottom: 20px;
  letter-spacing: -0.01em;
}
.why-us-left p {
  font-size: 15px;
  line-height: 1.65;
  color: var(--ink-soft);
  margin-bottom: 32px;
}
.learn-more-btn {
  display: inline-flex;
  align-items: center;
}

.why-us-cards-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}
@media (min-width: 640px) {
  .why-us-cards-grid {
    grid-template-columns: 1fr 1fr;
  }
}
.why-us-card {
  background-color: #ffffff;
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 28px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.01);
  transition: transform 0.25s;
}
.why-us-card:hover {
  transform: translateY(-2px);
}
.card-icon-wrap {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: var(--accent-soft);
  color: var(--accent);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 18px;
}
.card-icon-wrap svg {
  width: 20px;
  height: 20px;
}
.why-us-card h5 {
  font-size: 15px;
  font-weight: 700;
  color: var(--ink);
  margin-bottom: 8px;
}
.why-us-card p {
  font-size: 13px;
  line-height: 1.5;
  color: var(--ink-soft);
  margin: 0;
}

/* ============= OUR OFFICES ============= */
.offices-section {
  padding: 100px 0;
  background-color: #ffffff;
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
}
.offices-header-row {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 48px;
  justify-content: space-between;
  align-items: flex-start;
}
@media (min-width: 768px) {
  .offices-header-row {
    flex-direction: row;
    align-items: flex-end;
  }
}
.offices-header-row h2 {
  font-family: var(--serif);
  font-size: clamp(28px, 3.5vw, 42px);
  line-height: 1.2;
  font-weight: 400;
  margin-top: 6px;
  letter-spacing: -0.01em;
}

.offices-card-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}
@media (min-width: 640px) {
  .offices-card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 1024px) {
  .offices-card-grid {
    grid-template-columns: repeat(5, 1fr);
    gap: 16px;
  }
}
.office-location-card {
  background-color: #faf9f6;
  border: 1px solid var(--border);
  border-radius: 12px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 12px rgba(0,0,0,0.01);
}
.office-img {
  width: 100%;
  height: 110px;
  object-fit: cover;
  display: block;
}
.card-inner-body {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}
.country-row {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 12px;
}
.country-row .flag {
  font-size: 16px;
}
.country-row .country-name {
  font-size: 14px;
  font-weight: 700;
  color: var(--ink);
}
.badge-hq {
  font-size: 9px;
  font-weight: 700;
  letter-spacing: 0.04em;
  background-color: var(--accent-soft);
  color: var(--accent);
  padding: 2px 6px;
  border-radius: 4px;
  text-transform: uppercase;
  margin-left: auto;
}
.office-location-card .address {
  font-size: 12px;
  line-height: 1.55;
  color: var(--ink-soft);
  margin-bottom: 16px;
  display: flex;
  gap: 6px;
}
.office-location-card .phone-link {
  font-size: 12px;
  font-weight: 600;
  color: var(--accent);
  text-decoration: none;
  margin-top: auto;
  display: flex;
  align-items: center;
  gap: 6px;
}
.office-location-card .phone-link:hover {
  text-decoration: underline;
}
.loc-icon {
  width: 12px;
  height: 12px;
  color: var(--ink-muted);
  flex-shrink: 0;
  margin-top: 2px;
}

/* ============= MAP OVERLAY STRIP ============= */
.map-overlay-section {
  background-color: #fcfbf8;
  padding: 80px 0;
  position: relative;
  overflow: hidden;
}
.map-strip-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 40px;
  align-items: center;
  position: relative;
}
@media (min-width: 900px) {
  .map-strip-container {
    grid-template-columns: 1.3fr 0.7fr;
  }
}
.dotted-map-backdrop {
  height: 300px;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  position: relative;
  opacity: 0.75;
}
@media (min-width: 900px) {
  .dotted-map-backdrop {
    height: 340px;
  }
}

/* World pins overlay styling */
.map-pin {
  position: absolute;
  width: 12px;
  height: 12px;
  background-color: var(--accent);
  border: 2px solid white;
  border-radius: 50%;
  box-shadow: 0 0 10px rgba(176, 149, 89, 0.8);
  transform: translate(-50%, -50%);
  animation: pulse 2s infinite;
}
@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(176, 149, 89, 0.7); }
  70% { box-shadow: 0 0 0 8px rgba(176, 149, 89, 0); }
  100% { box-shadow: 0 0 0 0 rgba(176, 149, 89, 0); }
}

.stats-overlay-card {
  background-color: #ffffff;
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 36px 40px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.02);
  text-align: center;
  max-width: 320px;
  margin: 0 auto;
}
@media (min-width: 900px) {
  .stats-overlay-card {
    margin: 0 0 0 auto;
  }
}
.operating-label {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--ink-muted);
}
.countries-count {
  font-family: var(--serif);
  font-size: 72px;
  line-height: 1;
  font-weight: 400;
  color: var(--accent);
  margin: 8px 0;
}
.countries-label {
  font-size: 18px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  color: var(--ink);
  display: block;
  margin-bottom: 16px;
}
.stats-overlay-card .desc {
  font-size: 13px;
  line-height: 1.5;
  color: var(--ink-soft);
  margin: 0;
}
</style>
