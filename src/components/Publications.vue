<template>
  <section id="publications">
    <div class="section">
      <h2 class="section-title reveal" ref="titleEl">Publikasi Jurnal Ilmiah</h2>
      <p class="section-subtitle reveal" ref="subtitleEl">
        Karya ilmiah dan hasil penelitian yang telah dipublikasikan
      </p>

      <div class="pub-grid">
        <div
          v-for="(pub, idx) in publications"
          :key="pub.id"
          class="pub-card card reveal"
          :ref="el => pubRefs.push(el)"
          :style="{ '--accent': pub.color }"
        >
          <!-- Badge and Year -->
          <div class="pub-header-meta">
            <span class="badge pub-badge" :style="{ background: `${pub.color}15`, color: pub.color, borderColor: `${pub.color}35` }">
              {{ pub.category }}
            </span>
            <span class="pub-year">
              <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
                <line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/>
                <line x1="3" y1="10" x2="21" y2="10"/>
              </svg>
              {{ pub.year }}
            </span>
          </div>

          <!-- Title -->
          <h3 class="pub-title">{{ pub.title }}</h3>

          <!-- Authors -->
          <p class="pub-authors">
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/>
              <circle cx="9" cy="7" r="4"/>
              <path d="M23 21v-2a4 4 0 0 0-3-3.87"/>
              <path d="M16 3.13a4 4 0 0 1 0 7.75"/>
            </svg>
            <span v-html="formatAuthors(pub.authors)"></span>
          </p>

          <!-- Journal Name -->
          <p class="pub-journal">
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/>
              <path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>
            </svg>
            <strong>{{ pub.journal }}</strong> — <span class="pub-volume">{{ pub.volume }}</span>
          </p>

          <!-- Abstract / Description -->
          <div class="pub-abstract">
            <h4 class="abstract-title">Abstrak:</h4>
            <p class="abstract-text">{{ pub.abstract }}</p>
          </div>

          <!-- Keywords / Technologies -->
          <div class="pub-tech">
            <span v-for="t in pub.tech" :key="t" class="tech-tag">{{ t }}</span>
          </div>

          <!-- DOI / Link CTA -->
          <div class="pub-footer" v-if="pub.doi && pub.doi !== '#'">
            <a :href="pub.doi" target="_blank" rel="noopener noreferrer" class="pub-link-btn" :style="{ color: pub.color }">
              <span>Lihat Publikasi</span>
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                <polyline points="15 3 21 3 21 9"/>
                <line x1="10" y1="14" x2="21" y2="3"/>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  publications: { type: Array, required: true }
});

const titleEl = ref(null);
const subtitleEl = ref(null);
const pubRefs = [];

// Bold Bimo Bagas Riyanto in authors string
function formatAuthors(authorsString) {
  if (!authorsString) return '';
  // Match Bimo Bagas Riyanto (case-insensitive) and wrap in <strong>
  const target = 'Bimo Bagas Riyanto';
  const regex = new RegExp(`(${target})`, 'gi');
  return authorsString.replace(regex, '<strong>$1</strong>');
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('visible');
        }
      });
    },
    { threshold: 0.1 }
  );

  [titleEl.value, subtitleEl.value, ...pubRefs]
    .filter(Boolean)
    .forEach((el, i) => {
      el.style.transitionDelay = `${i * 0.1}s`;
      observer.observe(el);
    });
});
</script>

<style scoped>
.pub-grid {
  display: flex;
  flex-direction: column;
  gap: 28px;
  max-width: 850px;
  margin: 0 auto;
}

.pub-card {
  padding: 32px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  position: relative;
  overflow: hidden;
}

/* Accent top border highlight on hover */
.pub-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: var(--accent, var(--color-primary));
  opacity: 0;
  transition: var(--transition);
}

.pub-card:hover::after {
  opacity: 1;
}

.pub-header-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
}

.pub-badge {
  text-transform: uppercase;
  font-size: 0.7rem;
  letter-spacing: 0.5px;
}

.pub-year {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.85rem;
  color: var(--color-text-dim);
}

.pub-title {
  font-size: 1.3rem;
  line-height: 1.4;
  color: var(--color-text);
  font-weight: 800;
  transition: var(--transition);
}

.pub-card:hover .pub-title {
  color: var(--accent, var(--color-primary));
}

.pub-authors, .pub-journal {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.92rem;
  color: var(--color-text-muted);
}

.pub-authors svg, .pub-journal svg {
  flex-shrink: 0;
  color: var(--color-text-dim);
}

.pub-authors :deep(strong) {
  color: var(--color-text);
  font-weight: 600;
}

.pub-journal strong {
  color: var(--color-text);
}

.pub-volume {
  font-style: italic;
  font-size: 0.88rem;
}

.pub-abstract {
  background: rgba(37, 99, 235, 0.03);
  border-left: 3px solid var(--accent, var(--color-primary));
  padding: 16px 20px;
  border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
  margin: 4px 0;
}

.abstract-title {
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--color-text);
  margin-bottom: 6px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.abstract-text {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  line-height: 1.6;
}

.pub-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}

.tech-tag {
  padding: 4px 11px;
  border-radius: 999px;
  font-size: 0.75rem;
  font-weight: 600;
  background: rgba(37,99,235,0.06);
  color: var(--color-text-muted);
  border: 1px solid rgba(37,99,235,0.15);
}

.pub-footer {
  padding-top: 16px;
  border-top: 1px solid var(--color-border);
  display: flex;
  justify-content: flex-end;
}

.pub-link-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 0.88rem;
  font-weight: 700;
  transition: var(--transition);
}

.pub-link-btn:hover {
  opacity: 0.85;
  transform: translateX(4px);
}

@media (max-width: 640px) {
  .pub-card {
    padding: 24px;
  }
  .pub-title {
    font-size: 1.15rem;
  }
  .pub-header-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }
  .pub-year {
    align-self: flex-end;
    margin-top: -24px;
  }
}
</style>
