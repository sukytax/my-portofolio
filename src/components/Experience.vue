<template>
  <section id="experience">
    <div class="section">
      <h2 class="section-title reveal" ref="titleEl">Pengalaman</h2>
      <p class="section-subtitle reveal" ref="subtitleEl">
        Perjalanan karir dan kontribusi yang telah dijalani
      </p>

      <div class="exp-timeline">
        <div
          v-for="(exp, idx) in experiences"
          :key="exp.id"
          class="exp-item reveal"
          :ref="el => expRefs.push(el)"
          :style="{ '--accent': exp.color }"
        >
          <!-- Timeline line & dot -->
          <div class="timeline-aside">
            <div class="timeline-dot" :style="{ background: exp.color, boxShadow: `0 0 12px ${exp.color}55` }"></div>
            <div class="timeline-line" v-if="idx < experiences.length - 1"></div>
          </div>

          <!-- Card -->
          <div class="exp-card card" @click="openModal(exp)">
            <div class="exp-header">
              <div>
                <span class="badge" :style="{ background: `${exp.color}22`, color: exp.color, borderColor: `${exp.color}44` }">
                  {{ exp.type }}
                </span>
                <h3 class="exp-role">{{ exp.role }}</h3>
                <p class="exp-company">{{ exp.company }}</p>
              </div>
              <div class="exp-meta">
                <span class="exp-period">
                  <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24"
                    fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/>
                  </svg>
                  {{ exp.period }}
                </span>
                <span class="exp-period">
                  <svg xmlns="http://www.w3.org/2000/svg" width="13" height="13" viewBox="0 0 24 24"
                    fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
                    <circle cx="12" cy="10" r="3"/>
                  </svg>
                  {{ exp.location }}
                </span>
              </div>
            </div>

            <p class="exp-summary">{{ exp.summary }}</p>

            <!-- Tech badges -->
            <div class="exp-tech">
              <span v-for="t in exp.tech" :key="t" class="tech-tag">{{ t }}</span>
            </div>

            <div class="exp-cta">
              <span>Lihat Detail</span>
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                <path d="m9 18 6-6-6-6"/>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <ExperienceModal v-if="selectedExp" :experience="selectedExp" @close="selectedExp = null" />
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ExperienceModal from './ExperienceModal.vue';

const props = defineProps({ experiences: { type: Array, required: true } });

const titleEl    = ref(null);
const subtitleEl = ref(null);
const expRefs    = [];
const selectedExp = ref(null);

function openModal(exp) { selectedExp.value = exp; }

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); }),
    { threshold: 0.1 }
  );
  [titleEl.value, subtitleEl.value, ...expRefs]
    .filter(Boolean)
    .forEach((el, i) => {
      el.style.transitionDelay = `${i * 0.1}s`;
      observer.observe(el);
    });
});
</script>

<style scoped>
/* Timeline Layout */
.exp-timeline {
  display: flex;
  flex-direction: column;
  gap: 0;
}
.exp-item {
  display: flex;
  gap: 28px;
  align-items: flex-start;
}

/* Timeline Aside */
.timeline-aside {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 28px;
  flex-shrink: 0;
}
.timeline-dot {
  width: 14px; height: 14px;
  border-radius: 50%;
  border: 2px solid var(--color-bg);
  flex-shrink: 0;
  transition: var(--transition);
}
.exp-card:hover ~ .timeline-aside .timeline-dot,
.exp-item:hover .timeline-dot {
  transform: scale(1.3);
}
.timeline-line {
  width: 2px;
  flex: 1;
  min-height: 40px;
  background: linear-gradient(to bottom, var(--accent, var(--color-primary)) 0%, var(--color-border) 100%);
  margin: 8px 0;
  opacity: 0.4;
}

/* Card */
.exp-card {
  flex: 1;
  padding: 28px;
  cursor: pointer;
  margin-bottom: 28px;
}
.exp-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  margin-bottom: 14px;
}
.exp-role {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--color-text);
  margin: 8px 0 4px;
}
.exp-company {
  font-size: 0.9rem;
  color: var(--color-text-muted);
}
.exp-meta {
  display: flex;
  flex-direction: column;
  gap: 6px;
  text-align: right;
  flex-shrink: 0;
}
.exp-period {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 0.8rem;
  color: var(--color-text-dim);
  justify-content: flex-end;
}
.exp-summary {
  font-size: 0.92rem;
  color: var(--color-text-muted);
  line-height: 1.7;
  margin-bottom: 16px;
}
.exp-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
  margin-bottom: 18px;
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
.exp-cta {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-primary);
  padding-top: 14px;
  border-top: 1px solid var(--color-border);
  transition: var(--transition);
}
.exp-card:hover .exp-cta { color: var(--color-primary-dark); gap: 10px; }

@media (max-width: 640px) {
  .timeline-aside { display: none; }
  .exp-header {
    flex-direction: column;
    gap: 8px;
  }
  .exp-meta {
    text-align: left;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 10px;
  }
  .exp-period { justify-content: flex-start; }
}
</style>
