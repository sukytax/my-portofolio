<template>
  <Teleport to="body">
    <div class="modal-overlay" @click.self="$emit('close')">
      <div class="modal-box" :style="{ '--accent': experience.color }">
        <!-- Close button -->
        <button class="modal-close" @click="$emit('close')" aria-label="Close">
          <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24"
            fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/>
          </svg>
        </button>

        <!-- Header badge -->
        <span class="badge" :style="{ background: `${experience.color}22`, color: experience.color, borderColor: `${experience.color}44` }">
          {{ experience.type }}
        </span>

        <!-- Role & Company -->
        <h2 class="modal-role">{{ experience.role }}</h2>
        <p class="modal-company">
          <strong>{{ experience.company }}</strong>
        </p>

        <!-- Meta -->
        <div class="modal-meta">
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/>
            </svg>
            {{ experience.period }}
          </span>
          <span>
            <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/>
              <circle cx="12" cy="10" r="3"/>
            </svg>
            {{ experience.location }}
          </span>
        </div>

        <!-- Divider -->
        <div class="modal-divider" :style="{ background: `linear-gradient(90deg, ${experience.color}, transparent)` }"></div>

        <!-- Description -->
        <div class="modal-description">
          <p v-for="(line, i) in descLines" :key="i" :class="line.startsWith('•') ? 'bullet' : 'paragraph'">
            {{ line }}
          </p>
        </div>

        <!-- Tech Stack -->
        <div class="modal-tech-section">
          <p class="modal-tech-label">SKILLS</p>
          <div class="modal-tech">
            <span
              v-for="t in experience.tech"
              :key="t"
              class="tech-chip"
              :style="{ borderColor: `${experience.color}44`, color: experience.color }"
            >{{ t }}</span>
          </div>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<script setup>
import { computed, onMounted, onUnmounted } from 'vue';

const props = defineProps({ experience: { type: Object, required: true } });
const emit  = defineEmits(['close']);

const descLines = computed(() =>
  props.experience.description
    .split('\n')
    .map(l => l.trim())
    .filter(Boolean)
);

function onKeyDown(e) { if (e.key === 'Escape') emit('close'); }
onMounted(() => {
  document.addEventListener('keydown', onKeyDown);
  document.body.style.overflow = 'hidden';
});
onUnmounted(() => {
  document.removeEventListener('keydown', onKeyDown);
  document.body.style.overflow = '';
});
</script>

<style scoped>
.modal-close {
  position: absolute;
  top: 20px;
  right: 20px;
  background: var(--color-bg);
  border: 1px solid var(--color-border);
  border-radius: 50%;
  width: 38px; height: 38px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-text-muted);
  cursor: pointer;
  transition: var(--transition);
}
.modal-close:hover {
  background: rgba(239,68,68,0.08);
  border-color: rgba(239,68,68,0.4);
  color: #ef4444;
  transform: rotate(90deg);
}

.modal-role {
  font-size: 1.6rem;
  font-weight: 800;
  color: var(--color-text);
  margin: 14px 0 6px;
  line-height: 1.2;
}
.modal-company {
  font-size: 1rem;
  color: var(--color-text-muted);
  margin-bottom: 14px;
}

.modal-meta {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  margin-bottom: 20px;
}
.modal-meta span {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.85rem;
  color: var(--color-text-muted);
}

.modal-divider {
  height: 2px;
  border-radius: 2px;
  margin-bottom: 24px;
  opacity: 0.5;
}

.modal-description {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 28px;
}
.modal-description .paragraph {
  font-size: 0.95rem;
  color: var(--color-text-muted);
  line-height: 1.75;
}
.modal-description .bullet {
  font-size: 0.93rem;
  color: var(--color-text);
  line-height: 1.65;
  padding-left: 4px;
}

.modal-tech-label {
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-dim);
  margin-bottom: 12px;
}
.modal-tech {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.tech-chip {
  padding: 6px 16px;
  border-radius: 999px;
  border: 1.5px solid;
  font-size: 0.82rem;
  font-weight: 600;
  background: transparent;
}
</style>
