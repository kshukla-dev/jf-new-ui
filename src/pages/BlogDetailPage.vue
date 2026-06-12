<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue'
import { useRoute, RouterLink } from 'vue-router'
import { fetchBlogBySlug, fetchAllBlogs } from '@/services/blog'
import {
  applyStatCorrections,
  ensureImageAlt,
  demoteBodyH1s,
  formatBlogDate,
} from '@/utils/blog'
import AuthorAvatar from '@/components/ui/AuthorAvatar.vue'
import LeadModal from '@/components/ui/LeadModal.vue'
import type { BlogPost } from '@/types/blog'
import { h } from 'vue'
import { PortableText } from '@portabletext/vue'
import type { PortableTextVueComponents } from '@portabletext/vue'

const route = useRoute()
const slug = computed(() => String(route.params.slug ?? ''))

// Lead-capture modal
const modalOpen = ref(false)
function openModal() {
  modalOpen.value = true
}

function generateId(text: string) {
  return text.toLowerCase().replace(/\s+/g, '-').replace(/[^\w-]+/g, '')
}

const ptComponents: Partial<PortableTextVueComponents> = {
  block: {
    h2: (props: any, { slots }: any) => {
      const text = props.value.children?.map((c: any) => c.text).join('') || ''
      return h('h2', { id: generateId(text) }, slots.default?.())
    },
    h3: (props: any, { slots }: any) => {
      const text = props.value.children?.map((c: any) => c.text).join('') || ''
      return h('h3', { id: generateId(text) }, slots.default?.())
    }
  }
}

const blog = ref<BlogPost | null>(null)
const related = ref<BlogPost[]>([])
const loading = ref(true)
const notFound = ref(false)

const TOC_MARKER = '<!--BLOG_TOC_INSERT-->'

// Process CMS HTML once per blog change
const renderedContent = computed(() => {
  if (!blog.value) return ''
  let html = blog.value.page_content || blog.value.excerpt || ''
  html = ensureImageAlt(html, blog.value.title)
  html = applyStatCorrections(html)
  html = demoteBodyH1s(html)

  // Insert the table of contents at the marker (or strip the marker if no TOC).
  if (html.includes(TOC_MARKER)) {
    const toc = blog.value.toc_html
      ? `<nav class="blog-toc"><p class="blog-toc-title">On this page</p>${blog.value.toc_html}</nav>`
      : ''
    html = html.replace(TOC_MARKER, toc)
  }

  // Wrap every table so it scrolls horizontally on small screens instead of
  // breaking the layout.
  html = html.replace(/<table/gi, '<div class="blog-table-wrap"><table').replace(/<\/table>/gi, '</table></div>')

  return html
})

async function loadBlog(currentSlug: string) {
  loading.value = true
  notFound.value = false
  blog.value = null
  related.value = []

  const post = await fetchBlogBySlug(currentSlug)
  if (!post) {
    notFound.value = true
    loading.value = false
    return
  }

  blog.value = post

  // Update document title for SEO (basic — no full meta yet)
  document.title = `${post.meta_title || post.title} — Jackson & Frank`

  // Pull related articles. If the API returned them, use those; otherwise fetch
  // all blogs and pick 2 from the same category as fallback.
  if (post.related_articles && post.related_articles.length > 0) {
    // RelatedArticle and BlogPost have compatible-enough shapes for display
    related.value = post.related_articles.slice(0, 2) as unknown as BlogPost[]
  } else {
    try {
      const all = await fetchAllBlogs()
      const sameCat = (post.category_ids ?? '').split(',').map((s) => s.trim()).filter(Boolean)
      const candidates = all.filter(
        (b) =>
          b.slug !== post.slug &&
          sameCat.some((cid) =>
            (b.category_ids ?? '').split(',').map((s) => s.trim()).includes(cid)
          )
      )
      related.value = (candidates.length > 0 ? candidates : all.filter((b) => b.slug !== post.slug))
        .sort((a, b) => +new Date(b.publish_date) - +new Date(a.publish_date))
        .slice(0, 2)
    } catch {
      related.value = []
    }
  }

  loading.value = false
}

onMounted(() => loadBlog(slug.value))
watch(slug, (s) => {
  if (s) {
    window.scrollTo({ top: 0, behavior: 'smooth' })
    loadBlog(s)
  }
})
</script>

<template>
  <!-- ============= LOADING ============= -->
  <div v-if="loading" class="container blog-detail-loading">
    <div class="skeleton skeleton-title" />
    <div class="skeleton skeleton-meta" />
    <div class="skeleton skeleton-hero" />
    <div class="skeleton skeleton-line" v-for="i in 6" :key="i" />
  </div>

  <!-- ============= NOT FOUND ============= -->
  <div v-else-if="notFound" class="container blog-detail-404">
    <span class="tag">404 — Article not found</span>
    <h1 class="section-title">We couldn't find that <em>article</em></h1>
    <p>The link may be outdated or the post may have moved.</p>
    <RouterLink to="/blog" class="btn-primary">
      Back to all articles <span class="arrow">→</span>
    </RouterLink>
  </div>

  <!-- ============= ARTICLE ============= -->
  <article v-else-if="blog" class="blog-detail">
    <!-- Breadcrumb -->
    <nav class="container blog-breadcrumb" aria-label="Breadcrumb">
      <RouterLink to="/">Home</RouterLink>
      <span class="sep">/</span>
      <RouterLink to="/blog">Blog</RouterLink>
      <span class="sep">/</span>
      <span class="current">{{ blog.title }}</span>
    </nav>

    <!-- Header -->
    <header class="container blog-detail-header">
      <span class="tag">
        Insights · {{ formatBlogDate(blog.publish_date) }}
      </span>
      <h1>{{ blog.title }}</h1>
      <div class="blog-detail-meta">
        <span v-if="blog.author?.name" class="meta-author">
          <AuthorAvatar :name="blog.author.name" size="sm" />
          <span>{{ blog.author.name }}</span>
        </span>
        <span v-if="blog.estimated_reading_time" class="meta-time">
          · {{ blog.estimated_reading_time }} min read
        </span>
      </div>
    </header>

    <!-- Hero image -->
    <figure v-if="blog.image_url" class="container blog-detail-hero">
      <img :src="blog.image_url" :alt="blog.title" />
    </figure>

    <!-- Body -->
    <div class="container blog-detail-body-wrap" :class="{ 'has-sidebar': blog.in_this_guide && blog.in_this_guide.length > 0 }">
      <div class="blog-main-column">
        <div class="blog-detail-body blog-content">
          <PortableText v-if="blog.body" :value="blog.body" :components="ptComponents" />
          <div v-else v-html="renderedContent" />
        </div>

        <!-- Inline lead-capture CTA -->
        <div class="blog-lead-cta">
          <div>
            <span class="tag">Need a hand?</span>
            <h3>Talk to an expert about this</h3>
            <p>Get tailored guidance for your situation — most questions answered within 24 hours.</p>
          </div>
          <button class="btn-primary" @click="openModal">
            Get expert help <span class="arrow">→</span>
          </button>
        </div>

        <!-- Author footer card -->
        <div v-if="blog.author?.name" class="author-footer">
          <span class="tag">About the author</span>
          <div class="author-footer-card">
            <AuthorAvatar :name="blog.author.name" size="lg" />
            <div>
              <strong>{{ blog.author.name }}</strong>
              <span>Author at Jackson &amp; Frank</span>
              <p v-if="blog.author.email">
                <a :href="`mailto:${blog.author.email}`">{{ blog.author.email }}</a>
              </p>
            </div>
          </div>
        </div>
      </div>

      <aside v-if="blog.in_this_guide && blog.in_this_guide.length > 0" class="blog-sidebar">
        <div class="in-this-guide">
          <div class="guide-header">IN THIS GUIDE</div>
          <ul class="guide-list">
            <li v-for="(item, i) in blog.in_this_guide" :key="i">
              <a :href="item.url">{{ item.title }}</a>
            </li>
          </ul>
        </div>
      </aside>
    </div>

    <!-- Related articles -->
    <section v-if="related.length > 0" class="section container">
      <h2 class="section-title">Recent <em>articles</em></h2>
      <div class="related-grid">
        <RouterLink
          v-for="post in related"
          :key="post.id"
          :to="`/blog/${post.slug}`"
          class="related-card"
        >
          <div class="related-card-img-wrap">
            <img
              v-if="post.image_url"
              :src="post.image_url"
              :alt="post.title"
              loading="lazy"
            />
          </div>
          <div class="related-card-body">
            <div class="related-card-meta">
              <span class="related-card-eyebrow">ARTICLE</span>
              <span class="related-card-date">{{ formatBlogDate(post.publish_date) }}</span>
            </div>
            <h3 class="related-card-title">{{ post.title }}</h3>
            <p class="related-card-excerpt">{{ post.excerpt }}</p>
          </div>
        </RouterLink>
      </div>
    </section>

    <!-- Warm CTA -->
    <section class="cta-warm-wrap">
      <div class="cta-warm">
        <span class="cta-tag">Talk to us</span>
        <h2>We're ready to respond <em>to your doubts</em></h2>
        <p>
          Understanding your hurdles and helping you through them is the priority.
        </p>
        <div class="cta-warm-buttons">
          <button class="btn-primary" @click="openModal">
            Get in touch <span class="arrow">→</span>
          </button>
          <RouterLink to="/blog" class="btn-secondary">More articles</RouterLink>
        </div>
      </div>
    </section>

    <!-- Lead-capture modal -->
    <LeadModal
      :open="modalOpen"
      :title="`Talk to us about ${blog.title.length > 40 ? 'this topic' : blog.title}`"
      subtitle="Share your details and our team will get back to you within 24 hours."
      reason="general_inquiry"
      @close="modalOpen = false"
    />
  </article>
</template>

<style scoped>
.blog-detail {
  padding-top: 120px;
  padding-bottom: 80px;
}

/* ====== BREADCRUMB ====== */
.blog-breadcrumb {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--ink-muted);
  margin-bottom: 32px;
}
.blog-breadcrumb a {
  color: var(--ink-soft);
  transition: color 0.2s;
}
.blog-breadcrumb a:hover {
  color: var(--accent);
}
.blog-breadcrumb .sep {
  color: var(--ink-muted);
}
.blog-breadcrumb .current {
  color: var(--ink);
  max-width: 360px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* ====== HEADER ====== */
.blog-detail-header {
  max-width: 820px;
  margin: 0 auto;
  text-align: center;
  margin-bottom: 48px;
}
.blog-detail-header .tag {
  margin-bottom: 24px;
}
.blog-detail-header h1 {
  font-family: var(--serif);
  font-size: clamp(34px, 4.5vw, 60px);
  line-height: 1.08;
  font-weight: 400;
  letter-spacing: -0.015em;
  margin-bottom: 28px;
  color: var(--ink);
}
.blog-detail-meta {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  font-size: 14px;
  color: var(--ink-soft);
}
.meta-author {
  display: inline-flex;
  align-items: center;
  gap: 10px;
}
.meta-author span {
  font-weight: 500;
}
.meta-time {
  color: var(--ink-muted);
}

/* ====== HERO IMAGE ====== */
/* Blog banners often have text baked in, so we show the WHOLE image at its
   natural aspect ratio (no crop) rather than forcing 16:9 + cover. */
.blog-detail-hero {
  max-width: 1080px;
  margin: 0 auto 64px;
  border-radius: var(--radius-lg);
  overflow: hidden;
  background: var(--accent-soft);
}
.blog-detail-hero img {
  width: 100%;
  height: auto;
  display: block;
}

/* ====== BODY ====== */
.blog-detail-body-wrap {
  max-width: 720px;
  margin: 0 auto;
}
.blog-detail-body-wrap.has-sidebar {
  max-width: 1080px;
}
@media (min-width: 1024px) {
  .blog-detail-body-wrap.has-sidebar {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 40px;
    align-items: start;
  }
}
.blog-main-column {
  max-width: 720px;
  width: 100%;
  margin: 0 auto;
}

.blog-sidebar {
  position: sticky;
  top: 100px;
  margin-top: 56px;
}
@media (max-width: 1023px) {
  .blog-sidebar {
    margin-top: 0;
    margin-bottom: 40px;
    order: -1;
  }
  .blog-detail-body-wrap.has-sidebar {
    display: flex;
    flex-direction: column;
  }
}

.in-this-guide {
  background: #ffffff;
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}
.guide-header {
  background: var(--ink);
  color: #fff;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.05em;
  padding: 16px 20px;
  text-transform: uppercase;
}
.guide-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.guide-list li {
  border-bottom: 1px solid var(--border);
}
.guide-list li:last-child {
  border-bottom: none;
}
.guide-list a {
  display: block;
  padding: 14px 20px;
  color: var(--ink-soft);
  font-weight: 600;
  font-size: 14px;
  text-decoration: none;
  line-height: 1.4;
  transition: background 0.2s, color 0.2s;
}
.guide-list a:hover {
  background: var(--bg-hover, #f9fafb);
  color: var(--accent);
}

.blog-detail-body {
  font-family: var(--sans);
  font-size: 17px;
  line-height: 1.75;
  color: var(--ink);
}
/* :deep() because content is rendered via v-html */
.blog-detail-body :deep(h2),
.blog-detail-body :deep(h3) {
  font-family: var(--serif);
  font-weight: 400;
  letter-spacing: -0.01em;
  margin-top: 56px;
  margin-bottom: 20px;
  line-height: 1.2;
  color: var(--ink);
}
.blog-detail-body :deep(h2) { font-size: 30px; }
.blog-detail-body :deep(h3) { font-size: 22px; }
.blog-detail-body :deep(h4) {
  font-family: var(--sans);
  font-size: 16px;
  font-weight: 600;
  margin-top: 32px;
  margin-bottom: 12px;
  letter-spacing: 0.02em;
}
.blog-detail-body :deep(p) {
  margin: 18px 0;
  color: var(--ink-soft);
}
.blog-detail-body :deep(strong) {
  color: var(--ink);
  font-weight: 600;
}
.blog-detail-body :deep(em) {
  font-style: italic;
}
.blog-detail-body :deep(a) {
  color: var(--accent);
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1px;
}
.blog-detail-body :deep(a:hover) {
  text-decoration-thickness: 2px;
}
.blog-detail-body :deep(ul),
.blog-detail-body :deep(ol) {
  margin: 20px 0;
  padding-left: 28px;
  color: var(--ink-soft);
}
.blog-detail-body :deep(li) {
  margin: 8px 0;
  line-height: 1.7;
}
.blog-detail-body :deep(blockquote) {
  margin: 32px 0;
  padding: 24px 32px;
  border-left: 3px solid var(--accent);
  background: var(--bg-card);
  font-family: var(--serif);
  font-size: 22px;
  font-style: italic;
  line-height: 1.4;
  color: var(--ink);
  border-radius: 0 12px 12px 0;
}
.blog-detail-body :deep(img) {
  width: 100%;
  height: auto;
  border-radius: 12px;
  margin: 28px 0;
}
.blog-detail-body :deep(figure) {
  margin: 32px 0;
}
.blog-detail-body :deep(figcaption) {
  font-size: 13px;
  color: var(--ink-muted);
  text-align: center;
  margin-top: 10px;
}
/* Tables: scroll horizontally on overflow, zebra rows, clean borders */
.blog-detail-body :deep(.blog-table-wrap) {
  overflow-x: auto;
  margin: 28px 0;
  border: 1px solid var(--border);
  border-radius: 12px;
  -webkit-overflow-scrolling: touch;
}
.blog-detail-body :deep(table) {
  width: 100%;
  border-collapse: collapse;
  margin: 0;
  font-size: 13.5px;
  /* Wide enough that 6–7 column tables get readable column widths;
     the wrapper scrolls horizontally instead of squashing text. */
  min-width: 880px;
}
.blog-detail-body :deep(th),
.blog-detail-body :deep(td) {
  padding: 11px 14px;
  text-align: left;
  border-bottom: 1px solid var(--border);
  border-right: 1px solid var(--border);
  vertical-align: top;
  line-height: 1.5;
}
.blog-detail-body :deep(th:last-child),
.blog-detail-body :deep(td:last-child) {
  border-right: none;
}
.blog-detail-body :deep(tr:last-child td) {
  border-bottom: none;
}
.blog-detail-body :deep(thead th) {
  background: var(--ink);
  color: var(--bg);
  font-weight: 600;
  font-size: 12px;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  white-space: nowrap;
}
.blog-detail-body :deep(tbody tr:nth-child(even)) {
  background: var(--bg-card);
}

/* ====== TABLE OF CONTENTS ====== */
.blog-detail-body :deep(.blog-toc) {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 24px 28px;
  margin: 8px 0 40px;
}
.blog-detail-body :deep(.blog-toc-title) {
  font-size: 11px;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--ink-muted);
  margin: 0 0 14px;
}
.blog-detail-body :deep(.blog-toc ol) {
  margin: 0;
  padding-left: 20px;
  counter-reset: toc;
}
.blog-detail-body :deep(.blog-toc > ol) {
  list-style: none;
  padding-left: 0;
}
.blog-detail-body :deep(.blog-toc > ol > li) {
  counter-increment: toc;
  margin: 6px 0;
  padding-left: 30px;
  position: relative;
}
.blog-detail-body :deep(.blog-toc > ol > li)::before {
  content: counter(toc, decimal-leading-zero);
  position: absolute;
  left: 0;
  font-family: var(--serif);
  font-style: italic;
  color: var(--accent);
  font-size: 14px;
}
.blog-detail-body :deep(.blog-toc a) {
  color: var(--ink);
  text-decoration: none;
  font-size: 15px;
  line-height: 1.5;
}
.blog-detail-body :deep(.blog-toc a:hover) {
  color: var(--accent);
  text-decoration: underline;
}
.blog-detail-body :deep(.blog-toc ol ol) {
  padding-left: 16px;
  margin: 6px 0;
}
.blog-detail-body :deep(.blog-toc ol ol a) {
  font-size: 14px;
  color: var(--ink-soft);
}

/* ====== DETAILS / SUMMARY ACCORDIONS (country toggles) ====== */
.blog-detail-body :deep(details.country-accordion) {
  border: 1px solid var(--border);
  border-radius: var(--radius);
  margin: 16px 0;
  background: var(--bg-card);
  overflow: hidden;
}
.blog-detail-body :deep(details.country-accordion[open]) {
  border-color: var(--accent);
  box-shadow: 0 12px 36px -20px rgba(20, 51, 105, 0.4);
}
.blog-detail-body :deep(details.country-accordion > summary) {
  display: block; /* removes the native disclosure triangle reliably */
  list-style: none;
  cursor: pointer;
  padding: 20px 56px 20px 24px;
  position: relative;
  user-select: none;
}
.blog-detail-body :deep(details.country-accordion > summary::-webkit-details-marker) {
  display: none;
}
.blog-detail-body :deep(details.country-accordion > summary::marker) {
  content: '';
}
/* the content authors put an <h3> inside <summary>; render it inline */
.blog-detail-body :deep(details.country-accordion > summary h3) {
  display: inline;
  margin: 0;
  font-family: var(--serif);
  font-size: 20px;
  font-weight: 400;
  color: var(--ink);
  letter-spacing: 0;
}
.blog-detail-body :deep(details.country-accordion > summary strong) {
  font-weight: 400;
}
/* +/- toggle indicator */
.blog-detail-body :deep(details.country-accordion > summary)::after {
  content: '+';
  position: absolute;
  right: 22px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 24px;
  line-height: 1;
  color: var(--accent);
  transition: transform 0.2s;
}
.blog-detail-body :deep(details.country-accordion[open] > summary)::after {
  content: '−';
}
.blog-detail-body :deep(details.country-accordion > summary:hover h3) {
  color: var(--accent);
}
/* body of the accordion */
.blog-detail-body :deep(details.country-accordion > *:not(summary)) {
  padding: 0 24px;
}
.blog-detail-body :deep(details.country-accordion > *:not(summary):last-child) {
  padding-bottom: 22px;
}
.blog-detail-body :deep(details.country-accordion[open] > summary) {
  border-bottom: 1px solid var(--border);
  margin-bottom: 18px;
}
/* tone variants */
.blog-detail-body :deep(details.country-accordion.info[open]) {
  border-color: var(--accent);
}
.blog-detail-body :deep(details.country-accordion.warning) {
  background: #fdf8f0;
}
.blog-detail-body :deep(details.country-accordion.warning[open]) {
  border-color: var(--accent-warm);
}
.blog-detail-body :deep(code) {
  background: var(--bg-card);
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 0.9em;
  font-family: 'SF Mono', Menlo, monospace;
}
.blog-detail-body :deep(pre) {
  background: var(--dark);
  color: var(--bg);
  padding: 20px;
  border-radius: 12px;
  overflow-x: auto;
  margin: 24px 0;
}
.blog-detail-body :deep(pre code) {
  background: transparent;
  padding: 0;
  color: inherit;
}
.blog-detail-body :deep(hr) {
  border: 0;
  border-top: 1px solid var(--border);
  margin: 48px 0;
}

/* ====== INLINE LEAD CTA ====== */
.blog-lead-cta {
  margin-top: 64px;
  padding: 32px 36px;
  background: var(--accent-soft);
  border-radius: var(--radius-lg);
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 24px;
  align-items: center;
}
.blog-lead-cta .tag {
  margin-bottom: 8px;
  color: var(--accent);
}
.blog-lead-cta h3 {
  font-family: var(--serif);
  font-size: 26px;
  font-weight: 400;
  line-height: 1.15;
  margin-bottom: 6px;
  color: var(--ink);
}
.blog-lead-cta p {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.55;
}
.blog-lead-cta .btn-primary { white-space: nowrap; }

/* ====== AUTHOR FOOTER ====== */
.author-footer {
  margin-top: 80px;
  padding-top: 40px;
  border-top: 1px solid var(--border);
}
.author-footer .tag {
  margin-bottom: 16px;
}
.author-footer-card {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 24px;
  background: var(--bg-card);
  border-radius: var(--radius);
  border: 1px solid var(--border);
}
.author-footer-card strong {
  display: block;
  font-size: 17px;
  font-weight: 600;
  color: var(--ink);
  margin-bottom: 4px;
}
.author-footer-card span {
  font-size: 13px;
  color: var(--ink-muted);
}
.author-footer-card p {
  margin-top: 6px;
  font-size: 13px;
}
.author-footer-card a {
  color: var(--accent);
  text-decoration: underline;
  text-underline-offset: 3px;
}

/* ====== RELATED ====== */
.related-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
  margin-top: 48px;
}
.related-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
  text-decoration: none;
  color: var(--ink);
  transition: transform 0.3s, box-shadow 0.3s;
}
.related-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 20px 50px -25px rgba(0, 0, 0, 0.2);
}
.related-card-img-wrap {
  aspect-ratio: 16 / 9;
  background: var(--accent-soft);
  overflow: hidden;
}
.related-card-img-wrap img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s;
}
.related-card:hover .related-card-img-wrap img {
  transform: scale(1.04);
}
.related-card-body {
  padding: 22px 24px 26px;
}
.related-card-meta {
  display: flex;
  justify-content: space-between;
  font-size: 11px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--ink-muted);
  padding-bottom: 12px;
  border-bottom: 1px solid var(--border);
  margin-bottom: 14px;
}
.related-card-title {
  font-family: var(--serif);
  font-size: 22px;
  font-weight: 400;
  line-height: 1.25;
  margin-bottom: 10px;
}
.related-card-excerpt {
  font-size: 14px;
  color: var(--ink-soft);
  line-height: 1.55;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* ====== WARM CTA ====== */
.cta-warm-wrap {
  margin-top: 80px;
}
.cta-warm {
  background: linear-gradient(135deg, #f4a467 0%, #e87a4d 30%, #d35d4d 65%, #c25c8b 100%);
  color: white;
  padding: 80px 32px;
  text-align: center;
  border-radius: 0;
}
.cta-warm .cta-tag {
  display: inline-block;
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  margin-bottom: 20px;
  color: rgba(255, 255, 255, 0.8);
}
.cta-warm h2 {
  font-family: var(--serif);
  font-size: clamp(36px, 5vw, 64px);
  line-height: 1.05;
  font-weight: 400;
  letter-spacing: -0.02em;
  margin-bottom: 16px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}
.cta-warm h2 em {
  font-style: italic;
}
.cta-warm p {
  font-size: 16px;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 32px;
  max-width: 560px;
  margin-left: auto;
  margin-right: auto;
}
.cta-warm-buttons {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}
.cta-warm .btn-primary {
  background: white;
  color: var(--ink);
}
.cta-warm .btn-primary:hover {
  background: var(--ink);
  color: white;
}
.cta-warm .btn-secondary {
  border-color: rgba(255, 255, 255, 0.7);
  color: white;
}
.cta-warm .btn-secondary:hover {
  background: rgba(255, 255, 255, 0.9);
  color: var(--ink);
}

/* ====== 404 ====== */
.blog-detail-404 {
  padding: 200px 0 120px;
  text-align: center;
}
.blog-detail-404 .tag {
  margin-bottom: 12px;
}
.blog-detail-404 h1 {
  margin-bottom: 16px;
}
.blog-detail-404 p {
  color: var(--ink-soft);
  margin-bottom: 32px;
}

/* ====== LOADING ====== */
.blog-detail-loading {
  padding-top: 160px;
  padding-bottom: 100px;
  max-width: 820px;
}
.skeleton {
  background: var(--accent-soft);
  border-radius: 8px;
  margin-bottom: 16px;
  position: relative;
  overflow: hidden;
}
.skeleton::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(255, 255, 255, 0.5) 50%,
    transparent 100%
  );
  animation: shimmer 1.4s infinite;
}
@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}
.skeleton-title { height: 56px; width: 80%; margin-bottom: 24px; }
.skeleton-meta { height: 18px; width: 240px; margin: 0 auto 32px; }
.skeleton-hero { height: 420px; margin-bottom: 32px; }
.skeleton-line { height: 16px; }

/* ====== RESPONSIVE ====== */
@media (max-width: 900px) {
  .blog-detail-body {
    font-size: 16px;
  }
  .blog-detail-body :deep(h2) { font-size: 26px; margin-top: 40px; }
  .related-grid {
    grid-template-columns: 1fr;
  }
  .cta-warm {
    padding: 60px 24px;
  }
}
</style>
