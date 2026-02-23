<template>
  <section id="certificates">
    <div class="section">
      <h2 class="section-title reveal" ref="titleEl">Sertifikat</h2>
      <p class="section-subtitle reveal" ref="subtitleEl">
        Sertifikasi &amp; penghargaan yang pernah saya raih
      </p>

      <!-- Filter tabs -->
      <div class="cert-filters reveal" ref="filterEl">
        <button
          v-for="cat in categories"
          :key="cat"
          class="filter-btn"
          :class="{ active: activeFilter === cat }"
          @click="onFilterChange(cat)"
        >{{ cat }}</button>
      </div>

      <!-- Carousel wrapper -->
      <div class="carousel-wrapper reveal" ref="carouselEl">
        <div class="carousel-track" ref="trackEl">
          <div
            v-for="(cert, index) in filtered"
            :key="cert.id"
            class="carousel-slide"
            :class="{ active: index === activeIndex }"
          >
            <div class="cert-image-wrap" @click="openLightbox(cert)" title="Klik untuk melihat sertifikat">
              <template v-if="cert.pdf">
                <object
                  :data="cert.pdf + '#toolbar=0&navpanes=0&scrollbar=0&view=FitH'"
                  type="application/pdf"
                  class="cert-pdf"
                  :title="cert.name"
                >
                  <img
                    v-if="cert.image"
                    :src="cert.image"
                    :alt="cert.name"
                    class="cert-image"
                  />
                  <div v-else class="cert-pdf-fallback">
                    <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z"/><polyline points="14 2 14 8 20 8"/></svg>
                    <span>{{ cert.name }}</span>
                  </div>
                </object>
              </template>

              <img
                v-else
                :src="cert.image"
                :alt="cert.name"
                class="cert-image"
                loading="lazy"
              />

              <div class="cert-click-hint">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/><line x1="11" y1="8" x2="11" y2="14"/><line x1="8" y1="11" x2="14" y2="11"/></svg>
                Lihat Sertifikat
              </div>

              <div class="cert-image-overlay">
                <span class="badge">{{ cert.category }}</span>
              </div>
            </div>

            <div class="cert-info">
              <h3 class="cert-name">{{ cert.name }}</h3>
              <p class="cert-issuer">
                <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
                  fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <circle cx="12" cy="8" r="7"/>
                  <polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"/>
                </svg>
                {{ cert.issuer }}
              </p>
              <p class="cert-description">{{ cert.description }}</p>
              <div class="cert-footer">
                <span class="cert-year">
                  <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24"
                    fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect x="3" y="4" width="18" height="18" rx="2" ry="2"/>
                    <line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/>
                    <line x1="3" y1="10" x2="21" y2="10"/>
                  </svg>
                  {{ cert.year }}
                </span>
                <button class="cert-link-btn" @click="openLightbox(cert)">
                  Lihat Sertifikat →
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Nav arrows -->
        <button class="carousel-btn prev" @click="prev" :disabled="filtered.length <= 1">
          <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24"
            fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="15 18 9 12 15 6"/>
          </svg>
        </button>
        <button class="carousel-btn next" @click="next" :disabled="filtered.length <= 1">
          <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24"
            fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="9 18 15 12 9 6"/>
          </svg>
        </button>
      </div>

      <!-- Dot indicators -->
      <div class="carousel-dots" v-if="filtered.length > 1">
        <button
          v-for="(cert, i) in filtered"
          :key="cert.id"
          class="dot"
          :class="{ active: i === activeIndex }"
          @click="goTo(i)"
        />
      </div>

      <!-- Counter -->
      <p class="carousel-counter" v-if="filtered.length > 0">
        {{ activeIndex + 1 }} / {{ filtered.length }}
      </p>
    </div>
  </section>

  <!-- ── Lightbox Modal ── -->
  <Teleport to="body">
    <Transition name="lb">
      <div v-if="lightbox.open" class="lb-backdrop" @click.self="closeLightbox">
        <div class="lb-box" @contextmenu.prevent>
          <!-- Header -->
          <div class="lb-header">
            <div class="lb-title-wrap">
              <span class="lb-title">{{ lightbox.cert?.name }}</span>
              <span class="lb-issuer">{{ lightbox.cert?.issuer }}</span>
            </div>
            <button class="lb-close" @click="closeLightbox" title="Tutup">
              <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
              </svg>
            </button>
          </div>

          <!-- Content -->
          <div class="lb-content" @contextmenu.prevent>
            <!-- PDF -->
            <template v-if="lightbox.cert?.pdf">
              <object
                :data="lightbox.cert.pdf + '#toolbar=0&navpanes=0&scrollbar=0&view=FitH'"
                type="application/pdf"
                class="lb-pdf"
                :title="lightbox.cert.name"
              >
                <img
                  v-if="lightbox.cert.image"
                  :src="lightbox.cert.image"
                  :alt="lightbox.cert.name"
                  class="lb-image"
                  draggable="false"
                />
                <div v-else class="lb-pdf-fallback">
                  <svg xmlns="http://www.w3.org/2000/svg" width="56" height="56" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z"/><polyline points="14 2 14 8 20 8"/></svg>
                  <p>PDF tidak dapat ditampilkan di browser ini.</p>
                </div>
              </object>
            </template>
            <!-- Image only -->
            <img
              v-else-if="lightbox.cert?.image"
              :src="lightbox.cert.image"
              :alt="lightbox.cert.name"
              class="lb-image"
              draggable="false"
            />
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, reactive, computed, watch, onMounted, onUnmounted } from 'vue';

const props = defineProps({ certificates: { type: Array, required: true } });

const titleEl    = ref(null);
const subtitleEl = ref(null);
const filterEl   = ref(null);
const carouselEl = ref(null);

const allCats = ['Semua', ...new Set(props.certificates.map(c => c.category))];
const categories   = ref(allCats);
const activeFilter = ref('Semua');
const activeIndex  = ref(0);

const filtered = computed(() =>
  activeFilter.value === 'Semua'
    ? props.certificates
    : props.certificates.filter(c => c.category === activeFilter.value)
);

function onFilterChange(cat) {
  activeFilter.value = cat;
  activeIndex.value  = 0;
}

function prev() {
  activeIndex.value = (activeIndex.value - 1 + filtered.value.length) % filtered.value.length;
}

function next() {
  activeIndex.value = (activeIndex.value + 1) % filtered.value.length;
}

function goTo(i) {
  activeIndex.value = i;
}

// Reset index when filtered list changes
watch(filtered, () => { activeIndex.value = 0; });

// ── Lightbox ──
const lightbox = reactive({ open: false, cert: null });

function openLightbox(cert) {
  lightbox.cert = cert;
  lightbox.open = true;
  document.body.style.overflow = 'hidden';
}

function closeLightbox() {
  lightbox.open = false;
  document.body.style.overflow = '';
}

function onKeydown(e) {
  if (e.key === 'Escape') closeLightbox();
}

// Reveal animation
onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); }),
    { threshold: 0.1 }
  );
  [titleEl.value, subtitleEl.value, filterEl.value, carouselEl.value]
    .filter(Boolean)
    .forEach((el, i) => {
      el.style.transitionDelay = `${i * 0.08}s`;
      observer.observe(el);
    });

  window.addEventListener('keydown', onKeydown);
});

onUnmounted(() => {
  window.removeEventListener('keydown', onKeydown);
  document.body.style.overflow = '';
});
</script>

<style scoped>
/* Filters */
.cert-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-bottom: 40px;
}
.filter-btn {
  padding: 8px 20px;
  border-radius: 999px;
  border: 1.5px solid var(--color-border);
  background: transparent;
  color: var(--color-text-muted);
  font-family: var(--font-family);
  font-size: 0.88rem;
  font-weight: 500;
  cursor: pointer;
  transition: var(--transition);
}
.filter-btn:hover { border-color: var(--color-primary); color: var(--color-primary-light); }
.filter-btn.active {
  background: var(--color-primary);
  border-color: var(--color-primary);
  color: #fff;
}

/* Carousel wrapper */
.carousel-wrapper {
  position: relative;
  max-width: 720px;
  margin: 0 auto;
}

.carousel-track {
  position: relative;
  width: 100%;
  overflow: hidden;
  border-radius: var(--radius-lg, 16px);
}

/* Slides */
.carousel-slide {
  display: none;
  flex-direction: column;
  background: var(--color-card);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg, 16px);
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(37,99,235,0.08);
  animation: slideIn 0.4s ease;
}
.carousel-slide.active {
  display: flex;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateX(30px); }
  to   { opacity: 1; transform: translateX(0); }
}

/* Image / PDF preview area */
.cert-image-wrap {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 11;
  overflow: hidden;
  background: #E8F0FE;
  cursor: pointer;
}
.cert-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.4s ease;
  pointer-events: none;
  user-select: none;
}
.cert-image-wrap:hover .cert-image {
  transform: scale(1.03);
}

/* PDF embed */
.cert-pdf {
  display: block;
  width: calc(100% + 18px);
  height: calc(100% + 18px);
  border: none;
  pointer-events: none;
}

/* Hover hint on image */
.cert-click-hint {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: #fff;
  font-size: 0.88rem;
  font-weight: 600;
  background: rgba(0, 0, 0, 0.45);
  backdrop-filter: blur(2px);
  opacity: 0;
  transition: opacity 0.25s ease;
  pointer-events: none;
}
.cert-image-wrap:hover .cert-click-hint {
  opacity: 1;
}

/* PDF fallback block */
.cert-pdf-fallback {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12px;
  color: var(--color-text-dim);
  font-size: 0.9rem;
}

.cert-image-overlay {
  position: absolute;
  top: 14px;
  left: 14px;
}

/* Badge */
.badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 999px;
  background: rgba(37,99,235,0.85);
  color: #fff;
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.04em;
  backdrop-filter: blur(4px);
}

/* Info section below image */
.cert-info {
  padding: 24px 28px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.cert-name {
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--color-text);
  line-height: 1.35;
  margin: 0;
}
.cert-issuer {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.88rem;
  color: var(--color-primary);
  margin: 0;
}
.cert-description {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  line-height: 1.65;
  margin: 0;
}
.cert-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 6px;
  padding-top: 14px;
  border-top: 1px solid var(--color-border);
}
.cert-year {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 0.82rem;
  color: var(--color-text-dim);
}
.cert-link-btn {
  background: none;
  border: none;
  padding: 0;
  font-family: var(--font-family);
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-primary);
  cursor: pointer;
  transition: var(--transition);
}
.cert-link-btn:hover {
  color: var(--color-accent);
  text-decoration: underline;
}

/* Navigation arrows */
.carousel-btn {
  position: absolute;
  top: calc(50% - 56px);
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1.5px solid var(--color-border);
  background: rgba(22, 33, 62, 0.85);
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: var(--transition);
  backdrop-filter: blur(6px);
  z-index: 10;
}
.carousel-btn:hover:not(:disabled) {
  background: var(--color-primary);
  border-color: var(--color-primary);
}
.carousel-btn:disabled {
  opacity: 0.3;
  cursor: default;
}
.carousel-btn.prev { left: -22px; }
.carousel-btn.next { right: -22px; }

/* Dot indicators */
.carousel-dots {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 20px;
}
.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: none;
  background: var(--color-border);
  cursor: pointer;
  transition: var(--transition);
  padding: 0;
}
.dot.active {
  background: var(--color-primary-light, #a29bfe);
  width: 24px;
  border-radius: 4px;
}

/* Counter */
.carousel-counter {
  text-align: center;
  font-size: 0.82rem;
  color: var(--color-text-dim);
  margin-top: 8px;
}

.lb-backdrop {
  position: fixed;
  inset: 0;
  z-index: 9999;
  background: rgba(0, 0, 0, 0.82);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.lb-box {
  background: var(--color-card);
  border: 1px solid var(--color-border);
  border-radius: 16px;
  overflow: hidden;
  width: 95vw;
  max-width: 1000px;
  height: 92vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 32px 80px rgba(37,99,235,0.12);
}

/* Header */
.lb-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 16px 20px;
  border-bottom: 1px solid var(--color-border);
  flex-shrink: 0;
}
.lb-title-wrap {
  display: flex;
  flex-direction: column;
  gap: 2px;
  min-width: 0;
}
.lb-title {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--color-text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.lb-issuer {
  font-size: 0.78rem;
  color: var(--color-primary);
}
.lb-close {
  flex-shrink: 0;
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  color: var(--color-text-muted);
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: var(--transition);
}
.lb-close:hover {
  background: rgba(239,68,68,0.1);
  border-color: rgba(239,68,68,0.4);
  color: #ef4444;
}

/* Content area */
.lb-content {
  flex: 1;
  min-height: 0;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  user-select: none;
  padding: 12px;
  background: #F0F6FF;
  box-sizing: border-box;
}
.lb-image {
  display: block;
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  object-fit: contain;
  pointer-events: none;
  -webkit-user-drag: none;
  border-radius: 6px;
  box-shadow: 0 8px 40px rgba(37,99,235,0.1);
}
.lb-pdf {
  width: 100%;
  height: 100%;
  min-height: 460px;
  border: none;
  display: block;
  border-radius: 6px;
}
.lb-pdf-fallback {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 14px;
  padding: 48px;
  color: var(--color-text-dim);
  text-align: center;
}

/* Transition */
.lb-enter-active,
.lb-leave-active {
  transition: opacity 0.2s ease;
}
.lb-enter-from,
.lb-leave-to {
  opacity: 0;
}
.lb-enter-active .lb-box,
.lb-leave-active .lb-box {
  transition: transform 0.2s ease, opacity 0.2s ease;
}
.lb-enter-from .lb-box,
.lb-leave-to .lb-box {
  transform: scale(0.94);
  opacity: 0;
}

@media (max-width: 640px) {
  .carousel-btn.prev { left: -12px; }
  .carousel-btn.next { right: -12px; }
  .cert-info { padding: 18px 20px; }
  .cert-name { font-size: 1.05rem; }
  .lb-box { width: 98vw; height: 92vh; border-radius: 12px; }
  .lb-pdf { min-height: 280px; }
}
</style>
