<template>
  <nav class="navbar" :class="{ scrolled: isScrolled, 'menu-open': menuOpen }">
    <div class="navbar-inner">
      <!-- Logo -->
      <a class="navbar-logo" @click.prevent="scrollTo('hero')">
        <span class="logo-dot"></span>
        <span>{{ name }}<span class="logo-accent"></span></span>
      </a>

      <!-- Desktop Nav -->
      <ul class="navbar-links">
        <li v-for="link in links" :key="link.id">
          <a
            :href="`#${link.id}`"
            :class="{ active: activeSection === link.id }"
            @click.prevent="scrollTo(link.id)"
          >{{ link.label }}</a>
        </li>
      </ul>

      <!-- CTA -->
      <a :href="`https://mail.google.com/mail/?view=cm&to=${email}`" target="_blank" rel="noopener noreferrer" class="navbar-cta">
        Hubungi Saya
      </a>

      <!-- Hamburger -->
      <button class="hamburger" @click="toggleMenu" :class="{ active: menuOpen }" aria-label="Toggle menu">
        <span></span><span></span><span></span>
      </button>
    </div>

    <!-- Mobile Menu -->
    <div class="mobile-menu" :class="{ open: menuOpen }">
      <ul>
        <li v-for="link in links" :key="link.id">
          <a
            :href="`#${link.id}`"
            @click.prevent="() => { scrollTo(link.id); menuOpen = false; }"
          >{{ link.label }}</a>
        </li>
      </ul>
      <a :href="`https://mail.google.com/mail/?view=cm&to=${email}`" target="_blank" rel="noopener noreferrer" class="btn-primary mobile-cta">Hubungi Saya</a>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  name:  { type: String, default: 'Portfolio' },
  email: { type: String, default: '' },
});

const isScrolled = ref(false);
const menuOpen   = ref(false);
const activeSection = ref('hero');

const links = [
  { id: 'hero',         label: 'Home' },
  { id: 'certificates', label: 'Sertifikat' },
  { id: 'experience',   label: 'Pengalaman' },
  { id: 'publications', label: 'Publikasi' },
  { id: 'contact',      label: 'Kontak' },
];

function toggleMenu() { menuOpen.value = !menuOpen.value; }

function scrollTo(id) {
  const el = document.getElementById(id);
  if (el) {
    const offset = 68;
    const top = el.getBoundingClientRect().top + window.scrollY - offset;
    window.scrollTo({ top, behavior: 'smooth' });
  }
}

function onScroll() {
  isScrolled.value = window.scrollY > 20;

  // Detect active section
  for (const link of [...links].reverse()) {
    const el = document.getElementById(link.id);
    if (el) {
      const rect = el.getBoundingClientRect();
      if (rect.top <= 100) {
        activeSection.value = link.id;
        break;
      }
    }
  }
}

onMounted(() => window.addEventListener('scroll', onScroll));
onUnmounted(() => window.removeEventListener('scroll', onScroll));
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  transition: background 0.4s ease, box-shadow 0.4s ease, backdrop-filter 0.4s ease;
}

.navbar.scrolled {
  background: rgba(255, 255, 255, 0.92);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 1px 0 rgba(37, 99, 235, 0.12), 0 4px 16px rgba(37,99,235,0.06);
}

.navbar-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 1100px;
  margin: 0 auto;
  padding: 0 24px;
  height: 68px;
  gap: 24px;
}

/* Logo */
.navbar-logo {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 1.2rem;
  font-weight: 800;
  color: var(--color-text);
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
}
.logo-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--color-primary);
  box-shadow: 0 0 8px rgba(37,99,235,0.4);
}
.logo-accent { color: var(--color-primary); }

/* Nav links */
.navbar-links {
  display: flex;
  list-style: none;
  gap: 6px;
}
.navbar-links a {
  padding: 7px 16px;
  border-radius: 999px;
  font-size: 0.92rem;
  font-weight: 500;
  color: var(--color-text-muted);
  transition: var(--transition);
}
.navbar-links a:hover,
.navbar-links a.active {
  color: var(--color-primary);
  background: rgba(37, 99, 235, 0.08);
}
.navbar-links a.active { color: var(--color-primary-dark); }

/* CTA */
.navbar-cta {
  padding: 9px 22px;
  border-radius: 999px;
  background: linear-gradient(135deg, var(--color-primary), var(--color-primary-dark));
  color: #fff;
  font-size: 0.88rem;
  font-weight: 600;
  transition: var(--transition);
  white-space: nowrap;
  box-shadow: 0 2px 14px rgba(108,99,255,0.35);
}
.navbar-cta:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(108,99,255,0.5);
}

/* Hamburger */
.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  cursor: pointer;
  background: none;
  border: none;
  padding: 8px;
}
.hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--color-text);
  border-radius: 2px;
  transition: var(--transition);
  transform-origin: center;
}
.hamburger.active span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.hamburger.active span:nth-child(2) { opacity: 0; transform: scaleX(0); }
.hamburger.active span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

/* Mobile Menu */
.mobile-menu {
  flex-direction: column;
  gap: 12px;
  padding: 0 24px;
  background: rgba(255, 255, 255, 0.97);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid var(--color-border);
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease, padding 0.35s ease;
  display: flex;
}
.mobile-menu.open {
  max-height: 400px;
  padding: 20px 24px 28px;
}
.mobile-menu ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.mobile-menu ul a {
  display: block;
  padding: 10px 14px;
  border-radius: var(--radius-sm);
  font-size: 1rem;
  font-weight: 500;
  color: var(--color-text-muted);
  transition: var(--transition);
}
.mobile-menu ul a:hover { color: var(--color-primary); background: rgba(37,99,235,0.08); }
.mobile-cta {
  align-self: flex-start;
  margin-top: 8px;
}

@media (min-width: 769px) {
  .hamburger { display: none !important; }
  .mobile-menu { display: none !important; }
}
@media (max-width: 768px) {
  .navbar-links,
  .navbar-cta { display: none !important; }

  .hamburger {
    display: flex;
    margin-left: auto;
    flex-shrink: 0;
  }

  .navbar-logo {
    font-size: 1rem;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: calc(100% - 60px);
  }

  .navbar-inner {
    gap: 0;
    padding: 0 16px;
  }
}
</style>
