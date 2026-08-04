<template>
  <header class="nav" :class="{ 'nav--scrolled': scrolled, 'nav--hidden': hidden }">
    <div class="nav__bar">
      <a class="nav__brand" href="#" @click="closeMenu">
        <img class="nav__logo" :src="logoImg" alt="Tropical" />
        <span class="nav__brand-name">TROPICAL</span>
      </a>

      <nav class="nav__links" aria-label="Navegação principal">
        <a
          v-for="link in links"
          :key="link.label"
          :href="link.href"
          class="nav__link"
        >
          {{ link.label }}
        </a>

        <a href="#carrinho" class="nav__link nav__link--cart">
          <svg class="nav__cart-icon" viewBox="0 0 24 24" fill="none" aria-hidden="true">
            <path
              d="M3 4h2l2.4 12.2a2 2 0 0 0 2 1.6h7.2a2 2 0 0 0 2-1.6L20 8H6"
              stroke="currentColor"
              stroke-width="1.8"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <circle cx="10" cy="20" r="1.4" fill="currentColor" />
            <circle cx="17" cy="20" r="1.4" fill="currentColor" />
          </svg>
          Carrinho
          <span v-if="cartCount" class="nav__badge">{{ cartCount }}</span>
        </a>
      </nav>

      <a
        class="nav__cta"
        href="https://wa.me/5500000000000"
        target="_blank"
        rel="noopener"
      >
        <svg class="nav__cta-icon" viewBox="0 0 32 32" fill="currentColor" aria-hidden="true">
          <path
            d="M16.001 2.667c-7.364 0-13.334 5.97-13.334 13.333 0 2.353.615 4.56 1.692 6.474L2.667 29.333l7.05-1.848a13.27 13.27 0 0 0 6.284 1.6h.006c7.363 0 13.333-5.97 13.333-13.333S23.364 2.667 16.001 2.667Zm0 24.4h-.005a11.05 11.05 0 0 1-5.633-1.542l-.404-.24-4.184 1.097 1.117-4.078-.263-.418a11.03 11.03 0 0 1-1.696-5.886c0-6.114 4.977-11.09 11.093-11.09 2.963 0 5.748 1.155 7.843 3.252a11.02 11.02 0 0 1 3.246 7.845c0 6.114-4.977 11.06-11.114 11.06Zm6.083-8.284c-.333-.167-1.97-.972-2.276-1.083-.305-.111-.527-.167-.75.167-.221.333-.86 1.083-1.054 1.305-.194.222-.389.25-.722.083-.333-.167-1.407-.519-2.68-1.653-.99-.883-1.66-1.973-1.854-2.306-.194-.333-.02-.514.146-.68.15-.15.334-.389.5-.583.167-.194.222-.333.334-.556.11-.222.055-.417-.028-.583-.083-.167-.75-1.807-1.028-2.474-.27-.65-.546-.562-.75-.572l-.639-.011c-.222 0-.583.083-.888.417-.306.333-1.167 1.14-1.167 2.78s1.195 3.226 1.361 3.448c.167.222 2.352 3.592 5.7 5.04.796.344 1.417.55 1.9.703.798.254 1.525.218 2.1.133.64-.096 1.97-.805 2.248-1.583.278-.778.278-1.445.194-1.584-.083-.138-.305-.222-.638-.389Z"
          />
        </svg>
        WhatsApp
      </a>

      <button
        class="nav__burger"
        :class="{ 'is-open': menuOpen }"
        type="button"
        :aria-expanded="menuOpen"
        aria-label="Abrir menu"
        @click="toggleMenu"
      >
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>

    <Transition name="fullmenu">
      <div v-if="menuOpen" class="nav__fullmenu" @click="closeMenu">
        <button
          class="nav__close"
          type="button"
          aria-label="Fechar menu"
          @click.stop="closeMenu"
        >
          <svg viewBox="0 0 24 24" fill="none" aria-hidden="true">
            <path d="M6 6l12 12M18 6L6 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" />
          </svg>
        </button>

        <div class="nav__fullmenu-content" @click.stop>
          <a class="nav__fullmenu-brand" href="#" @click="closeMenu">
            <img class="nav__logo nav__logo--lg" :src="logoImg" alt="Tropical" />
            <span>TROPICAL</span>
          </a>

          <nav class="nav__fullmenu-links" aria-label="Navegação mobile">
            <a
              v-for="(link, i) in links"
              :key="link.label"
              :href="link.href"
              class="nav__fullmenu-link"
              :style="{ transitionDelay: `${0.06 + i * 0.05}s` }"
              @click="closeMenu"
            >
              {{ link.label }}
            </a>

            <a
              href="#carrinho"
              class="nav__fullmenu-link nav__fullmenu-link--cart"
              :style="{ transitionDelay: `${0.06 + links.length * 0.05}s` }"
              @click="closeMenu"
            >
              Carrinho
              <span v-if="cartCount" class="nav__badge">{{ cartCount }}</span>
            </a>
          </nav>

          <a
            class="nav__fullmenu-cta"
            href="https://wa.me/5500000000000"
            target="_blank"
            rel="noopener"
            :style="{ transitionDelay: `${0.12 + links.length * 0.05}s` }"
            @click="closeMenu"
          >
            <svg viewBox="0 0 32 32" fill="currentColor" aria-hidden="true">
              <path
                d="M16.001 2.667c-7.364 0-13.334 5.97-13.334 13.333 0 2.353.615 4.56 1.692 6.474L2.667 29.333l7.05-1.848a13.27 13.27 0 0 0 6.284 1.6h.006c7.363 0 13.333-5.97 13.333-13.333S23.364 2.667 16.001 2.667Zm0 24.4h-.005a11.05 11.05 0 0 1-5.633-1.542l-.404-.24-4.184 1.097 1.117-4.078-.263-.418a11.03 11.03 0 0 1-1.696-5.886c0-6.114 4.977-11.09 11.093-11.09 2.963 0 5.748 1.155 7.843 3.252a11.02 11.02 0 0 1 3.246 7.845c0 6.114-4.977 11.06-11.114 11.06Zm6.083-8.284c-.333-.167-1.97-.972-2.276-1.083-.305-.111-.527-.167-.75.167-.221.333-.86 1.083-1.054 1.305-.194.222-.389.25-.722.083-.333-.167-1.407-.519-2.68-1.653-.99-.883-1.66-1.973-1.854-2.306-.194-.333-.02-.514.146-.68.15-.15.334-.389.5-.583.167-.194.222-.333.334-.556.11-.222.055-.417-.028-.583-.083-.167-.75-1.807-1.028-2.474-.27-.65-.546-.562-.75-.572l-.639-.011c-.222 0-.583.083-.888.417-.306.333-1.167 1.14-1.167 2.78s1.195 3.226 1.361 3.448c.167.222 2.352 3.592 5.7 5.04.796.344 1.417.55 1.9.703.798.254 1.525.218 2.1.133.64-.096 1.97-.805 2.248-1.583.278-.778.278-1.445.194-1.584-.083-.138-.305-.222-.638-.389Z"
              />
            </svg>
            Falar no WhatsApp
          </a>
        </div>
      </div>
    </Transition>
  </header>
</template>

<script setup>
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'

// Placeholder logo — swap `logoImg` for the real one later.
const logoImg = ref('/IMG/eee.png')

const links = [
  { label: 'Início', href: '#inicio' },
  { label: 'Serviços', href: '#servicos' },
  { label: 'Quem Somos', href: '#quem-somos' },
  { label: 'Produtos', href: '#produtos' },
  { label: 'Projetos Realizados', href: '#projetos' },
  { label: 'Avaliações', href: '#avaliacoes' }
]

const cartCount = ref(2)
const scrolled = ref(false)
const menuOpen = ref(false)
const hidden = ref(false)
const lastScrollY = ref(0)

function onScroll() {
  const currentY = window.scrollY
  scrolled.value = currentY > 12

  if (menuOpen.value) {
    lastScrollY.value = currentY
    return
  }

  const delta = currentY - lastScrollY.value
  // ignore tiny jitters, and always show near the very top
  if (currentY <= 80) {
    hidden.value = false
  } else if (delta > 6) {
    hidden.value = true
  } else if (delta < -6) {
    hidden.value = false
  }

  lastScrollY.value = currentY
}

function toggleMenu() {
  menuOpen.value = !menuOpen.value
  if (menuOpen.value) {
    hidden.value = false
  }
}

function closeMenu() {
  menuOpen.value = false
}

function onKeydown(e) {
  if (e.key === 'Escape') closeMenu()
}

watch(menuOpen, (open) => {
  document.body.style.overflow = open ? 'hidden' : ''
})

onMounted(() => {
  onScroll()
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('keydown', onKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('keydown', onKeydown)
  document.body.style.overflow = ''
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@500;600;700;800&family=Plus+Jakarta+Sans:wght@600;700;800&display=swap');

* {
  box-sizing: border-box;
}

.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 999;
  padding: 16px clamp(12px, 3vw, 28px) 0;
  transform: translateY(0);
  transition: transform 0.35s ease;
  font-family: 'Manrope', 'Segoe UI', system-ui, sans-serif;
}

.nav--hidden {
  transform: translateY(-130%);
}

.nav__bar {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: clamp(16px, 2.5vw, 32px);
  padding: 10px 18px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(14px) saturate(160%);
  -webkit-backdrop-filter: blur(14px) saturate(160%);
  border: 1px solid rgba(255, 255, 255, 0.6);
  box-shadow: 0 8px 24px rgba(20, 40, 30, 0.06);
  transition: box-shadow 0.35s ease, padding 0.35s ease, background 0.35s ease,
    transform 0.35s ease;
}

.nav--scrolled .nav__bar {
  padding: 6px 18px;
  box-shadow: 0 14px 34px rgba(20, 40, 30, 0.12);
  background: rgba(255, 255, 255, 0.9);
}

/* ---------- brand ---------- */
.nav__brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  flex-shrink: 0;
}

.nav__logo {
  width: 34px;
  height: 34px;
  object-fit: contain;
  transition: transform 0.35s ease;
}

.nav__brand:hover .nav__logo {
  transform: rotate(-8deg) scale(1.06);
}

.nav__brand-name {
  font-family: 'Plus Jakarta Sans', 'Manrope', sans-serif;
  font-weight: 800;
  font-size: 1rem;
  letter-spacing: 0.05em;
  color: #2f2f2f;
  white-space: nowrap;
}

/* ---------- links ---------- */
.nav__links {
  display: flex;
  align-items: center;
  gap: clamp(14px, 1.6vw, 26px);
  margin-left: 8px;
  flex: 1;
}

.nav__link {
  position: relative;
  font-size: 0.92rem;
  font-weight: 600;
  color: #3f3f3f;
  text-decoration: none;
  white-space: nowrap;
  padding: 4px 0;
  transition: color 0.25s ease;
}

.nav__link::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -2px;
  width: 100%;
  height: 2px;
  background: #4caf50;
  border-radius: 2px;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s ease;
}

.nav__link:hover {
  color: #1f1f1f;
}
.nav__link:hover::after {
  transform: scaleX(1);
}

.nav__link--cart {
  color: #4caf50;
  font-weight: 700;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  margin-left: auto;
}
.nav__link--cart::after {
  background: #4caf50;
}

.nav__cart-icon {
  width: 18px;
  height: 18px;
}

.nav__badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 18px;
  height: 18px;
  padding: 0 5px;
  border-radius: 999px;
  background: #ff7043;
  color: #fff;
  font-size: 0.68rem;
  font-weight: 700;
  line-height: 1;
}

/* ---------- CTA ---------- */
.nav__cta {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
  padding: 10px 18px;
  border-radius: 999px;
  background: #4caf50;
  color: #fff;
  font-size: 0.9rem;
  font-weight: 700;
  text-decoration: none;
  transition: background 0.25s ease, transform 0.2s ease, box-shadow 0.25s ease;
}

.nav__cta-icon {
  width: 16px;
  height: 16px;
}

.nav__cta:hover {
  background: #43a047;
  transform: translateY(-2px);
  box-shadow: 0 8px 18px rgba(76, 175, 80, 0.32);
}

/* ---------- burger (mobile) ---------- */
.nav__burger {
  display: none;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 5px;
  width: 38px;
  height: 38px;
  border: none;
  border-radius: 50%;
  background: rgba(76, 175, 80, 0.12);
  cursor: pointer;
  flex-shrink: 0;
}

.nav__burger span {
  display: block;
  width: 18px;
  height: 2px;
  border-radius: 2px;
  background: #2f2f2f;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.nav__burger.is-open span:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}
.nav__burger.is-open span:nth-child(2) {
  opacity: 0;
}
.nav__burger.is-open span:nth-child(3) {
  transform: translateY(-7px) rotate(-45deg);
}

/* ---------- fullscreen mobile menu ---------- */
.nav__fullmenu {
  position: fixed;
  inset: 0;
  width: 100vw;
  height: 100dvh;
  z-index: 998;
  background: radial-gradient(
      circle at 85% 12%,
      rgba(76, 175, 80, 0.14),
      transparent 55%
    ),
    rgba(255, 255, 255, 0.98);
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 32px 24px;
  cursor: pointer;
}

.nav__close {
  position: absolute;
  top: clamp(16px, 4vw, 28px);
  left: clamp(16px, 4vw, 28px);
  width: 46px;
  height: 46px;
  border-radius: 50%;
  border: none;
  background: rgba(76, 175, 80, 0.12);
  color: #2f2f2f;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.25s ease, transform 0.2s ease, rotate 0.3s ease;
}
.nav__close svg {
  width: 20px;
  height: 20px;
}
.nav__close:hover {
  background: #4caf50;
  color: #fff;
  rotate: 90deg;
}

.nav__fullmenu-content {
  width: 100%;
  max-width: 420px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: clamp(24px, 5vw, 40px);
  cursor: default;
}

.nav__fullmenu-brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-weight: 800;
  font-size: 1.1rem;
  letter-spacing: 0.06em;
  color: #2f2f2f;
}

.nav__logo--lg {
  width: 40px;
  height: 40px;
}

.nav__fullmenu-links {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  width: 100%;
}

.nav__fullmenu-link {
  font-family: 'Plus Jakarta Sans', sans-serif;
  font-size: clamp(1.6rem, 6vw, 2.1rem);
  font-weight: 700;
  letter-spacing: -0.01em;
  color: #2f2f2f;
  text-decoration: none;
  padding: 10px 0;
  transition: color 0.25s ease, opacity 0.4s ease, transform 0.4s ease;
}

.nav__fullmenu-link:hover {
  color: #4caf50;
}

.nav__fullmenu-link--cart {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: #4caf50;
}

.nav__fullmenu-cta {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin-top: 8px;
  padding: 15px 30px;
  border-radius: 999px;
  background: #4caf50;
  color: #fff;
  font-family: 'Manrope', sans-serif;
  font-size: 1rem;
  font-weight: 700;
  text-decoration: none;
  transition: background 0.25s ease, transform 0.2s ease, box-shadow 0.25s ease,
    opacity 0.4s ease;
}
.nav__fullmenu-cta svg {
  width: 18px;
  height: 18px;
}
.nav__fullmenu-cta:hover {
  background: #43a047;
  transform: translateY(-2px);
  box-shadow: 0 10px 24px rgba(76, 175, 80, 0.32);
}

/* fullscreen menu transition */
.fullmenu-enter-active,
.fullmenu-leave-active {
  transition: opacity 0.35s ease;
}
.fullmenu-enter-from,
.fullmenu-leave-to {
  opacity: 0;
}

.fullmenu-enter-from .nav__fullmenu-link,
.fullmenu-enter-from .nav__fullmenu-cta {
  opacity: 0;
  transform: translateY(16px);
}
.fullmenu-enter-from .nav__close {
  opacity: 0;
  transform: scale(0.7);
}

/* ---------- responsive ---------- */
@media (max-width: 1110px) {
  .nav__links {
    display: none;
  }
  .nav__cta {
    display: none;
  }
  .nav__burger {
    display: flex;
    margin-left: auto;
  }
}

@media (prefers-reduced-motion: reduce) {
  .nav__bar,
  .nav__logo,
  .nav__link::after,
  .nav__cta,
  .nav__burger span,
  .nav__fullmenu-link,
  .nav__fullmenu-cta,
  .nav__close,
  .fullmenu-enter-active,
  .fullmenu-leave-active {
    transition: none !important;
  }
}
</style>