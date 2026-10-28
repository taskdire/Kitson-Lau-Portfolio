<template>
  <div
    id="top"
    class="site-shell"
    :class="[{ 'home-shell': activePage.id === 'home' }, `shell-${activePage.id}`]"
  >
    <header class="site-header">
      <a class="brand" href="#top" aria-label="Kitson Lau, back to top">
        <span class="brand-mark">KL</span>
        <span>Kitson Lau</span>
      </a>

      <nav class="site-nav" aria-label="Main navigation">
        <button
          v-for="(page, index) in pages"
          :key="page.id"
          type="button"
          :class="{ active: activeIndex === index }"
          :aria-current="activeIndex === index ? 'page' : undefined"
          @click="showPage(index)"
        >{{ page.label }}</button>
      </nav>

    </header>

    <main>
      <Transition :name="`page-${transitionDirection}`" mode="out-in">
        <section :key="activePage.id" class="page-view" :class="`view-${activePage.id}`">
          <template v-if="activePage.id === 'home'">
            <section class="home-hero">
              <div class="home-hero-copy">
                <p class="home-kicker">{{ personal.status }}</p>
                <h1>Hi, I'm {{ personal.name }}</h1>
                <p class="home-subtitle">{{ personal.title }}</p>
              </div>
            </section>
          </template>

          <template v-else-if="activePage.id === 'portfolio'">
            <header class="view-heading">
              <span class="section-index">01</span>
              <h1>Projects</h1>
            </header>
            <ProjectCarousel class="c-proj" />
          </template>

          <template v-else-if="activePage.id === 'experience'">
            <header class="view-heading">
              <span class="section-index">02</span>
              <h1>Experience</h1>
            </header>
            <ExperienceCard class="c-exp" />
          </template>

          <template v-else>
            <header class="view-heading">
              <span class="section-index">03</span>
              <h1>About</h1>
            </header>
            <CertsCard class="c-certs" />
          </template>
        </section>
      </Transition>
    </main>

  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import ProjectCarousel from './components/ProjectCarousel.vue'
import ExperienceCard  from './components/ExperienceCard.vue'
import CertsCard       from './components/CertsCard.vue'
import { personal } from './data/index.js'

const pages = [
  { id: 'home', label: 'Home' },
  { id: 'portfolio', label: 'Portfolio' },
  { id: 'experience', label: 'Resume' },
  { id: 'about', label: 'About' },
]

const activeIndex = ref(0)
const transitionDirection = ref('left')
const activePage = computed(() => pages[activeIndex.value])

const showPage = (index) => {
  if (index === activeIndex.value) return
  transitionDirection.value = index > activeIndex.value ? 'left' : 'right'
  activeIndex.value = index
}

onMounted(() => {
  document.querySelectorAll('.card').forEach((card, i) => {
    card.animate(
      [{ opacity: 0, transform: 'translateY(18px)' }, { opacity: 1, transform: 'translateY(0)' }],
      { duration: 520, delay: 70 + i * 90, easing: 'cubic-bezier(0.22,1,0.36,1)', fill: 'forwards' }
    )
  })
})
</script>
