<template>
  <header class="sub-nav-header" role="banner">
    <a :href="homeUrl" class="sub-nav-header__brand">{{ brand }}</a>

    <div class="sub-nav-header__actions">

      <div class="font-controls" role="group" aria-label="Ajustar tamanho da fonte">
        <button type="button" aria-label="Diminuir fonte" @click="decreaseFont">A-</button>
        <button type="button" aria-label="Tamanho de fonte padrão" @click="resetFont">A</button>
        <button type="button" aria-label="Aumentar fonte" @click="increaseFont">A+</button>
      </div>

      <span class="sub-nav-header__divider" aria-hidden="true"></span>

      <button
        type="button"
        class="icon-btn"
        :aria-pressed="highContrast"
        @click="toggleContrast"
      >
        <i class="mdi mdi-circle-half-full contrast-icon" aria-hidden="true"></i>
        ALTO CONTRASTE
      </button>

      <a :href="accessibilityUrl" class="icon-btn">
        <span class="a11y-icon"><i class="mdi mdi-human" aria-hidden="true"></i></span>
        ACESSIBILIDADE
      </a>

      <a :href="siteMapUrl" class="icon-btn">
        <i class="mdi mdi-sitemap" aria-hidden="true"></i>
        MAPA DO SITE
      </a>

    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

/**
 * SubNavHeader
 * Mini header institucional com controles de acessibilidade:
 * tamanho de fonte (A- / A / A+) e alto contraste.
 *
 * Os controles atuam globalmente:
 * - Tamanho de fonte: altera font-size do <html>, então se o resto
 *   do site usa `rem`, tudo escala junto.
 * - Alto contraste: adiciona/remove a classe `high-contrast` no <html>.
 *   Reaproveite essa classe no CSS global do site (ver bloco de
 *   variáveis no <style> abaixo como referência).
 *
 * Ambas as preferências persistem em localStorage.
 */

const props = defineProps({
  brand: {
    type: String,
    default: 'SEDUC-PII.GOV.BR'
  },
  homeUrl: {
    type: String,
    default: '/'
  },
  accessibilityUrl: {
    type: String,
    default: '#acessibilidade'
  },
  siteMapUrl: {
    type: String,
    default: '#mapa-do-site'
  }
})

const STORAGE_FONT_KEY = 'site-font-scale'
const STORAGE_CONTRAST_KEY = 'site-high-contrast'
const STEP = 10
const MIN_SCALE = 80
const MAX_SCALE = 150
const DEFAULT_SCALE = 100

const fontScale = ref(DEFAULT_SCALE)
const highContrast = ref(false)

function applyFontScale() {
  document.documentElement.style.fontSize = fontScale.value + '%'
}

function clampScale(value) {
  return Math.min(MAX_SCALE, Math.max(MIN_SCALE, value))
}

function increaseFont() {
  fontScale.value = clampScale(fontScale.value + STEP)
}

function decreaseFont() {
  fontScale.value = clampScale(fontScale.value - STEP)
}

function resetFont() {
  fontScale.value = DEFAULT_SCALE
}

function toggleContrast() {
  highContrast.value = !highContrast.value
}

watch(fontScale, (value) => {
  applyFontScale()
  localStorage.setItem(STORAGE_FONT_KEY, String(value))
})

watch(highContrast, (value) => {
  document.documentElement.classList.toggle('high-contrast', value)
  localStorage.setItem(STORAGE_CONTRAST_KEY, String(value))
})

onMounted(() => {
  const savedScale = parseInt(localStorage.getItem(STORAGE_FONT_KEY), 10)
  fontScale.value = Number.isFinite(savedScale) ? clampScale(savedScale) : DEFAULT_SCALE
  applyFontScale()

  highContrast.value = localStorage.getItem(STORAGE_CONTRAST_KEY) === 'true'
  document.documentElement.classList.toggle('high-contrast', highContrast.value)
})
</script>

<style scoped>
/* =========================================================
   Tokens locais do componente. As cores de alto contraste
   reagem à classe global .high-contrast no <html> — estenda
   as mesmas variáveis no CSS global do site se quiser que o
   contraste afete outros componentes também.
   ========================================================= */
.sub-nav-header{
  --gov-blue:  #ffffff;
  --gov-yellow: #0064B6;
  --bar-height: 26px;

  width: 100%;
  min-height: var(--bar-height);
  background: var(--gov-blue);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 16px;
  font-size: 11px;
  line-height: 1;
  font-family: Arial, Helvetica, sans-serif;
  transition: background .2s ease;
  box-sizing: border-box;
}

:global(html.high-contrast) .sub-nav-header{
  --gov-blue: #000000;
  --gov-yellow: #0064B6;
  border-bottom: 1px solid #0064B6;
}

.sub-nav-header__brand{
  color: var(--gov-yellow);
  font-weight: 700;
  letter-spacing: .4px;
  text-decoration: none;
  white-space: nowrap;
}

.sub-nav-header__actions{
  display: flex;
  align-items: center;
  gap: 4px;
  white-space: nowrap;
}

.sub-nav-header__divider{
  width: 1px;
  height: 14px;
  background: rgba(255,255,255,.4);
  margin: 0 10px;
}

:global(html.high-contrast) .sub-nav-header__divider{
  background: #0064B6;
}

/* Botões A- A A+ */
.font-controls{
  display: flex;
  align-items: center;
  gap: 10px;
  margin-right: 4px;
}

.font-controls button{
  background: none;
  border: none;
  color: #0064B6;
  font-weight: 700;
  cursor: pointer;
  padding: 4px 2px;
  font-size: 11px;
  font-family: inherit;
}

.font-controls button:nth-child(2){ font-size: 13px; }
.font-controls button:nth-child(3){ font-size: 15px; }

:global(html.high-contrast) .font-controls button{ color: #ffff00; }

.font-controls button:hover,
.font-controls button:focus-visible{
  text-decoration: underline;
}

/* Botões com ícone */
.icon-btn{
  display: flex;
  align-items: center;
  gap: 6px;
  background: none;
  border: none;
  color: #0064B6;
  font-weight: 700;
  font-family: inherit;
  font-size: 11px;
  letter-spacing: .3px;
  cursor: pointer;
  padding: 4px 2px;
  text-decoration: none;
  white-space: nowrap;
}

:global(html.high-contrast) .icon-btn{ color: #0064B6; }

.icon-btn:hover,
.icon-btn:focus-visible{
  text-decoration: underline;
}

.icon-btn .mdi{
  font-size: 15px;
}

.a11y-icon{
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  border: 1.4px solid currentColor;
}

.a11y-icon .mdi{
  font-size: 11px;
  line-height: 1;
}

.contrast-icon{
  font-size: 15px !important;
}

.sub-nav-header :focus-visible{
  outline: 2px solid var(--gov-yellow);
  outline-offset: 2px;
}

/* =========================================================
   Mobile: só sobra a linha azul, sem conteúdo
   ========================================================= */
@media (max-width: 768px){
  .sub-nav-header{
    min-height: 6px;
    padding: 0;
  }
  .sub-nav-header__brand,
  .sub-nav-header__actions{
    display: none;
  }
}
</style>