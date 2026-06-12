<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { RouterLink } from 'vue-router'
import { fetchAllBlogs, fetchCategories } from '@/services/blog'
import { formatBlogDate } from '@/utils/blog'
import type { BlogPost, Category } from '@/types/blog'
import blogData from '@/data/blog.json'

const blogs = ref<BlogPost[]>([])
const categories = ref<Category[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

// Newsletter signup state
const newsletterEmail = ref('')
const newsletterStatus = ref<'idle' | 'sending' | 'ok' | 'error'>('idle')

const sortedBlogs = computed(() =>
  [...blogs.value].sort((a, b) => {
    const da = a.publish_date ? +new Date(a.publish_date) : 0
    const db = b.publish_date ? +new Date(b.publish_date) : 0
    return (Number.isNaN(db) ? 0 : db) - (Number.isNaN(da) ? 0 : da)
  })
)

const featured = computed(() => sortedBlogs.value[0])
const recent = computed(() => sortedBlogs.value.slice(1, 7)) // up to 6 below the featured
const remaining = computed(() => sortedBlogs.value.slice(7))

// Resolve category id → category_name (CMS returns id strings on each blog)
const categoryById = computed(() => {
  const map = new Map<string, string>()
  for (const c of categories.value) map.set(String(c.id), c.category_name)
  return map
})
function blogCategory(b: BlogPost): string {
  const first = (b.category_ids ?? '').split(',')[0]?.trim()
  if (!first) return 'Insights'
  return categoryById.value.get(first) ?? 'Insights'
}

onMounted(async () => {
  try {
    const [bs, cats] = await Promise.all([fetchAllBlogs(), fetchCategories()])
    blogs.value = bs
    categories.value = cats
  } catch (e) {
    error.value = e instanceof Error ? e.message : 'Failed to load blogs'
  } finally {
    loading.value = false
  }
})

async function submitNewsletter(e: Event) {
  e.preventDefault()
  if (!newsletterEmail.value.trim()) return
  newsletterStatus.value = 'sending'
  try {
    const res = await fetch('/api/v1/newsletter/subscribe', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: newsletterEmail.value.trim() }),
    })
    newsletterStatus.value = res.ok ? 'ok' : 'error'
    if (res.ok) newsletterEmail.value = ''
  } catch {
    newsletterStatus.value = 'error'
  }
}
</script>

<template>
  <!-- ============= DARK HERO + FEATURED ============= -->
  <header class="blog-hero">
    <div class="container blog-hero-inner">
      <div class="blog-hero-head">
        <span class="blog-hero-eyebrow">Blog</span>
        <h1 class="blog-hero-title">Blog &amp; <em>Insights</em></h1>
      </div>

      <!-- Featured article card -->
      <div v-if="loading" class="blog-featured blog-featured-skeleton">
        <div class="skeleton-shimmer" />
      </div>

      <RouterLink
        v-else-if="featured"
        :to="`/blog/${featured.slug}`"
        class="blog-featured"
      >
        <img
          v-if="featured.image_url"
          :src="featured.image_url"
          :alt="featured.title"
          class="blog-featured-img"
        />
        <div class="blog-featured-overlay" />
        <div class="blog-featured-meta">
          <span class="blog-featured-eyebrow">{{ blogCategory(featured).toUpperCase() }}</span>
          <span class="blog-featured-date">
            {{ formatBlogDate(featured.publish_date) }}
          </span>
        </div>
        <div class="blog-featured-body">
          <h2>{{ featured.title }}</h2>
          <span class="blog-featured-cta">
            Read now <span aria-hidden>→</span>
          </span>
        </div>
      </RouterLink>
    </div>
  </header>

  <!-- ============= INTRO COPY ============= -->
  <section class="blog-intro container">
    <div></div>
    <div>
      <p class="blog-intro-lead">
        {{ blogData.intro.title }}
      </p>
      <p v-for="(p, i) in blogData.intro.paragraphs" :key="i" class="blog-intro-body">
        {{ p }}
      </p>
    </div>
  </section>

  <!-- ============= RECENT ARTICLES GRID ============= -->
  <section class="section container">
    <h2 class="section-title blog-section-heading">Recent <em>articles</em></h2>

    <div v-if="loading" class="blog-grid">
      <div v-for="i in 6" :key="i" class="blog-card blog-card-skeleton">
        <div class="skeleton-shimmer" />
      </div>
    </div>

    <p v-else-if="error" class="blog-error">
      Couldn't load articles right now. Please refresh, or
      <RouterLink to="/contact">contact us</RouterLink>.
    </p>

    <div v-else class="blog-grid">
      <RouterLink
        v-for="post in recent"
        :key="post.id"
        :to="`/blog/${post.slug}`"
        class="blog-card"
      >
        <div class="blog-card-img-wrap">
          <img
            v-if="post.image_url"
            :src="post.image_url"
            :alt="post.title"
            class="blog-card-img"
            loading="lazy"
          />
        </div>
        <div class="blog-card-body">
          <div class="blog-card-meta">
            <span class="blog-card-eyebrow">{{ blogCategory(post).toUpperCase() }}</span>
            <span class="blog-card-date">
              {{ formatBlogDate(post.publish_date) }}
            </span>
          </div>
          <h3 class="blog-card-title">{{ post.title }}</h3>
          <p class="blog-card-excerpt">{{ post.excerpt }}</p>
        </div>
      </RouterLink>
    </div>

    <!-- "More articles" section if there's a long tail -->
    <div v-if="remaining.length > 0" class="blog-more">
      <h3 class="blog-more-heading">More to read</h3>
      <ul class="blog-more-list">
        <li v-for="post in remaining" :key="post.id">
          <RouterLink :to="`/blog/${post.slug}`" class="blog-more-link">
            <span class="blog-more-date">{{ formatBlogDate(post.publish_date) }}</span>
            <span class="blog-more-title">{{ post.title }}</span>
            <span class="blog-more-arrow" aria-hidden>→</span>
          </RouterLink>
        </li>
      </ul>
    </div>
  </section>

  <!-- ============= NEWSLETTER ============= -->
  <section class="container">
    <div class="newsletter">
      <div>
        <h3>Stay updated with newest <em>insights</em></h3>
        <p>
          Get practical advice on EOR, payroll, compliance, and global hiring —
          straight to your inbox. Short, clear, and no spam.
        </p>
      </div>
      <form @submit="submitNewsletter" class="newsletter-form">
        <label class="newsletter-label">
          <span>Email</span>
          <input
            v-model="newsletterEmail"
            type="email"
            placeholder="jane@flow.com"
            required
            :disabled="newsletterStatus === 'sending'"
          />
        </label>
        <button
          type="submit"
          class="btn-primary"
          :disabled="newsletterStatus === 'sending'"
        >
          {{ newsletterStatus === 'sending' ? 'Subscribing…' : 'Subscribe now' }}
          <span class="arrow">→</span>
        </button>
        <p v-if="newsletterStatus === 'ok'" class="newsletter-msg ok">
          Thanks — we'll be in touch.
        </p>
        <p v-if="newsletterStatus === 'error'" class="newsletter-msg error">
          Something went wrong. Try again or email us directly.
        </p>
      </form>
    </div>
  </section>

  <!-- ============= "HERE'S HOW WE BEGIN" ============= -->
  <section class="section container">
    <h2 class="section-title">Here's how we <em>begin</em></h2>
    <div class="begin-grid">
      <RouterLink to="/contact" class="begin-card begin-dark">
        <div>
          <h3>Let's talk about what's not working yet</h3>
          <p>
            When pricing and content finally speak the same language, results follow.
          </p>
        </div>
        <span class="begin-card-cta">
          Book a consultation <span aria-hidden>→</span>
        </span>
      </RouterLink>

      <RouterLink to="/employer-of-record" class="begin-card begin-warm">
        <div>
          <h3>Explore how we structure complex services</h3>
          <p>
            From core offers to brand voice, our frameworks help you align what you
            do with how it's understood.
          </p>
        </div>
        <span class="begin-card-cta">
          See our services <span aria-hidden>→</span>
        </span>
      </RouterLink>
    </div>
  </section>
</template>

<style scoped>
/* ============= DARK HERO ============= */
.blog-hero {
  background: var(--dark);
  color: var(--bg);
  padding: 120px 0 80px;
  margin-bottom: 0;
}
.blog-hero-inner {
  display: grid;
  gap: 48px;
}
.blog-hero-head {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  gap: 24px;
  flex-wrap: wrap;
}
.blog-hero-eyebrow {
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.55);
}
.blog-hero-title {
  font-family: var(--serif);
  font-size: clamp(48px, 6.5vw, 92px);
  line-height: 1;
  letter-spacing: -0.025em;
  font-weight: 400;
  color: var(--bg);
}
.blog-hero-title em {
  font-style: italic;
  color: var(--accent-warm);
}

/* Featured card */
.blog-featured {
  position: relative;
  display: block;
  border-radius: var(--radius-lg);
  overflow: hidden;
  aspect-ratio: 21 / 9;
  background: var(--dark-soft);
  color: var(--bg);
  text-decoration: none;
  transition: transform 0.4s;
}
.blog-featured:hover {
  transform: translateY(-3px);
}
.blog-featured-img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.blog-featured-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0.1) 0%, rgba(0, 0, 0, 0.7) 100%);
}
.blog-featured-meta {
  position: absolute;
  top: 28px;
  left: 32px;
  right: 32px;
  display: flex;
  justify-content: space-between;
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.85);
  z-index: 2;
}
.blog-featured-body {
  position: absolute;
  bottom: 32px;
  left: 32px;
  right: 32px;
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: end;
  gap: 24px;
  z-index: 2;
}
.blog-featured-body h2 {
  font-family: var(--serif);
  font-size: clamp(28px, 3.5vw, 44px);
  font-weight: 400;
  line-height: 1.1;
  max-width: 720px;
  color: var(--bg);
}
.blog-featured-cta {
  background: rgba(255, 255, 255, 0.12);
  border: 1px solid rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(6px);
  border-radius: 999px;
  padding: 10px 20px;
  font-size: 13px;
  font-weight: 500;
  display: inline-flex;
  gap: 8px;
  align-items: center;
  white-space: nowrap;
  transition: background 0.2s;
}
.blog-featured:hover .blog-featured-cta {
  background: rgba(255, 255, 255, 0.2);
}

.blog-featured-skeleton {
  background: var(--dark-soft);
}
.skeleton-shimmer {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(255, 255, 255, 0.05) 50%,
    transparent 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.6s infinite;
}
@keyframes shimmer {
  0% { background-position: 100% 0; }
  100% { background-position: -100% 0; }
}

/* ============= INTRO ============= */
.blog-intro {
  padding: 80px 0 40px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 64px;
}
.blog-intro-lead {
  font-family: var(--serif);
  font-size: clamp(24px, 2.4vw, 32px);
  line-height: 1.3;
  color: var(--ink);
  margin-bottom: 20px;
}
.blog-intro-body {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.7;
  margin-bottom: 14px;
}
.blog-intro-body:last-child {
  margin-bottom: 0;
}

/* ============= ARTICLES GRID ============= */
.blog-section-heading {
  margin-bottom: 48px;
}

.blog-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}
.blog-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  overflow: hidden;
  text-decoration: none;
  color: var(--ink);
  transition: transform 0.3s, box-shadow 0.3s;
  border: 1px solid var(--border);
  display: flex;
  flex-direction: column;
}
.blog-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 50px -25px rgba(0, 0, 0, 0.2);
}
.blog-card-img-wrap {
  aspect-ratio: 16 / 10;
  overflow: hidden;
  background: var(--accent-soft);
}
.blog-card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s;
}
.blog-card:hover .blog-card-img {
  transform: scale(1.04);
}
.blog-card-body {
  padding: 22px 22px 26px;
  flex: 1;
  display: flex;
  flex-direction: column;
}
.blog-card-meta {
  display: flex;
  justify-content: space-between;
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin-bottom: 14px;
  border-bottom: 1px solid var(--border);
  padding-bottom: 14px;
}
.blog-card-title {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  line-height: 1.25;
  margin-bottom: 10px;
  color: var(--ink);
}
.blog-card-excerpt {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.55;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.blog-card-skeleton {
  position: relative;
  background: var(--accent-soft);
  min-height: 360px;
}

.blog-error {
  text-align: center;
  color: var(--ink-soft);
  padding: 48px 0;
}
.blog-error a {
  color: var(--accent);
  text-decoration: underline;
}

/* "More to read" condensed list */
.blog-more {
  margin-top: 80px;
  padding-top: 48px;
  border-top: 1px solid var(--border);
}
.blog-more-heading {
  font-family: var(--serif);
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 28px;
}
.blog-more-list {
  list-style: none;
  display: flex;
  flex-direction: column;
}
.blog-more-list li {
  border-bottom: 1px solid var(--border);
}
.blog-more-link {
  display: grid;
  grid-template-columns: 160px 1fr 24px;
  gap: 24px;
  align-items: center;
  padding: 18px 0;
  color: var(--ink);
  transition: padding 0.2s, color 0.2s;
}
.blog-more-link:hover {
  padding-left: 12px;
  color: var(--accent);
}
.blog-more-date {
  font-size: 12px;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--ink-muted);
}
.blog-more-title {
  font-family: var(--serif);
  font-size: 19px;
  line-height: 1.3;
}
.blog-more-arrow {
  text-align: right;
  color: var(--ink-muted);
  transition: transform 0.2s, color 0.2s;
}
.blog-more-link:hover .blog-more-arrow {
  color: var(--accent);
  transform: translateX(4px);
}

/* ============= NEWSLETTER ============= */
.newsletter {
  background: var(--dark);
  color: var(--bg);
  border-radius: var(--radius-lg);
  padding: 56px 64px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 48px;
  align-items: center;
}
.newsletter h3 {
  font-family: var(--serif);
  font-size: clamp(28px, 3vw, 40px);
  font-weight: 400;
  line-height: 1.1;
  margin-bottom: 14px;
}
.newsletter h3 em {
  font-style: italic;
  color: var(--accent-warm);
}
.newsletter p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.6;
  max-width: 460px;
}
.newsletter-form {
  display: flex;
  flex-direction: column;
  gap: 14px;
}
.newsletter-label {
  display: flex;
  flex-direction: column;
  gap: 8px;
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.6);
}
.newsletter-label input {
  font-family: var(--sans);
  font-size: 15px;
  padding: 14px 18px;
  border: 1px solid rgba(255, 255, 255, 0.18);
  background: rgba(255, 255, 255, 0.04);
  border-radius: 12px;
  color: var(--bg);
  outline: none;
  transition: border-color 0.2s, background 0.2s;
}
.newsletter-label input::placeholder {
  color: rgba(255, 255, 255, 0.35);
}
.newsletter-label input:focus {
  border-color: var(--accent-warm);
  background: rgba(255, 255, 255, 0.08);
}
.newsletter-form .btn-primary {
  align-self: flex-start;
  background: var(--bg);
  color: var(--ink);
}
.newsletter-form .btn-primary:hover {
  background: var(--accent-warm);
}
.newsletter-msg {
  font-size: 13px;
  margin: 0;
}
.newsletter-msg.ok { color: var(--accent-warm); }
.newsletter-msg.error { color: #ffb4a8; }

/* ============= "BEGIN" CARDS ============= */
.begin-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
  margin-top: 48px;
}
.begin-card {
  border-radius: var(--radius-lg);
  padding: 56px 48px;
  min-height: 360px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 32px;
  text-decoration: none;
  transition: transform 0.3s;
}
.begin-card:hover {
  transform: translateY(-3px);
}
.begin-card h3 {
  font-family: var(--serif);
  font-size: clamp(28px, 3vw, 38px);
  font-weight: 400;
  line-height: 1.1;
  margin-bottom: 16px;
}
.begin-card p {
  font-size: 15px;
  line-height: 1.6;
  max-width: 380px;
}
.begin-card-cta {
  display: inline-flex;
  align-self: flex-start;
  padding: 12px 22px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.95);
  color: var(--ink);
  font-size: 13px;
  font-weight: 500;
  gap: 8px;
  align-items: center;
  transition: background 0.2s, transform 0.2s;
}
.begin-card-cta:hover {
  background: white;
  transform: translateY(-1px);
}

.begin-dark {
  background: var(--dark);
  color: var(--bg);
}
.begin-dark p {
  color: rgba(255, 255, 255, 0.7);
}

.begin-warm {
  background: linear-gradient(135deg, #f4a467 0%, #e87a4d 35%, #d35d4d 75%, #c25c8b 100%);
  color: white;
}
.begin-warm p {
  color: rgba(255, 255, 255, 0.9);
}

/* ============= RESPONSIVE ============= */
@media (max-width: 1024px) {
  .blog-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .blog-intro,
  .newsletter,
  .begin-grid {
    grid-template-columns: 1fr;
  }
  .newsletter {
    padding: 40px 32px;
  }
  .blog-more-link {
    grid-template-columns: 120px 1fr 24px;
    gap: 16px;
  }
}

@media (max-width: 640px) {
  .blog-hero {
    padding: 100px 0 60px;
  }
  .blog-featured {
    aspect-ratio: 4 / 5;
  }
  .blog-featured-body {
    grid-template-columns: 1fr;
    bottom: 24px;
    left: 24px;
    right: 24px;
  }
  .blog-featured-meta {
    top: 20px;
    left: 24px;
    right: 24px;
    font-size: 10px;
  }
  .blog-grid {
    grid-template-columns: 1fr;
  }
  .blog-more-link {
    grid-template-columns: 1fr;
    gap: 4px;
  }
  .blog-more-arrow {
    display: none;
  }
  .begin-card {
    padding: 40px 28px;
    min-height: 280px;
  }
}
</style>
