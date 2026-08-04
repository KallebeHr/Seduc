<template>
  <section class="hero">
    <div class="hero__inner">
      <div class="hero__media" :class="{ 'is-in': mounted }">
        <span class="hero__dot" aria-hidden="true"></span>
        <img
          :src="randomImage"
          alt="Serralheria e Vidraçaria"
          class="hero__img"
        />
      </div>

      <div class="hero__content">
        <h1 class="hero__title" :class="{ 'is-in': mounted }">
          Serralheria &amp; Vidraçaria
        </h1>

        <h2 class="hero__subtitle" :class="{ 'is-in': mounted }">
          Com mais de {{ years }} anos de experiência
        </h2>

        <p class="hero__tagline" :class="{ 'is-in': mounted }">
          Construindo histórias, realizando sonhos
        </p>

        <button
          class="hero__cta"
          :class="{ 'is-in': mounted }"
          @click="$emit('contact')"
        >
          <svg
            class="hero__cta-icon"
            viewBox="0 0 32 32"
            fill="currentColor"
            aria-hidden="true"
          >
            <path
              d="M16.001 2.667c-7.364 0-13.334 5.97-13.334 13.333 0 2.353.615 4.56 1.692 6.474L2.667 29.333l7.05-1.848a13.27 13.27 0 0 0 6.284 1.6h.006c7.363 0 13.333-5.97 13.333-13.333S23.364 2.667 16.001 2.667Zm0 24.4h-.005a11.05 11.05 0 0 1-5.633-1.542l-.404-.24-4.184 1.097 1.117-4.078-.263-.418a11.03 11.03 0 0 1-1.696-5.886c0-6.114 4.977-11.09 11.093-11.09 2.963 0 5.748 1.155 7.843 3.252a11.02 11.02 0 0 1 3.246 7.845c0 6.114-4.977 11.06-11.114 11.06Zm6.083-8.284c-.333-.167-1.97-.972-2.276-1.083-.305-.111-.527-.167-.75.167-.221.333-.86 1.083-1.054 1.305-.194.222-.389.25-.722.083-.333-.167-1.407-.519-2.68-1.653-.99-.883-1.66-1.973-1.854-2.306-.194-.333-.02-.514.146-.68.15-.15.334-.389.5-.583.167-.194.222-.333.334-.556.11-.222.055-.417-.028-.583-.083-.167-.75-1.807-1.028-2.474-.27-.65-.546-.562-.75-.572l-.639-.011c-.222 0-.583.083-.888.417-.306.333-1.167 1.14-1.167 2.78s1.195 3.226 1.361 3.448c.167.222 2.352 3.592 5.7 5.04.796.344 1.417.55 1.9.703.798.254 1.525.218 2.1.133.64-.096 1.97-.805 2.248-1.583.278-.778.278-1.445.194-1.584-.083-.138-.305-.222-.638-.389Z"
            />
          </svg>
          <span>Entrar em contato.</span>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from 'vue'

defineEmits(['contact'])

const props = defineProps({
  years: {
    type: [String, Number],
    default: 25
  }
})

const mounted = ref(false)

// Swap for your real logo/icon whenever needed.
const randomImage = ref('/IMG/eee.png')

onMounted(() => {
  requestAnimationFrame(() => {
    setTimeout(() => {
      mounted.value = true
    }, 60)
  })
})
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.hero {
  /* Height reserved for the fixed NavHeader — tweak here if the header's
     real height changes, everything below reacts to this single value. */
  --nav-offset: 5rem;

  width: 100%;
  min-height: clamp(540px, 88vh, 780px);
  display: flex;
  align-items: center;
  background: #f4f6f9;
  padding-top: var(--nav-offset);
}

.hero__inner {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: clamp(24px, 5vw, 64px);
  padding: clamp(28px, 5vw, 64px) clamp(20px, 6vw, 56px);
  flex-wrap: wrap;
}

/* ---------- media / icon ---------- */
.hero__media {
  position: relative;
  flex-shrink: 0;
  width: clamp(150px, 22vw, 310px);
  aspect-ratio: 1 / 1;
  opacity: 0;
  transform: translateY(16px) scale(0.94);
  transition: opacity 0.7s ease, transform 0.7s ease;
  animation: float 4.5s ease-in-out infinite;
  animation-play-state: paused;
}
.hero__media.is-in {
  opacity: 1;
  transform: translateY(0) scale(1);
  animation-play-state: running;
}

.hero__img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  display: block;
}

.hero__dot {
  position: absolute;
  top: 6%;
  right: 4%;
  width: 14%;
  aspect-ratio: 1 / 1;
  border-radius: 50%;
  background: #66bb6a;
  animation: pulse 2.2s ease-in-out infinite;
  z-index: 2;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-12px); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.25); opacity: 0.6; }
}

/* ---------- content ---------- */
.hero__content {
  display: flex;
  flex-direction: column;
  min-width: min(100%, 320px);
  max-width: 620px;
  flex: 1 1 320px;
}

.hero__title,
.hero__subtitle,
.hero__tagline,
.hero__cta {
  opacity: 0;
  transform: translateY(14px);
  transition: opacity 0.55s ease, transform 0.55s ease;
}

.hero__title.is-in { transition-delay: 0.05s; }
.hero__subtitle.is-in { transition-delay: 0.15s; }
.hero__tagline.is-in { transition-delay: 0.25s; }
.hero__cta.is-in { transition-delay: 0.35s; }

.hero__title.is-in,
.hero__subtitle.is-in,
.hero__tagline.is-in,
.hero__cta.is-in {
  opacity: 1;
  transform: translateY(0);
}

.hero__title {
  margin: 0;
  font-size: clamp(1.75rem, 4.6vw + 0.6rem, 3.6rem);
  font-weight: 800;
  color: #3a3a3a;
  line-height: 1.12;
  letter-spacing: -0.01em;
}

.hero__subtitle {
  margin: 6px 0 0;
  font-size: clamp(1.75rem, 4.6vw + 0.6rem, 3.6rem);
  font-weight: 800;
  color: #4caf50;
  line-height: 1.12;
  letter-spacing: -0.01em;
}

.hero__tagline {
  margin: clamp(12px, 2.5vw, 18px) 0 clamp(20px, 4vw, 28px);
  font-size: clamp(0.88rem, 0.8vw + 0.65rem, 1.05rem);
  line-height: 1.5;
  color: #8a8f98;
}

.hero__cta {
  align-self: flex-start;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: clamp(12px, 1.6vw, 14px) clamp(20px, 3vw, 26px);
  border: none;
  border-radius: 8px;
  background: #4caf50;
  color: #fff;
  font-size: clamp(0.9rem, 0.5vw + 0.75rem, 1rem);
  font-weight: 600;
  cursor: pointer;
  transition: background 0.25s ease, transform 0.2s ease, box-shadow 0.25s ease,
    opacity 0.55s ease, translate 0.55s ease;
}

.hero__cta-icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}

.hero__cta:hover {
  background: #43a047;
  transform: translateY(-2px);
  box-shadow: 0 8px 18px rgba(76, 175, 80, 0.3);
}

.hero__cta:active {
  transform: translateY(0);
}

/* ---------- responsive ---------- */
@media (max-width: 720px) {
  .hero {
    --nav-offset: 4.5rem;
    min-height: auto;
  }
  .hero__inner {
    flex-direction: column;
    text-align: center;
  }
  .hero__content {
    align-items: center;
    max-width: 100%;
  }
  .hero__cta {
    align-self: center;
  }
}

@media (max-width: 380px) {
  .hero {
    --nav-offset: 4rem;
  }
}

/* respect reduced motion preference */
@media (prefers-reduced-motion: reduce) {
  .hero__media,
  .hero__title,
  .hero__subtitle,
  .hero__tagline,
  .hero__cta,
  .hero__dot {
    animation: none !important;
    transition: none !important;
    opacity: 1 !important;
    transform: none !important;
  }
}
</style>