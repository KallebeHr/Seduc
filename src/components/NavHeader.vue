<template>
  <header class="main-nav-header">

   <!-- <SubNavHeader /> -->
    <div class="top-bar">
      <div class="top-bar__inner" :class="{ 'top-bar__inner--search-open': mobileSearchOpen }">

        <!-- Logo (a própria imagem já traz o lockup completo) -->
        <a :href="homeUrl" class="brand">
          <img :src="logoSrc" :alt="logoAlt" class="brand__logo-img" />
        </a>

        <!-- Busca desktop -->
        <form class="search search--desktop" role="search" @submit.prevent="submitSearch">
          <i class="mdi mdi-magnify search--desktop__icon" aria-hidden="true"></i>
          <label for="search-desktop" class="sr-only">{{ searchPlaceholder }}</label>
          <input
            id="search-desktop"
            v-model="searchQuery"
            type="search"
            :placeholder="searchPlaceholder"
          />
          <button type="submit" class="search__btn">Buscar</button>
        </form>

        <!-- Ícones mobile: lupa + hamburguer -->
        <div class="mobile-actions">
          <button
            type="button"
            class="icon-round"
            aria-label="Abrir busca"
            @click="openMobileSearch"
          >
            <i class="mdi mdi-magnify" aria-hidden="true"></i>
          </button>
          <button
            type="button"
            class="icon-round"
            aria-label="Abrir menu"
            @click="openMobileMenu"
          >
            <i class="mdi mdi-menu" aria-hidden="true"></i>
          </button>
        </div>

        <!-- Busca mobile expandida -->
        <form
          v-if="mobileSearchOpen"
          class="search search--mobile"
          role="search"
          @submit.prevent="submitSearch"
        >
          <i class="mdi mdi-magnify search--mobile__icon" aria-hidden="true"></i>
          <input
            ref="mobileSearchInput"
            v-model="searchQuery"
            type="search"
            :placeholder="searchPlaceholder"
          />
          <button
            type="button"
            class="icon-round icon-round--ghost"
            aria-label="Fechar busca"
            @click="closeMobileSearch"
          >
            <i class="mdi mdi-close" aria-hidden="true"></i>
          </button>
        </form>

      </div>
    </div>

    <!-- ======================= NAVEGAÇÃO (desktop) ======================= -->
    <nav class="nav-bar" aria-label="Navegação principal">
      <ul class="nav-bar__list">
        <li v-for="item in navItems" :key="item.label" class="nav-bar__item">

          <a v-if="!item.children" :href="item.href" class="nav-bar__link">
            <i v-if="item.icon" class="mdi" :class="item.icon" :style="item.iconColor ? { color: item.iconColor } : null" aria-hidden="true"></i>
            {{ item.label }}
          </a>

          <template v-else>
            <button
              type="button"
              class="nav-bar__link nav-bar__link--dropdown"
              :aria-expanded="openDropdown === item.label"
              aria-haspopup="true"
              @click="toggleDropdown(item.label)"
            >
              {{ item.label }}
              <i class="mdi mdi-chevron-down nav-bar__chevron" :class="{ 'is-open': openDropdown === item.label }" aria-hidden="true"></i>
            </button>

            <transition name="dropdown">
              <ul v-if="openDropdown === item.label" class="dropdown-panel" role="menu">
                <li v-for="child in item.children" :key="child.label" role="none">
                  <a :href="child.href" role="menuitem" @click="openDropdown = null">{{ child.label }}</a>
                </li>
              </ul>
            </transition>
          </template>

        </li>
      </ul>
    </nav>

    <!-- ======================= MENU MOBILE FULLSCREEN ======================= -->
    <Teleport to="body">
      <transition name="fullscreen">
        <div
          v-if="mobileMenuOpen"
          class="mobile-menu"
          role="dialog"
          aria-modal="true"
          aria-label="Menu de navegação"
        >
          <div class="mobile-menu__header">
            <span class="brand brand--mobile-menu">
              <img :src="logoSrc" :alt="logoAlt" class="brand__logo-img brand__logo-img--menu" />
            </span>
            <button
              ref="closeMenuBtn"
              type="button"
              class="icon-round icon-round--ghost icon-round--close"
              aria-label="Fechar menu"
              @click="closeMobileMenu"
            >
              <i class="mdi mdi-close" aria-hidden="true"></i>
            </button>
          </div>

          <ul class="mobile-menu__list">
            <li
              v-for="(item, index) in navItems"
              :key="item.label"
              class="mobile-menu__item"
              :style="{ transitionDelay: (index * 60) + 'ms' }"
            >
              <a
                v-if="!item.children"
                :href="item.href"
                class="mobile-menu__link"
                @click="closeMobileMenu"
              >
                <i v-if="item.icon" class="mdi mobile-menu__icon" :class="item.icon" :style="item.iconColor ? { color: item.iconColor } : null" aria-hidden="true"></i>
                <i v-else class="mdi mdi-circle-small mobile-menu__icon" aria-hidden="true"></i>
                <span>{{ item.label }}</span>
                <i class="mdi mdi-chevron-right mobile-menu__arrow" aria-hidden="true"></i>
              </a>

              <template v-else>
                <button
                  type="button"
                  class="mobile-menu__link"
                  :aria-expanded="openMobileAccordion === item.label"
                  @click="toggleMobileAccordion(item.label)"
                >
                  <i v-if="item.icon" class="mdi mobile-menu__icon" :class="item.icon" aria-hidden="true"></i>
                  <span>{{ item.label }}</span>
                  <i
                    class="mdi mdi-chevron-down mobile-menu__arrow"
                    :class="{ 'is-open': openMobileAccordion === item.label }"
                    aria-hidden="true"
                  ></i>
                </button>

                <transition name="accordion">
                  <ul v-if="openMobileAccordion === item.label" class="mobile-menu__submenu">
                    <li v-for="child in item.children" :key="child.label">
                      <a :href="child.href" @click="closeMobileMenu">{{ child.label }}</a>
                    </li>
                  </ul>
                </transition>
              </template>
            </li>
          </ul>

          <div v-if="footerText" class="mobile-menu__footer">
            <span>{{ footerText }}</span>
          </div>
        </div>
      </transition>
    </Teleport>

  </header>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue'
import SubNavHeader from './SubNavHeader.vue'

/**
 * MainNavHeader
 * Header institucional completo: barra com logo + busca, e navegação
 * secundária em teal. No mobile, colapsa para logo + ícones de busca
 * e menu; o menu abre em tela cheia e a busca expande na própria barra.
 */

const props = defineProps({
  homeUrl: { type: String, default: '/' },
  searchPlaceholder: { type: String, default: 'O que você procura ...' },
  /**
   * Caminho do arquivo da sua logo (SVG, PNG etc). A logo já deve trazer
   * o lockup completo (brasão + nome), como no site original — o
   * componente não desenha texto ao lado dela. A barra azul agora não
   * tem padding vertical: a altura dela é ditada pela própria logo
   * (ver .brand__logo-img no <style>), então a imagem preenche 100%
   * do espaço de cima a baixo, sem margem "fantasma".
   */
  logoSrc: { type: String, default: '/IMG/semedBranco.svg' },
  logoAlt: { type: String, default: 'Governo do Estado de Goiás' },
  /** Texto opcional no rodapé do menu mobile fullscreen. Deixe vazio para omitir. */
  footerText: { type: String, default: '' },
  navItems: {
    type: Array,
    default: () => ([
      { label: 'INSTITUCIONAL', href: '#institucional', icon: 'mdi-domain' },
      { label: 'CARTA DE SERVIÇOS', href: '#carta-de-servicos', icon: 'mdi-file-document-outline' },
      {
        label: 'ACESSO RÁPIDO',
        icon: 'mdi-lightning-bolt-outline',
        children: [
          { label: 'Sistemas', href: '#sistemas' },
          { label: 'Editais', href: '#editais' },
          { label: 'Legislação', href: '#legislacao' }
        ]
      },
      {
        label: 'GOVERNANÇA',
        icon: 'mdi-scale-balance',
        children: [
          { label: 'Planejamento', href: '#planejamento' },
          { label: 'Transparência', href: '#transparencia' },
          { label: 'Indicadores', href: '#indicadores' }
        ]
      },
      { label: 'ACESSO À INFORMAÇÃO', href: '#acesso-a-informacao', icon: 'mdi-information', iconColor: '#F5B301' },
      { label: 'FALE CONOSCO', href: '#fale-conosco', icon: 'mdi-message-text-outline' }
    ])
  }
})

const emit = defineEmits(['search'])

const searchQuery = ref('')
const mobileSearchOpen = ref(false)
const mobileMenuOpen = ref(false)
const openDropdown = ref(null)
const openMobileAccordion = ref(null)

const mobileSearchInput = ref(null)
const closeMenuBtn = ref(null)

function submitSearch() {
  emit('search', searchQuery.value)
  closeMobileSearch()
}

async function openMobileSearch() {
  mobileSearchOpen.value = true
  await nextTick()
  // pequeno atraso evita o "salto"/zoom do Safari iOS ao focar
  // um input logo após uma mudança de layout
  setTimeout(() => mobileSearchInput.value?.focus(), 50)
}

function closeMobileSearch() {
  mobileSearchOpen.value = false
  searchQuery.value = ''
}

async function openMobileMenu() {
  mobileMenuOpen.value = true
  document.body.style.overflow = 'hidden'
  await nextTick()
  closeMenuBtn.value?.focus()
}

function closeMobileMenu() {
  mobileMenuOpen.value = false
  openMobileAccordion.value = null
  document.body.style.overflow = ''
}

function toggleDropdown(label) {
  openDropdown.value = openDropdown.value === label ? null : label
}

function toggleMobileAccordion(label) {
  openMobileAccordion.value = openMobileAccordion.value === label ? null : label
}

function handleOutsideClick(event) {
  if (openDropdown.value && !event.target.closest('.nav-bar__item')) {
    openDropdown.value = null
  }
}

function handleKeydown(event) {
  if (event.key !== 'Escape') return
  if (mobileMenuOpen.value) closeMobileMenu()
  else if (mobileSearchOpen.value) closeMobileSearch()
  else if (openDropdown.value) openDropdown.value = null
}

onMounted(() => {
  window.addEventListener('click', handleOutsideClick)
  window.addEventListener('keydown', handleKeydown)
})

onBeforeUnmount(() => {
  window.removeEventListener('click', handleOutsideClick)
  window.removeEventListener('keydown', handleKeydown)
  document.body.style.overflow = ''
})
</script>

<style>
/* Fonte: import global (fora do escopo do componente) */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&display=swap');
</style>

<style scoped>
.main-nav-header{
  --teal: #037770;
  --teal-dark: #025c56;
  --gold: #F5B301;
  --blue: #0064B6;
  --blue-dark: #00508f;
  --ink: #17332f;

  font-family: 'Poppins', Arial, Helvetica, sans-serif;
  position: relative;
  z-index: 40;
  max-width: 100vw;
  overflow-x: hidden;
}

.sr-only{
  position:absolute; width:1px; height:1px; padding:0; margin:-1px;
  overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0;
}

*, *::before, *::after{
  box-sizing: border-box;
}

/* ============================= TOP BAR ============================= */
.top-bar{
  background: #0064B6;
  /* Sem padding vertical de propósito: a altura da barra passa a ser
     ditada pela própria logo (.brand__logo-img), então ao aumentar a
     altura da imagem ela ocupa 100% do espaço, sem sobra em cima/embaixo. */
  padding: 0 24px;
  width: 100%;
  overflow: hidden;
}

.top-bar__inner{
  max-width: 1440px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 24px;
  width: 100%;
  min-width: 0;
}

/* Quando a busca mobile está aberta, a logo e os ícones somem de
   verdade do fluxo do flex (display:none), em vez de só ficarem
   invisíveis — assim o form de busca pode ocupar 100% da largura
   sem disputar espaço com elementos "fantasmas" ainda no layout. */
.top-bar__inner--search-open .brand,
.top-bar__inner--search-open .mobile-actions{
  display: none;
}

.brand{
  display: flex;
  align-items: center;
  gap: 14px;
  text-decoration: none;
  flex-shrink: 0;
  min-width: 0;
  /* a logo é o elemento mais alto da barra: como o pai não tem padding
     vertical, ela dita a altura visual total da .top-bar */
}

.brand__logo-img{
  display: block;
  height: 96px;
  width: auto;
  max-width: 340px;
  object-fit: contain;
  margin: 0;
  padding: 0;
}

/* Busca desktop — pill unificado com ícone embutido (tamanho independente da logo) */
.search--desktop{
  margin-left: auto;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  background: #fff;
  border: 1.5px solid #e3e9e8;
  border-radius: 999px;
  padding: 4px 4px 4px 18px;
  transition: border-color .18s ease, box-shadow .18s ease;
  max-width: 100%;
}
.search--desktop:focus-within{
  border-color: var(--teal);
  box-shadow: 0 0 0 4px rgba(3, 119, 112, .10);
}
.search--desktop__icon{
  font-size: 18px;
  color: #93a09f;
  flex-shrink: 0;
  transition: color .18s ease;
}
.search--desktop:focus-within .search--desktop__icon{ color: var(--teal); }

.search--desktop input{
  width: 280px;
  min-width: 0;
  border: none;
  background: transparent;
  padding: 11px 12px;
  font-size: 14px;
  font-family: inherit;
  color: var(--ink);
  outline: none;
}
.search--desktop input::placeholder{ color: #98a3a2; }

.search__btn{
  background: var(--blue);
  color: #fff;
  border: none;
  border-radius: 999px;
  padding: 0 26px;
  height: 42px;
  font-weight: 700;
  font-size: 14px;
  font-family: inherit;
  cursor: pointer;
  flex-shrink: 0;
  transition: background .15s ease, transform .1s ease;
}
.search__btn:hover{ background: var(--blue-dark); }
.search__btn:active{ transform: scale(.97); }

/* Ícones mobile (escondidos no desktop) */
.mobile-actions{
  display: none;
  align-items: center;
  gap: 6px;
  margin-left: auto;
  flex-shrink: 0;
}

.icon-round{
  width: 40px;
  height: 40px;
  min-width: 40px;
  border-radius: 50%;
  border: none;
  background: #f0f4f3;
  color: var(--teal);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  cursor: pointer;
  transition: background .15s ease, color .15s ease, transform .15s ease;
}
.icon-round:hover{ background: var(--teal); color: #fff; }
.icon-round--ghost{ background: transparent; color: var(--ink); }
.icon-round--ghost:hover{ background: rgba(0,0,0,.06); color: var(--ink); }

/* Busca mobile expandida dentro da top bar */
.search--mobile{
  display: flex;
  align-items: center;
  gap: 8px;
  width: 100%;
  min-width: 0;
  background: #f4f7f6;
  border: 1.5px solid transparent;
  border-radius: 999px;
  padding: 4px 6px 4px 16px;
  animation: expand .18s ease;
}
.search--mobile:focus-within{
  background: #fff;
  border-color: var(--teal);
}
.search--mobile__icon{
  font-size: 19px;
  color: var(--teal);
  flex-shrink: 0;
}
.search--mobile input{
  flex: 1;
  min-width: 0;
  border: none;
  padding: 10px 4px;
  /* 16px é o mínimo para o Safari/iOS NÃO dar zoom automático da
     página inteira ao focar o campo — era essa a causa da tela
     "quebrar" ao abrir a lupa no mobile. */
  font-size: 16px;
  font-family: inherit;
  background: transparent;
  outline: none;
}

@keyframes expand{
  from{ opacity: 0; transform: translateY(-4px); }
  to{ opacity: 1; transform: translateY(0); }
}

/* ============================= NAV BAR (desktop) ============================= */
.nav-bar{
  background: var(--teal);
  width: 100%;
  overflow: hidden;
}
.nav-bar__list{
  max-width: 1440px;
  margin: 0 auto;
  list-style: none;
  display: flex;
  align-items: stretch;
  padding: 0 24px;
  gap: 4px;
  flex-wrap: wrap;
}
.nav-bar__item{
  position: relative;
}
.nav-bar__link{
  display: flex;
  align-items: center;
  gap: 8px;
  height: 52px;
  padding: 0 14px;
  color: #fff;
  text-decoration: none;
  font-size: 13.5px;
  font-weight: 600;
  letter-spacing: .2px;
  background: none;
  border: none;
  font-family: inherit;
  cursor: pointer;
  position: relative;
}
.nav-bar__link .mdi{ font-size: 17px; }
.nav-bar__link::after{
  content: '';
  position: absolute;
  left: 14px;
  right: 14px;
  bottom: 8px;
  height: 2px;
  background: var(--gold);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform .2s ease;
}
.nav-bar__link:hover::after,
.nav-bar__link:focus-visible::after{
  transform: scaleX(1);
}
.nav-bar__link:hover,
.nav-bar__link:focus-visible{
  background: rgba(255,255,255,.06);
}

.nav-bar__chevron{
  font-size: 16px !important;
  transition: transform .2s ease;
}
.nav-bar__chevron.is-open{ transform: rotate(180deg); }

.dropdown-panel{
  position: absolute;
  top: 100%;
  left: 8px;
  min-width: 200px;
  background: #fff;
  border-radius: 0 0 8px 8px;
  box-shadow: 0 12px 24px rgba(0,0,0,.15);
  list-style: none;
  padding: 6px;
  margin: 0;
  z-index: 50;
}
.dropdown-panel a{
  display: block;
  padding: 10px 12px;
  color: var(--ink);
  text-decoration: none;
  font-size: 13.5px;
  font-weight: 500;
  border-radius: 6px;
}
.dropdown-panel a:hover{
  background: #eef6f5;
  color: var(--teal);
}

.dropdown-enter-active, .dropdown-leave-active{ transition: opacity .15s ease, transform .15s ease; }
.dropdown-enter-from, .dropdown-leave-to{ opacity: 0; transform: translateY(-6px); }

/* ============================= MOBILE MENU FULLSCREEN ============================= */
.mobile-menu{
  /* Redefinido aqui porque o Teleport move este elemento para fora
     de .main-nav-header, então ele não herda mais as variáveis do pai */
  --teal: #037770;
  --teal-dark: #025c56;
  --gold: #F5B301;

  position: fixed;
  inset: 0;
  z-index: 1000;
  background: linear-gradient(160deg, var(--teal) 0%, var(--teal-dark) 100%);
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  font-family: 'Poppins', Arial, Helvetica, sans-serif;
}

.mobile-menu__header{
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 20px 8px;
}
.brand--mobile-menu{
  border-radius: 12px;
  padding: 6px 14px;
  display: inline-flex;
}
.brand__logo-img--menu{
  height: 34px;
}

.icon-round--close{
  background: rgba(255,255,255,.12);
  color: #fff;
  width: 44px;
  height: 44px;
  min-width: 44px;
  font-size: 22px;
}
.icon-round--close:hover{ background: rgba(255,255,255,.25); color: #fff; }

.mobile-menu__list{
  list-style: none;
  margin: 12px 0 0;
  padding: 0 12px;
  flex: 1;
}

.mobile-menu__item{
  border-bottom: 1px solid rgba(255,255,255,.12);
}

.mobile-menu__link{
  width: 100%;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 18px 8px;
  background: none;
  border: none;
  color: #fff;
  text-decoration: none;
  font-size: 18px;
  font-weight: 600;
  font-family: inherit;
  cursor: pointer;
  text-align: left;
}
.mobile-menu__icon{
  font-size: 22px;
  color: var(--gold);
  width: 26px;
  flex-shrink: 0;
}
.mobile-menu__arrow{
  margin-left: auto;
  font-size: 20px;
  color: rgba(255,255,255,.6);
  transition: transform .2s ease;
}
.mobile-menu__arrow.is-open{ transform: rotate(180deg); }

.mobile-menu__submenu{
  list-style: none;
  margin: 0 0 12px 42px;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.mobile-menu__submenu a{
  display: block;
  padding: 10px 8px;
  color: rgba(255,255,255,.85);
  text-decoration: none;
  font-size: 15px;
  font-weight: 500;
  border-left: 2px solid var(--gold);
  padding-left: 14px;
}
.mobile-menu__submenu a:hover{ color: var(--gold); }

.accordion-enter-active, .accordion-leave-active{
  transition: max-height .25s ease, opacity .2s ease;
  overflow: hidden;
}
.accordion-enter-from, .accordion-leave-to{ max-height: 0; opacity: 0; }
.accordion-enter-to, .accordion-leave-from{ max-height: 400px; opacity: 1; }

.mobile-menu__footer{
  padding: 20px 24px 32px;
  color: rgba(255,255,255,.55);
  font-size: 12px;
  font-weight: 600;
  letter-spacing: .3px;
  text-align: center;
}

.fullscreen-enter-active{ transition: opacity .25s ease; }
.fullscreen-leave-active{ transition: opacity .2s ease; }
.fullscreen-enter-from, .fullscreen-leave-to{ opacity: 0; }

/* ============================= RESPONSIVO ============================= */
@media (max-width: 900px){
  .search--desktop{ width: 240px; }
  .search--desktop input{ width: 180px; }
}

@media (max-width: 1108px){
  .top-bar{ padding: 0 16px; }
  .top-bar__inner{ gap: 12px; }

  .brand__logo-img{ height: 52px; max-width: 200px; }

  .search--desktop{ display: none; }

  .mobile-actions{ display: flex; }

  .nav-bar{ display: none; }
}

@media (max-width: 380px){
  .top-bar{ padding: 0 12px; }
  .brand__logo-img{ height: 44px; max-width: 160px; }
  .icon-round{ width: 36px; height: 36px; min-width: 36px; font-size: 18px; }
}
</style>