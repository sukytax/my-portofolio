<template>
  <div id="app">
    <Navbar :name="profile.name" :email="profile.socials.email" />
    <main>
      <Hero :profile="profile" />
      <hr class="section-divider" />
      <Certificates :certificates="certificates" />
      <hr class="section-divider" />
      <Experience :experiences="experiences" />
      <hr class="section-divider" />
      <Publications :publications="publications" />
    </main>
    <Footer :contact="profile.socials" :name="profile.name" />
  </div>
</template>

<script setup>
import Navbar       from './components/Navbar.vue';
import Hero         from './components/Hero.vue';
import Certificates from './components/Certificates.vue';
import Experience   from './components/Experience.vue';
import Publications from './components/Publications.vue';
import Footer       from './components/Footer.vue';
import { profile, certificates, experiences, publications } from './data.js';
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
main { flex: 1; }
</style>
