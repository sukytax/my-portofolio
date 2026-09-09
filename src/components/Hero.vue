<template>
  <section id="hero" class="hero-section">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>

    <div class="hero-inner">
      <!-- Photo -->
      <div class="hero-photo-wrap reveal" ref="photoEl">
        <div class="hero-photo-ring">
          <img :src="profile.photo" :alt="profile.name" class="hero-photo" />
        </div>
      </div>

      <!-- Text -->
      <div class="hero-text reveal" ref="textEl">
        <span class="hero-greeting">Perkenalkan, Saya</span>
        <h1 class="hero-name">{{ profile.name }}</h1>
        <p class="hero-title gradient-text">{{ profile.title }}</p>

        <!-- Tab buttons -->
        <div class="tab-btns">
          <button
            class="tab-btn"
            :class="{ active: activeTab === 'about' }"
            @click="activeTab = 'about'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"/>
              <line x1="12" y1="16" x2="12" y2="12"/>
              <line x1="12" y1="8" x2="12.01" y2="8"/>
            </svg>
            About Me
          </button>
          <button
            class="tab-btn"
            :class="{ active: activeTab === 'education' }"
            @click="activeTab = 'education'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
              <path d="M6 12v5c3 3 9 3 12 0v-5"/>
            </svg>
            Education
          </button>
        </div>

        <!-- Tab panel -->
        <Transition name="tab-fade" mode="out-in">
          <!-- About Me -->
          <div v-if="activeTab === 'about'" key="about" class="tab-panel">
            <p v-for="(para, i) in profile.about" :key="i" class="tab-para">{{ para }}</p>
          </div>

          <!-- Education -->
          <div v-else key="education" class="tab-panel">
            <div v-for="(edu, i) in profile.education" :key="i" class="edu-item">
              <div class="edu-dot"></div>
              <div class="edu-body">
                <div class="edu-top">
                  <div>
                    <p class="edu-degree">{{ edu.degree }}</p>
                    <p class="edu-school">{{ edu.school }}</p>
                  </div>
                  <div class="edu-right">
                    <span class="edu-period">{{ edu.period }}</span>
                    <span v-if="edu.gpa" class="edu-gpa">GPA {{ edu.gpa }}</span>
                  </div>
                </div>
                <p class="edu-loc">
                  <svg xmlns="http://www.w3.org/2000/svg" width="11" height="11" viewBox="0 0 24 24"
                    fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
                  </svg>
                  {{ edu.location }}
                </p>
                <p class="edu-desc">{{ edu.description }}</p>
              </div>
            </div>
          </div>
        </Transition>

        <!-- CV button -->
        <div class="hero-cv-wrap">
          <a :href="profile.cv" class="btn-primary">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
              <polyline points="7 10 12 15 17 10"/>
              <line x1="12" y1="15" x2="12" y2="3"/>
            </svg>
            Download CV
          </a>
        </div>

        <!-- Social links -->
        <div class="hero-socials">
          <a :href="`https://mail.google.com/mail/?view=cm&to=${profile.socials.email}`" target="_blank" rel="noopener noreferrer" title="Email" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" fill="none" viewBox="0 0 24 24"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="4" width="20" height="16" rx="2"/>
              <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
            </svg>
          </a>
          <a :href="profile.socials.linkedin" target="_blank" title="LinkedIn" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
              <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
              <rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/>
            </svg>
          </a>
          <a :href="profile.socials.github" target="_blank" title="GitHub" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
              <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35
                6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16
                2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5
                4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9
                18.13V22"/>
            </svg>
          </a>
          <a :href="profile.socials.instagram" target="_blank" title="Instagram" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none"
              stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
              <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/>
              <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/>
            </svg>
          </a>
        </div>
      </div>
    </div>

    <!-- Scroll indicator -->
    <div class="scroll-indicator">
      <span>Scroll ke bawah</span>
      <div class="scroll-mouse">
        <div class="scroll-wheel"></div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({ profile: { type: Object, required: true } });

const photoEl  = ref(null);
const textEl   = ref(null);
const activeTab = ref('about');

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); }),
    { threshold: 0.1 }
  );
  [photoEl.value, textEl.value].forEach(el => el && observer.observe(el));
});
</script>

<style scoped>
.hero-section {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120px 24px 80px;
  overflow: hidden;
}

.blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
  animation: blobFloat 8s ease-in-out infinite alternate;
}
.blob-1 {
  width: 500px; height: 500px;
  background: radial-gradient(circle, rgba(37,99,235,0.1) 0%, transparent 70%);
  top: -100px; right: -100px;
}
.blob-2 {
  width: 400px; height: 400px;
  background: radial-gradient(circle, rgba(14,165,233,0.08) 0%, transparent 70%);
  bottom: -80px; left: -80px;
  animation-delay: -4s;
}
@keyframes blobFloat {
  from { transform: translate(0, 0) scale(1); }
  to   { transform: translate(30px, 20px) scale(1.05); }
}

.hero-inner {
  display: flex;
  align-items: center;
  gap: 72px;
  max-width: 1100px;
  width: 100%;
  z-index: 1;
}

/* Photo */
.hero-photo-wrap { flex-shrink: 0; }
.hero-photo-ring {
  width: 240px; height: 240px;
  border-radius: 50%;
  padding: 4px;
  background: linear-gradient(135deg, var(--color-primary), var(--color-accent));
  box-shadow: 0 0 40px rgba(37,99,235,0.2);
}
.hero-photo {
  width: 100%; height: 100%;
  border-radius: 50%;
  object-fit: cover;
  background: var(--color-card);
}

/* Text column */
.hero-text {
  display: flex;
  flex-direction: column;
  gap: 12px;
  flex: 1;
}
.hero-greeting { font-size: 1rem; color: var(--color-text-muted); font-weight: 500; }
.hero-name {
  font-size: clamp(2.2rem, 5vw, 3.2rem);
  font-weight: 800;
  color: var(--color-text);
  line-height: 1.1;
}
.hero-title { font-size: 1rem; font-weight: 600; }

/* Tab buttons */
.tab-btns {
  display: flex;
  gap: 8px;
  margin-top: 4px;
}
.tab-btn {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 7px 16px;
  border-radius: 999px;
  border: 1.5px solid var(--color-border);
  background: transparent;
  color: var(--color-text-muted);
  font-family: var(--font-family);
  font-size: 0.83rem;
  font-weight: 600;
  cursor: pointer;
  transition: var(--transition);
}
.tab-btn:hover {
  border-color: var(--color-primary);
  color: var(--color-primary);
}
.tab-btn.active {
  background: var(--color-primary);
  border-color: var(--color-primary);
  color: #fff;
}

/* Tab panel */
.tab-panel {
  background: var(--color-card);
  border: 1px solid var(--color-border);
  border-radius: 14px;
  padding: 18px 20px;
  max-width: 520px;
  box-shadow: 0 2px 12px rgba(37,99,235,0.06);
}

/* About Me paragraphs */
.tab-para {
  font-size: 0.88rem;
  color: var(--color-text-muted);
  line-height: 1.75;
  margin-bottom: 10px;
}
.tab-para:last-child { margin-bottom: 0; }

/* Education items */
.edu-item {
  display: flex;
  gap: 14px;
  padding: 14px 0;
  border-bottom: 1px solid var(--color-border);
}
.edu-item:first-child { padding-top: 0; }
.edu-item:last-child { border-bottom: none; padding-bottom: 0; }
.edu-dot {
  width: 10px; height: 10px;
  border-radius: 50%;
  background: var(--color-primary);
  flex-shrink: 0;
  margin-top: 5px;
  box-shadow: 0 0 0 3px rgba(37,99,235,0.12);
}
.edu-body { flex: 1; }
.edu-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 8px;
  margin-bottom: 4px;
}
.edu-degree {
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 0;
}
.edu-school {
  font-size: 0.82rem;
  color: var(--color-primary);
  font-weight: 600;
  margin: 0;
}
.edu-right {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 3px;
  flex-shrink: 0;
}
.edu-period {
  font-size: 0.75rem;
  color: var(--color-text-dim);
  white-space: nowrap;
}
.edu-gpa {
  font-size: 0.72rem;
  font-weight: 700;
  padding: 2px 8px;
  border-radius: 999px;
  background: rgba(37,99,235,0.1);
  color: var(--color-primary);
}
.edu-loc {
  display: flex;
  align-items: center;
  gap: 3px;
  font-size: 0.75rem;
  color: var(--color-text-dim);
  margin-bottom: 5px;
}
.edu-desc {
  font-size: 0.83rem;
  color: var(--color-text-muted);
  line-height: 1.6;
  margin: 0;
}

/* CV button */
.hero-cv-wrap { margin-top: 4px; }

/* Social icons */
.hero-socials { display: flex; gap: 10px; }
.social-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px; height: 36px;
  border-radius: 50%;
  background: var(--color-card);
  border: 1px solid var(--color-border);
  color: var(--color-text-muted);
  transition: var(--transition);
}
.social-icon:hover {
  color: var(--color-primary);
  border-color: var(--color-primary);
  background: rgba(37,99,235,0.08);
  transform: translateY(-2px);
}

/* Scroll indicator */
.scroll-indicator {
  position: absolute;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  color: var(--color-text-dim);
  font-size: 0.78rem;
  letter-spacing: 0.05em;
  animation: fadeInUp 2s ease 1.5s both;
}
@keyframes fadeInUp {
  from { opacity: 0; transform: translateX(-50%) translateY(12px); }
  to   { opacity: 1; transform: translateX(-50%) translateY(0); }
}
.scroll-mouse { width: 22px; height: 34px; border: 2px solid var(--color-text-dim); border-radius: 12px; display: flex; justify-content: center; padding-top: 5px; }
.scroll-wheel { width: 3px; height: 7px; background: var(--color-text-dim); border-radius: 2px; animation: scrollWheel 1.8s ease infinite; }
@keyframes scrollWheel {
  0%   { transform: translateY(0); opacity: 1; }
  100% { transform: translateY(10px); opacity: 0; }
}

/* Tab transition */
.tab-fade-enter-active,
.tab-fade-leave-active { transition: opacity 0.18s ease, transform 0.18s ease; }
.tab-fade-enter-from { opacity: 0; transform: translateY(6px); }
.tab-fade-leave-to  { opacity: 0; transform: translateY(-4px); }

@media (max-width: 900px) {
  .hero-inner { flex-direction: column; text-align: center; align-items: center; gap: 36px; }
  .hero-photo-ring { width: 180px; height: 180px; }
  .tab-btns, .hero-socials, .hero-cv-wrap { justify-content: center; display: flex; }
  .tab-panel { max-width: 100%; }
  .edu-top { text-align: left; }
}
@media (max-width: 560px) {
  .hero-section { padding: 100px 20px 90px; }
  .scroll-indicator { display: none; }
}
</style>
