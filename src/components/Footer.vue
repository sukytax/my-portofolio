<template>
  <footer id="contact">
    <div class="footer-inner section">
      <!-- CTA Headline -->
      <div class="footer-cta reveal" ref="ctaEl">
        <h2 class="footer-heading">Mari Berkolaborasi 🚀</h2>
        <p class="footer-desc">
          Punya proyek menarik atau sekadar ingin ngobrol soal teknologi?
          Jangan ragu untuk menghubungi saya!
        </p>
        <a :href="`https://mail.google.com/mail/?view=cm&to=${contact.email}`" target="_blank" rel="noopener noreferrer" class="btn-primary footer-email-btn">
          <svg xmlns="http://www.w3.org/2000/svg" width="17" height="17" viewBox="0 0 24 24"
            fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="4" width="20" height="16" rx="2"/>
            <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
          </svg>
          {{ contact.email }}
        </a>
      </div>

      <!-- Divider -->
      <div class="footer-divider"></div>

      <!-- Bottom row -->
      <div class="footer-bottom reveal" ref="bottomEl">
        <p class="footer-copy">
          © {{ year }} <strong>{{ name }}</strong>. Dibuat dengan Vue.js
        </p>
        <div class="footer-socials">
          <a :href="`https://mail.google.com/mail/?view=cm&to=${contact.email}`" target="_blank" rel="noopener noreferrer" title="Email" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="4" width="20" height="16" rx="2"/>
              <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/>
            </svg>
          </a>
          <a :href="contact.linkedin" target="_blank" title="LinkedIn" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
              <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/>
              <rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/>
            </svg>
          </a>
          <a :href="contact.github" target="_blank" title="GitHub" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="currentColor">
              <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35
                6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16
                2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5
                4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9
                18.13V22"/>
            </svg>
          </a>
          <a :href="contact.instagram" target="_blank" title="Instagram" class="social-icon">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24"
              fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <rect x="2" y="2" width="20" height="20" rx="5" ry="5"/>
              <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/>
              <line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/>
            </svg>
          </a>
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  contact: { type: Object, required: true },
  name:    { type: String, default: 'Portfolio' },
});

const year    = new Date().getFullYear();
const ctaEl   = ref(null);
const bottomEl = ref(null);

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); }),
    { threshold: 0.1 }
  );
  [ctaEl.value, bottomEl.value].filter(Boolean).forEach(el => observer.observe(el));
});
</script>

<style scoped>
footer {
  background: var(--color-bg-alt);
  border-top: 1px solid var(--color-border);
  position: relative;
  overflow: hidden;
}
footer::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse at 50% 0%, rgba(37,99,235,0.05) 0%, transparent 65%);
  pointer-events: none;
}

.footer-inner {
  display: flex;
  flex-direction: column;
  gap: 48px;
}

/* CTA */
.footer-cta {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 18px;
}
.footer-heading {
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  font-weight: 800;
  background: linear-gradient(135deg, var(--color-primary-dark), var(--color-primary-light));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
.footer-desc {
  max-width: 520px;
  color: var(--color-text-muted);
  font-size: 1rem;
  line-height: 1.75;
}
.footer-email-btn {
  font-size: 0.95rem;
  padding: 14px 32px;
}

/* Divider */
.footer-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--color-border), transparent);
}

/* Bottom */
.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 18px;
}
.footer-copy {
  font-size: 0.88rem;
  color: var(--color-text-dim);
}

/* Socials */
.footer-socials {
  display: flex;
  gap: 10px;
}
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

@media (max-width: 640px) {
  .footer-bottom { flex-direction: column; align-items: center; text-align: center; }
}
</style>
