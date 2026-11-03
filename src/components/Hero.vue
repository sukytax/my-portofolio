<template>
  <section id="hero" class="hero-section">
    <div class="hero-grid-bg" aria-hidden="true"></div>
    <div class="ambient-glow glow-1" aria-hidden="true"></div>
    <div class="ambient-glow glow-2" aria-hidden="true"></div>
    <div class="ambient-glow glow-3" aria-hidden="true"></div>

    <div class="hero-container">
      <!-- LEFT COLUMN: Content, About Me & Education -->
      <div class="hero-content-col reveal" ref="contentEl">
        <!-- Name & Titles -->
        <div class="hero-heading-group">
          <span class="hero-greeting">Halo, Saya</span>
          <h1 class="hero-name">
            {{ profile.name }}
          </h1>
          <div class="hero-title-badge">
            <span class="hero-title-text">{{ profile.title }}</span>
          </div>
        </div>

        <!-- Segmented Tab Navigation -->
        <div class="tab-control-wrap">
          <div class="tab-control-bar" role="tablist">
            <button
              class="tab-pill"
              :class="{ active: activeTab === 'about' }"
              @click="activeTab = 'about'"
              role="tab"
              :aria-selected="activeTab === 'about'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="8" r="5"/>
                <path d="M20 21a8 8 0 0 0-16 0"/>
              </svg>
              <span>Tentang Saya</span>
            </button>

            <button
              class="tab-pill"
              :class="{ active: activeTab === 'education' }"
              @click="activeTab = 'education'"
              role="tab"
              :aria-selected="activeTab === 'education'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
                <path d="M6 12v5c3 3 9 3 12 0v-5"/>
              </svg>
              <span>Pendidikan</span>
            </button>
          </div>
        </div>

        <!-- Dynamic Content Card Panels -->
        <div class="content-panel-wrapper">
          <Transition name="tab-slide" mode="out-in">
            <!-- TAB: ABOUT -->
            <div v-if="activeTab === 'about'" key="tab-about" class="info-card">
              <div class="card-inner-section">
                <div class="card-section-header">
                  <h3 class="panel-title">Mengenal Bimo</h3>
                </div>
                <div class="about-paragraphs">
                  <p v-for="(para, idx) in profile.about" :key="idx" class="about-text">
                    {{ para }}
                  </p>
                </div>

                <!-- Focus Badges -->
                <div class="skill-tags">
                  <span class="skill-tag">Python & SQL</span>
                  <span class="skill-tag">Data Science & ML</span>
                  <span class="skill-tag">Data Analytics</span>
                  <span class="skill-tag">IT Developer</span>
                </div>

                <!-- Quick Education Teaser -->
                <div class="edu-teaser" @click="activeTab = 'education'">
                  <div class="edu-teaser-left">
                    <span class="teaser-icon">🎓</span>
                    <div>
                      <strong class="teaser-title">S1 Teknik Informatika • Univ. Mercu Buana</strong>
                      <span class="teaser-sub">IPK 3.90 / 4.00 • 2022 – Sekarang</span>
                    </div>
                  </div>
                  <span class="teaser-arrow">Detail →</span>
                </div>
              </div>
            </div>

            <!-- TAB: EDUCATION -->
            <div v-else key="tab-edu" class="info-card">
              <div class="card-inner-section">
                <div class="card-section-header">
                  <h3 class="panel-title">Riwayat Pendidikan</h3>
                </div>

                <div class="edu-timeline">
                  <div v-for="(edu, i) in profile.education" :key="i" class="timeline-item">
                    <div class="timeline-marker">
                      <div class="marker-dot"></div>
                      <div v-if="i < profile.education.length - 1" class="marker-line"></div>
                    </div>
                    <div class="timeline-card">
                      <div class="timeline-header">
                        <div>
                          <h4 class="edu-name">{{ edu.degree }}</h4>
                          <p class="edu-institution">{{ edu.school }}</p>
                        </div>
                        <div class="timeline-meta">
                          <span v-if="edu.gpa" class="gpa-pill">IPK {{ edu.gpa }}</span>
                          <span class="period-pill">{{ edu.period }}</span>
                        </div>
                      </div>
                      <div class="edu-meta-row">
                        <span class="edu-location">
                          <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                            <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
                          </svg>
                          {{ edu.location }}
                        </span>
                      </div>
                      <p class="edu-detail">{{ edu.description }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </Transition>
        </div>

        <!-- Action Buttons & Social Connections -->
        <div class="hero-actions-group">
          <div class="cta-buttons">
            <a :href="profile.cv" download class="btn-cv">
              <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                <polyline points="7 10 12 15 17 10"/>
                <line x1="12" y1="15" x2="12" y2="3"/>
              </svg>
              <span>Download CV</span>
            </a>

            <a :href="`https://mail.google.com/mail/?view=cm&to=${profile.socials.email}`" target="_blank" rel="noopener noreferrer" class="btn-contact">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="4" width="20" height="16" rx="2"/>
                <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
              </svg>
              <span>Hubungi Saya</span>
            </a>
          </div>

          <!-- Social Link Icons -->
          <div class="social-bar">
            <a
              :href="`https://mail.google.com/mail/?view=cm&to=${profile.socials.email}`"
              target="_blank"
              rel="noopener noreferrer"
              class="social-pill"
              title="Email"
              aria-label="Email"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="4" width="20" height="16" rx="2"/>
                <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
              </svg>
            </a>
            <a
              :href="profile.socials.linkedin"
              target="_blank"
              rel="noopener noreferrer"
              class="social-pill"
              title="LinkedIn"
              aria-label="LinkedIn"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24" fill="currentColor">
                <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
                <rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/>
              </svg>
            </a>
            <a
              :href="profile.socials.github"
              target="_blank"
              rel="noopener noreferrer"
              class="social-pill"
              title="GitHub"
              aria-label="GitHub"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24" fill="currentColor">
                <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/>
              </svg>
            </a>
            <a
              :href="profile.socials.instagram"
              target="_blank"
              rel="noopener noreferrer"
              class="social-pill"
              title="Instagram"
              aria-label="Instagram"
            >
              <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
                <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/>
                <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/>
              </svg>
            </a>
          </div>
        </div>
      </div>

      <!-- RIGHT COLUMN: Large Premium Photo Presentation -->
      <div class="hero-photo-col reveal" ref="photoEl">
        <div class="photo-card-container">
          <!-- Ambient Backlight -->
          <div class="photo-glow-aura"></div>

          <!-- Decorative Outer Rim / Offset Frame -->
          <div class="photo-outer-frame">
            <!-- Main Portrait Image -->
            <div class="photo-image-wrapper">
              <img
                :src="profile.photo"
                :alt="profile.name"
                class="hero-large-photo"
                loading="eager"
              />
              <div class="photo-overlay-gradient"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  profile: {
    type: Object,
    required: true,
  },
});

const contentEl = ref(null);
const photoEl = ref(null);
const activeTab = ref('about'); // 'about' | 'education'

onMounted(() => {
  // Ensure hero is visible immediately without delay
  setTimeout(() => {
    if (contentEl.value) contentEl.value.classList.add('visible');
    if (photoEl.value) photoEl.value.classList.add('visible');
  }, 50);

  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((e) => {
        if (e.isIntersecting) {
          e.target.classList.add('visible');
        }
      });
    },
    { threshold: 0.05 }
  );

  if (contentEl.value) observer.observe(contentEl.value);
  if (photoEl.value) observer.observe(photoEl.value);
});
</script>

<style scoped>

.hero-section {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 120px 24px 70px;
  overflow: hidden;
  background-color: var(--color-bg);
}

/* Subtle modern dot-grid background pattern */
.hero-grid-bg {
  position: absolute;
  inset: 0;
  background-image: radial-gradient(rgba(37, 99, 235, 0.08) 1.2px, transparent 1.2px);
  background-size: 28px 28px;
  mask-image: radial-gradient(ellipse at 50% 40%, black 40%, transparent 80%);
  -webkit-mask-image: radial-gradient(ellipse at 50% 40%, black 40%, transparent 80%);
  pointer-events: none;
  z-index: 0;
}

/* Ambient dynamic light orbs */
.ambient-glow {
  position: absolute;
  border-radius: 50%;
  filter: blur(100px);
  pointer-events: none;
  z-index: 0;
}
.glow-1 {
  width: 520px;
  height: 520px;
  background: radial-gradient(circle, rgba(37, 99, 235, 0.15) 0%, transparent 70%);
  top: 5%;
  right: -5%;
  animation: floatAmbient 10s ease-in-out infinite alternate;
}
.glow-2 {
  width: 440px;
  height: 440px;
  background: radial-gradient(circle, rgba(14, 165, 233, 0.12) 0%, transparent 70%);
  bottom: 0%;
  left: -5%;
  animation: floatAmbient 12s ease-in-out infinite alternate-reverse;
}
.glow-3 {
  width: 320px;
  height: 320px;
  background: radial-gradient(circle, rgba(99, 102, 241, 0.08) 0%, transparent 70%);
  top: 30%;
  left: 35%;
  animation: floatAmbient 14s ease-in-out infinite alternate;
}

@keyframes floatAmbient {
  0% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(25px, -20px) scale(1.05); }
  100% { transform: translate(-20px, 20px) scale(0.95); }
}

/* Main Hero Grid Container */
.hero-container {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  width: 100%;
  display: grid;
  grid-template-columns: 1.15fr 0.95fr;
  align-items: center;
  gap: 56px;
  margin: auto 0;
}

.hero-content-col {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 620px;
}

/* Heading Group */
.hero-heading-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.hero-greeting {
  font-size: 1.05rem;
  font-weight: 500;
  color: var(--color-text-muted);
  letter-spacing: 0.03em;
  text-transform: uppercase;
}

.hero-name {
  font-size: clamp(2.4rem, 4.2vw, 3.4rem);
  font-weight: 800;
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: var(--color-text);
}

.hero-title-badge {
  display: inline-block;
}

.hero-title-text {
  font-size: 1.25rem;
  font-weight: 700;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-accent) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.tab-control-wrap {
  margin-top: 4px;
}

.tab-control-bar {
  display: inline-flex;
  align-items: center;
  padding: 4px;
  background: rgba(241, 245, 249, 0.85);
  border: 1px solid rgba(226, 232, 240, 0.9);
  border-radius: 999px;
  backdrop-filter: blur(12px);
  gap: 4px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.04);
}

.tab-pill {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  padding: 8px 18px;
  border-radius: 999px;
  border: none;
  background: transparent;
  color: var(--color-text-muted);
  font-family: var(--font-family);
  font-size: 0.85rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.22s cubic-bezier(0.16, 1, 0.3, 1);
  white-space: nowrap;
}

.tab-pill svg {
  transition: transform 0.2s ease;
}

.tab-pill:hover {
  color: var(--color-primary);
}

.tab-pill:hover svg {
  transform: translateY(-1px);
}

.tab-pill.active {
  background: #FFFFFF;
  color: var(--color-primary);
  box-shadow: 0 2px 10px rgba(37, 99, 235, 0.12), 0 1px 2px rgba(0, 0, 0, 0.05);
}

.content-panel-wrapper {
  position: relative;
  min-height: 220px;
}

.info-card {
  background: #FFFFFF;
  border: 1px solid rgba(226, 232, 240, 0.95);
  border-radius: 20px;
  padding: 22px 24px;
  box-shadow: 0 6px 24px rgba(37, 99, 235, 0.05), 0 1px 3px rgba(0, 0, 0, 0.02);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.info-card:hover {
  border-color: rgba(37, 99, 235, 0.25);
  box-shadow: 0 10px 30px rgba(37, 99, 235, 0.08);
}

.card-inner-section {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.card-section-header {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.panel-title {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 0;
}

/* About Paragraphs */
.about-paragraphs {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.about-text {
  font-size: 0.91rem;
  color: var(--color-text-muted);
  line-height: 1.7;
  margin: 0;
}

/* Skill Tags */
.skill-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  margin-top: 2px;
}

.skill-tag {
  font-size: 0.75rem;
  font-weight: 600;
  padding: 4px 10px;
  border-radius: 6px;
  background: rgba(37, 99, 235, 0.06);
  color: var(--color-primary-dark);
  border: 1px solid rgba(37, 99, 235, 0.12);
}

/* Education Teaser Strip inside About */
.edu-teaser {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 14px;
  background: rgba(248, 250, 252, 0.9);
  border: 1px dashed rgba(37, 99, 235, 0.25);
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-top: 4px;
}

.edu-teaser:hover {
  background: rgba(238, 242, 255, 0.7);
  border-color: var(--color-primary);
  transform: translateX(2px);
}

.edu-teaser-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.teaser-icon {
  font-size: 1.25rem;
}

.teaser-title {
  display: block;
  font-size: 0.83rem;
  color: var(--color-text);
  font-weight: 700;
}

.teaser-sub {
  display: block;
  font-size: 0.74rem;
  color: var(--color-text-muted);
}

.teaser-arrow {
  font-size: 0.78rem;
  font-weight: 700;
  color: var(--color-primary);
}

.edu-timeline {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-top: 4px;
}

.timeline-item {
  display: flex;
  gap: 14px;
}

.timeline-marker {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-shrink: 0;
  padding-top: 4px;
}

.marker-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.2);
}

.marker-line {
  width: 2px;
  flex: 1;
  background: linear-gradient(180deg, rgba(37, 99, 235, 0.3) 0%, rgba(226, 232, 240, 0.8) 100%);
  margin-top: 6px;
  min-height: 40px;
}

.timeline-card {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.timeline-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 10px;
  flex-wrap: wrap;
}

.edu-name {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 0;
}

.edu-institution {
  font-size: 0.84rem;
  font-weight: 600;
  color: var(--color-primary);
  margin: 0;
}

.timeline-meta {
  display: flex;
  align-items: center;
  gap: 6px;
  flex-wrap: wrap;
}

.gpa-pill {
  font-size: 0.74rem;
  font-weight: 700;
  color: #047857;
  background: #ECFDF5;
  border: 1px solid rgba(16, 185, 129, 0.3);
  padding: 2px 8px;
  border-radius: 999px;
}

.period-pill {
  font-size: 0.74rem;
  font-weight: 600;
  color: var(--color-text-dim);
  background: #F1F5F9;
  padding: 2px 8px;
  border-radius: 999px;
}

.edu-meta-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 2px 0;
}

.edu-location {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 0.75rem;
  color: var(--color-text-dim);
}

.edu-detail {
  font-size: 0.83rem;
  color: var(--color-text-muted);
  line-height: 1.6;
  margin: 2px 0 0;
}

.hero-actions-group {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  flex-wrap: wrap;
  padding-top: 4px;
}

.cta-buttons {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
}

.btn-cv {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 11px 22px;
  background: linear-gradient(135deg, var(--color-primary) 0%, var(--color-primary-dark) 100%);
  color: #FFFFFF;
  border-radius: 999px;
  font-size: 0.9rem;
  font-weight: 600;
  box-shadow: 0 4px 16px rgba(37, 99, 235, 0.28);
  transition: all 0.25s ease;
}

.btn-cv:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 22px rgba(37, 99, 235, 0.4);
  color: #FFFFFF;
}

.btn-contact {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 10px 20px;
  background: #FFFFFF;
  color: var(--color-text);
  border: 1px solid rgba(226, 232, 240, 0.95);
  border-radius: 999px;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.25s ease;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
}

.btn-contact:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
  background: rgba(37, 99, 235, 0.04);
  transform: translateY(-2px);
}

.social-bar {
  display: flex;
  align-items: center;
  gap: 8px;
}

.social-pill {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: #FFFFFF;
  border: 1px solid rgba(226, 232, 240, 0.95);
  color: var(--color-text-muted);
  transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
}

.social-pill:hover {
  color: var(--color-primary);
  border-color: var(--color-primary);
  background: rgba(37, 99, 235, 0.08);
  transform: translateY(-2px) scale(1.05);
}

.hero-photo-col {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.photo-card-container {
  position: relative;
  width: 100%;
  max-width: 440px;
}

/* Backlight Aura */
.photo-glow-aura {
  position: absolute;
  inset: -15px;
  background: radial-gradient(circle at 60% 40%, rgba(37, 99, 235, 0.25) 0%, rgba(14, 165, 233, 0.15) 50%, transparent 75%);
  filter: blur(40px);
  border-radius: 40px;
  z-index: 0;
  pointer-events: none;
}

/* Outer frame with subtle border and floating elements */
.photo-outer-frame {
  position: relative;
  z-index: 1;
  background: #FFFFFF;
  padding: 10px;
  border-radius: 32px;
  border: 1px solid rgba(255, 255, 255, 0.8);
  box-shadow: 0 20px 48px -12px rgba(37, 99, 235, 0.16), 0 1px 3px rgba(0, 0, 0, 0.04);
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), box-shadow 0.4s ease;
}

.photo-outer-frame:hover {
  transform: translateY(-4px);
  box-shadow: 0 28px 56px -12px rgba(37, 99, 235, 0.22), 0 1px 4px rgba(0, 0, 0, 0.06);
}

/* Image wrapper */
.photo-image-wrapper {
  position: relative;
  width: 100%;
  aspect-ratio: 3.8 / 4.8;
  max-height: 530px;
  border-radius: 24px;
  overflow: hidden;
  background: #E2E8F0;
}

.hero-large-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 18%;
  display: block;
  transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.photo-outer-frame:hover .hero-large-photo {
  transform: scale(1.025);
}

.photo-overlay-gradient {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(15, 23, 42, 0) 65%, rgba(15, 23, 42, 0.35) 100%);
  pointer-events: none;
}


.tab-slide-enter-active,
.tab-slide-leave-active {
  transition: opacity 0.22s ease, transform 0.22s ease;
}

.tab-slide-enter-from {
  opacity: 0;
  transform: translateY(8px);
}

.tab-slide-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}


@media (max-width: 1080px) {
  .hero-container {
    gap: 40px;
  }
}

@media (max-width: 960px) {
  .hero-section {
    padding: 110px 20px 80px;
  }
  .hero-container {
    grid-template-columns: 1fr;
    gap: 48px;
    text-align: center;
  }
  .hero-content-col {
    max-width: 100%;
    align-items: center;
    order: 2;
  }
  .hero-photo-col {
    order: 1;
  }
  .tab-control-wrap {
    display: flex;
    justify-content: center;
  }
  .card-section-header {
    text-align: left;
  }
  .about-paragraphs {
    text-align: left;
  }
  .edu-teaser {
    text-align: left;
  }
  .timeline-card {
    text-align: left;
  }
  .hero-actions-group {
    justify-content: center;
  }
  .photo-card-container {
    max-width: 360px;
  }
}

@media (max-width: 540px) {
  .hero-section {
    padding: 95px 16px 65px;
  }
  .hero-name {
    font-size: 2.2rem;
  }
  .hero-title-text {
    font-size: 1.1rem;
  }
  .tab-control-bar {
    width: 100%;
    justify-content: space-between;
  }
  .tab-pill {
    padding: 7px 12px;
    font-size: 0.78rem;
    flex: 1;
    justify-content: center;
  }
  .info-card {
    padding: 18px 16px;
  }
  .cta-buttons {
    width: 100%;
    flex-direction: column;
  }
  .btn-cv, .btn-contact {
    width: 100%;
    justify-content: center;
  }
}
</style>
