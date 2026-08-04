<template>
  <section class="services" ref="sectionRef">
    <div class="services__head">
      <span class="services__eyebrow">O que fazemos</span>
      <h2 class="services__title">Serviços</h2>
      <p class="services__subtitle">
        Do projeto à manutenção, cuidamos de cada detalhe em metal e vidro.
      </p>

      <div class="services__tabs">
        <button
          v-for="(service, i) in services"
          :key="service.id"
          class="services__tab"
          :class="{ 'is-active': i === activeIndex }"
          type="button"
          @click="goTo(i)"
        >
          {{ service.title }}
        </button>
      </div>
    </div>

    <div
      class="services__slider-wrap"
      @mouseenter="pause"
      @mouseleave="resume"
    >
      <Swiper
        :modules="modules"
        effect="coverflow"
        :coverflow-effect="{
          rotate: 14,
          stretch: 0,
          depth: 160,
          modifier: 1.2,
          slideShadows: false
        }"
        centered-slides
        slides-per-view="auto"
        :space-between="24"
        loop
        :speed="700"
        :autoplay="{ delay: 2600, disableOnInteraction: false }"
        :pagination="{ el: paginationEl, clickable: true }"
        class="services__swiper"
        @swiper="onSwiperInit"
        @slide-change="onSlideChange"
      >
        <SwiperSlide
          v-for="service in services"
          :key="service.id"
          class="service-card"
        >
          <div
            class="service-card__image"
            :style="{ backgroundImage: `url(${service.image})` }"
          ></div>
          <div class="service-card__body">
            <span class="service-card__tag">{{ service.tag }}</span>
            <h3 class="service-card__title">{{ service.title }}</h3>
            <p class="service-card__desc">{{ service.description }}</p>
            <a class="service-card__link" href="#contato">
              Ver mais
              <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <path d="M9 6l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
              </svg>
            </a>
          </div>
        </SwiperSlide>
      </Swiper>

      <button
        class="services__nav services__nav--prev"
        type="button"
        aria-label="Serviço anterior"
        @click="swiperInstance?.slidePrev()"
      >
        <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path d="M15 6l-6 6 6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </button>
      <button
        class="services__nav services__nav--next"
        type="button"
        aria-label="Próximo serviço"
        @click="swiperInstance?.slideNext()"
      >
        <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path d="M9 6l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
        </svg>
      </button>
    </div>

    <div ref="paginationRef" class="services__pagination"></div>

    <div class="services__cta">
      <p class="services__cta-text">
        Não sabe qual serviço encaixa no seu projeto? Fala com a gente.
      </p>
      <a
        class="services__cta-button"
        href="https://wa.me/5500000000000"
        target="_blank"
        rel="noopener"
      >
        <svg class="services__cta-icon" viewBox="0 0 32 32" fill="currentColor" aria-hidden="true">
          <path
            d="M16.001 2.667c-7.364 0-13.334 5.97-13.334 13.333 0 2.353.615 4.56 1.692 6.474L2.667 29.333l7.05-1.848a13.27 13.27 0 0 0 6.284 1.6h.006c7.363 0 13.333-5.97 13.333-13.333S23.364 2.667 16.001 2.667Zm0 24.4h-.005a11.05 11.05 0 0 1-5.633-1.542l-.404-.24-4.184 1.097 1.117-4.078-.263-.418a11.03 11.03 0 0 1-1.696-5.886c0-6.114 4.977-11.09 11.093-11.09 2.963 0 5.748 1.155 7.843 3.252a11.02 11.02 0 0 1 3.246 7.845c0 6.114-4.977 11.06-11.114 11.06Zm6.083-8.284c-.333-.167-1.97-.972-2.276-1.083-.305-.111-.527-.167-.75.167-.221.333-.86 1.083-1.054 1.305-.194.222-.389.25-.722.083-.333-.167-1.407-.519-2.68-1.653-.99-.883-1.66-1.973-1.854-2.306-.194-.333-.02-.514.146-.68.15-.15.334-.389.5-.583.167-.194.222-.333.334-.556.11-.222.055-.417-.028-.583-.083-.167-.75-1.807-1.028-2.474-.27-.65-.546-.562-.75-.572l-.639-.011c-.222 0-.583.083-.888.417-.306.333-1.167 1.14-1.167 2.78s1.195 3.226 1.361 3.448c.167.222 2.352 3.592 5.7 5.04.796.344 1.417.55 1.9.703.798.254 1.525.218 2.1.133.64-.096 1.97-.805 2.248-1.583.278-.778.278-1.445.194-1.584-.083-.138-.305-.222-.638-.389Z"
          />
        </svg>
        Entrar em contato
      </a>
    </div>
  </section>
</template>

<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Autoplay, EffectCoverflow, Pagination } from 'swiper/modules'
import 'swiper/css'
import 'swiper/css/effect-coverflow'
import 'swiper/css/pagination'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const modules = [EffectCoverflow, Autoplay, Pagination]

// Fictitious images — swap for real project photos later.
const services = [
  {
    id: 1,
    title: 'Serralheria',
    tag: 'Estrutura & segurança',
    description:
      'Portões, grades e estruturas metálicas sob medida, com acabamento resistente e projeto pensado pro seu espaço.',
    image: '/IMG/serra.jpeg'
  },
  {
    id: 2,
    title: 'Porta de aço automática',
    tag: 'Automação',
    description:
      'Motorização silenciosa, controle remoto e trava anti-arrombamento pra quem quer praticidade com segurança.',
    image: '/IMG/porta.avif'
  },
  {
    id: 3,
    title: 'Vidraçaria',
    tag: 'Vidro temperado',
    description:
      'Box, janelas, espelhos e fachadas em vidro temperado, com instalação precisa e alto padrão de acabamento.',
    image: '/IMG/vidro.avif'
  },
  {
    id: 4,
    title: 'Manutenção',
    tag: 'Suporte contínuo',
    description:
      'Revisão periódica, lubrificação de trilhos e reparos rápidos pra manter tudo funcionando como no primeiro dia.',
    image: '/IMG/manu.avif'
  }
]

const sectionRef = ref(null)
const paginationRef = ref(null)
const paginationEl = ref(null)
const swiperInstance = ref(null)
const activeIndex = ref(0)

function onSwiperInit(swiper) {
  swiperInstance.value = swiper
}

function onSlideChange(swiper) {
  activeIndex.value = swiper.realIndex
}

function goTo(i) {
  swiperInstance.value?.slideToLoop(i)
}

function pause() {
  swiperInstance.value?.autoplay?.stop()
}

function resume() {
  swiperInstance.value?.autoplay?.start()
}

let ctx

onMounted(() => {
  paginationEl.value = paginationRef.value

  ctx = gsap.context(() => {
    gsap.from(
      '.services__eyebrow, .services__title, .services__subtitle, .services__tabs',
      {
        y: 24,
        opacity: 0,
        duration: 0.7,
        stagger: 0.08,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: sectionRef.value,
          start: 'top 78%'
        }
      }
    )

    gsap.from('.services__slider-wrap', {
      y: 32,
      opacity: 0,
      duration: 0.8,
      delay: 0.15,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 72%'
      }
    })

    gsap.from('.services__cta-text, .services__cta-button', {
      y: 20,
      opacity: 0,
      duration: 0.6,
      stagger: 0.1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: '.services__cta',
        start: 'top 88%'
      }
    })
  }, sectionRef.value)
})

onBeforeUnmount(() => {
  ctx?.revert()
})
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.services {
  width: 100%;
  overflow: hidden;
  background: #f4f6f9;
  padding: clamp(32px, 5vw, 56px) 0 clamp(56px, 9vw, 100px);
  padding-top: 0;
}

.services__head {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 clamp(20px, 6vw, 56px);
  text-align: center;
}

.services__eyebrow {
  display: inline-block;
  font-size: 0.82rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #4caf50;
  margin-bottom: 10px;
}

.services__title {
  margin: 0 0 12px;
  font-size: clamp(2rem, 3.6vw, 2.8rem);
  font-weight: 800;
  color: #3a3a3a;
  line-height: 1.15;
}

.services__subtitle {
  margin: 0 auto;
  max-width: 46ch;
  font-size: 1rem;
  line-height: 1.6;
  color: #8a8f98;
}

.services__tabs {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
  margin-top: 26px;
}

.services__tab {
  border: 1px solid #dfe3e6;
  background: #fff;
  color: #5b5f66;
  font-size: 0.85rem;
  font-weight: 600;
  padding: 8px 16px;
  border-radius: 999px;
  cursor: pointer;
  transition: background 0.25s ease, color 0.25s ease, border-color 0.25s ease,
    transform 0.2s ease;
}

.services__tab:hover {
  transform: translateY(-1px);
}

.services__tab.is-active {
  background: #4caf50;
  border-color: #4caf50;
  color: #fff;
}

/* ---------- slider ---------- */
.services__slider-wrap {
  position: relative;
  margin-top: clamp(36px, 6vw, 56px);
}

.services__swiper {
  width: 100%;
  padding: 20px 0 30px;
}

.service-card {
  width: clamp(220px, 68vw, 300px);
  border-radius: 20px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 18px 40px rgba(20, 40, 30, 0.14);
  display: flex;
  flex-direction: column;
}

.service-card__image {
  height: 200px;
  background-size: cover;
  background-position: center;
}

.service-card__body {
  flex: 1;
  padding: 18px 20px 22px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.service-card__tag {
  align-self: flex-start;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: #4caf50;
  background: rgba(76, 175, 80, 0.12);
  padding: 4px 10px;
  border-radius: 999px;
}

.service-card__title {
  margin: 4px 0 0;
  font-size: 1.1rem;
  font-weight: 800;
  color: #2f2f2f;
}

.service-card__desc {
  margin: 0;
  font-size: 0.85rem;
  line-height: 1.5;
  color: #8a8f98;
}

.service-card__link {
  margin-top: auto;
  padding-top: 10px;
  display: inline-flex;
  align-items: center;
  gap: 4px;
  font-size: 0.85rem;
  font-weight: 700;
  color: #4caf50;
  text-decoration: none;
  width: fit-content;
}

.service-card__link svg {
  width: 16px;
  height: 16px;
  transition: transform 0.25s ease;
}

.service-card__link:hover svg {
  transform: translateX(3px);
}

/* ---------- nav arrows ---------- */
.services__nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 5;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  border: none;
  background: #fff;
  color: #4caf50;
  box-shadow: 0 6px 18px rgba(20, 40, 30, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.25s ease, transform 0.2s ease, box-shadow 0.25s ease;
}

.services__nav svg {
  width: 20px;
  height: 20px;
}

.services__nav--prev {
  left: clamp(6px, 3vw, 24px);
}
.services__nav--next {
  right: clamp(6px, 3vw, 24px);
}

.services__nav:hover {
  background: #4caf50;
  color: #fff;
  transform: translateY(-50%) scale(1.06);
  box-shadow: 0 10px 22px rgba(76, 175, 80, 0.32);
}

@media (max-width: 640px) {
  .services__nav {
    display: none;
  }
}

/* ---------- pagination ---------- */
.services__pagination {
  display: flex;
  justify-content: center;
  gap: 6px;
  margin-top: 4px;
}

.services__pagination :deep(.swiper-pagination-bullet) {
  width: 7px;
  height: 7px;
  background: #c7ccd1;
  opacity: 1;
  transition: background 0.25s ease, width 0.25s ease;
  border-radius: 999px;
  cursor: pointer;
}

.services__pagination :deep(.swiper-pagination-bullet-active) {
  width: 20px;
  background: #4caf50;
}

@media (prefers-reduced-motion: reduce) {
  .services__tab,
  .services__nav,
  .services__pagination :deep(.swiper-pagination-bullet) {
    transition: none !important;
  }
}

/* ---------- bottom CTA ---------- */
.services__cta {
  max-width: 640px;
  margin: clamp(40px, 6vw, 64px) auto 0;
  padding: 0 clamp(20px, 6vw, 56px);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 18px;
  text-align: center;
}

.services__cta-text {
  margin: 0;
  font-size: 1.05rem;
  font-weight: 600;
  color: #3a3a3a;
}

.services__cta-button {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  padding: 13px 26px;
  border-radius: 999px;
  background: #4caf50;
  color: #fff;
  font-size: 0.95rem;
  font-weight: 700;
  text-decoration: none;
  transition: background 0.25s ease, transform 0.2s ease, box-shadow 0.25s ease;
}

.services__cta-icon {
  width: 18px;
  height: 18px;
}

.services__cta-button:hover {
  background: #43a047;
  transform: translateY(-2px);
  box-shadow: 0 10px 22px rgba(76, 175, 80, 0.32);
}
</style>