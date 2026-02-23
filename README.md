<div align="center">

# 🚀 Bimo Bagas Riyanto — Personal Portfolio

[![Vue.js](https://img.shields.io/badge/Vue.js-3.x-4FC08D?style=for-the-badge&logo=vuedotjs&logoColor=white)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-5.x-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES2022-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![CSS3](https://img.shields.io/badge/CSS3-Vanilla-1572B6?style=for-the-badge&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![Deployed on Vercel](https://img.shields.io/badge/Deployed%20on-Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://vercel.com/)

**A modern, minimalist personal portfolio website built with Vue 3 + Vite.**  
Showcasing my skills, certifications, and professional experience as an Informatics Engineering student and Data Science enthusiast.

[🌐 Live Demo](#) · [💼 LinkedIn](https://linkedin.com/in/bimobagasriyanto) · [📂 GitHub](https://github.com/sukytax)

</div>

---

## ✨ Features

- 🎨 **Minimalist Design** — Clean white & blue color palette with smooth animations
- 📱 **Fully Responsive** — Looks great on desktop, tablet, and mobile
- 🪄 **Smooth Interactions** — Scroll-reveal animations, tab transitions, hover effects
- 📜 **Certificate Viewer** — Browse certifications in a carousel with lightbox preview (view-only, no download)
- 💼 **Experience Timeline** — Expandable cards with detailed role descriptions
- 🎓 **About Me & Education** — Inline tab panel in the hero section
- 📬 **Gmail Integration** — Contact buttons open Gmail Compose directly
- ⚡ **Fast & Lightweight** — Built with Vite for instant HMR and optimized builds

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | [Vue 3](https://vuejs.org/) (Composition API + `<script setup>`) |
| Build Tool | [Vite 5](https://vitejs.dev/) |
| Styling | Vanilla CSS with CSS Custom Properties |
| Fonts | [Outfit](https://fonts.google.com/specimen/Outfit) via Google Fonts |
| Deployment | [Vercel](https://vercel.com/) |

---

## 📁 Project Structure

```
MyPortofolio/
├── public/
│   ├── FotoProfile.jpeg        # Profile photo
│   ├── cv/                     # CV file
│   └── certificates/           # Certificate PDF files
├── src/
│   ├── components/
│   │   ├── Navbar.vue          # Navigation bar
│   │   ├── Hero.vue            # Hero section with About/Education tabs
│   │   ├── Certificates.vue    # Certificate carousel + lightbox
│   │   ├── Experience.vue      # Experience timeline
│   │   ├── ExperienceModal.vue # Experience detail modal
│   │   └── Footer.vue          # Footer with contact links
│   ├── data.js                 # All content data (profile, certs, experience)
│   ├── style.css               # Global CSS variables & utilities
│   ├── App.vue                 # Root component
│   └── main.js                 # App entry point
├── index.html
├── vite.config.js
└── package.json
```

---

## 🚀 Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) v18+
- npm v9+

### Installation

```bash
# Clone the repository
git clone https://github.com/sukytax/my-portfolio.git
cd my-portfolio

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will be available at `http://localhost:5173`

### Build for Production

```bash
npm run build
```

---

## 🎨 Customization

All content is centralized in [`src/data.js`](./src/data.js):

```js
export const profile = {
  name: 'Your Name',
  title: 'Your Title',
  about: ['Paragraph 1...', 'Paragraph 2...'],
  photo: '/FotoProfile.jpeg',
  cv: '/cv/YourCV.pdf',
  socials: { email, linkedin, github, instagram },
  education: [{ degree, school, period, gpa, description }],
};

export const certificates = [...];
export const experiences  = [...];
```

---

## 📦 Deployment (Vercel)

This project is configured for zero-config deployment on Vercel:

1. Push this repository to GitHub
2. Import the project on [vercel.com](https://vercel.com/)
3. Vercel auto-detects Vite — just click **Deploy**
4. Your portfolio will be live at `https://your-project.vercel.app`

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

<div align="center">
  Made with ❤️ by <a href="https://linkedin.com/in/bimobagasriyanto">Bimo Bagas Riyanto</a>
</div>
