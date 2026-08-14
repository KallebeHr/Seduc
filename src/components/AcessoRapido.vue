<template>
  <section class="acesso-rapido" aria-labelledby="acesso-rapido-titulo" ref="sectionEl">
    <div class="acesso-rapido__inner">

      <div class="head">
        <p class="eyebrow"><i class="mdi mdi-view-grid-outline" aria-hidden="true"></i>Acesso rápido</p>
        <h2 id="acesso-rapido-titulo" class="title">{{ titulo }}</h2>
        <p class="subtitle">{{ subtitulo }}</p>
      </div>

      <div class="bento">

        <!-- Tile em destaque -->
        <a
          :href="destaque.href"
          class="tile tile--feature"
          data-reveal
          style="--delay: 0ms"
        >
          <svg class="tile__selo" viewBox="0 0 200 200" aria-hidden="true">
            <circle cx="100" cy="100" r="92" fill="none" stroke="currentColor" stroke-width="1.2" stroke-dasharray="2 6"/>
            <circle cx="100" cy="100" r="70" fill="none" stroke="currentColor" stroke-width="1"/>
          </svg>

          <span v-if="destaque.badge" class="tile__badge">{{ destaque.badge }}</span>

          <div class="tile__icon tile__icon--lg">
            <i class="mdi" :class="destaque.icone" aria-hidden="true"></i>
          </div>

          <div class="tile__text">
            <h3>{{ destaque.titulo }}</h3>
            <p>{{ destaque.descricao }}</p>
          </div>

          <span class="tile__cta">
            {{ destaque.acao || 'Acessar' }}
            <i class="mdi mdi-arrow-right" aria-hidden="true"></i>
          </span>
        </a>

        <!-- Tiles secundários -->
        <a
          v-for="(item, index) in itens"
          :key="item.id"
          :href="item.href"
          class="tile"
          :class="[`tile--slot-${index}`, { 'tile--wide': item.wide, 'tile--solid': item.solido }]"
          data-reveal
          :style="{ '--delay': `${(index + 1) * 70}ms` }"
        >
          <span v-if="item.badge" class="tile__badge tile__badge--sm">{{ item.badge }}</span>

          <div class="tile__icon">
            <i class="mdi" :class="item.icone" aria-hidden="true"></i>
          </div>

          <div class="tile__text">
            <h3>{{ item.titulo }}</h3>
            <p v-if="item.wide">{{ item.descricao }}</p>
          </div>

          <i class="mdi mdi-arrow-right tile__arrow" aria-hidden="true"></i>
        </a>

        <!-- Tile CTA -->
        <a :href="cta.href" class="tile tile--cta" data-reveal style="--delay: 420ms">
          <i class="mdi" :class="cta.icone || 'mdi-dots-grid'" aria-hidden="true"></i>
          <span>{{ cta.titulo }}</span>
        </a>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

/**
 * AcessoRapido (bento)
 * Painel de acesso rápido em grade assimétrica ("bento"), com um tile
 * em destaque (selo decorativo), tiles sólidos e um cartão de saída
 * para o catálogo completo de serviços. Revela os tiles com um leve
 * fade/slide ao entrar na viewport (respeita prefers-reduced-motion).
 */

const props = defineProps({
  titulo: { type: String, default: 'Tudo o que você precisa, organizado num só lugar' },
  subtitulo: { type: String, default: 'Os serviços mais usados por alunos, famílias e servidores da rede municipal de ensino.' },

  destaque: {
    type: Object,
    default: () => ({
      titulo: 'Matrícula online 2027',
      descricao: 'Matricule ou renove a matrícula do aluno na rede municipal em poucos minutos, sem sair de casa.',
      href: '/matricula',
      icone: 'mdi-account-plus-outline',
      acao: 'Matricular agora',
      badge: 'Prazo aberto'
    })
  },

  itens: {
    type: Array,
    // { id, titulo, descricao, href, icone, badge, solido, wide }
    default: () => ([
      {
        id: 'diario',
        titulo: 'Diário de classe',
        href: '/diario-online',
        icone: 'mdi-notebook-outline',
        solido: true
      },
      {
        id: 'calendario',
        titulo: 'Calendário escolar',
        href: '/calendario',
        icone: 'mdi-calendar-month-outline'
      },
      {
        id: 'editais',
        titulo: 'Editais e concursos',
        href: '/editais',
        icone: 'mdi-certificate-outline',
        badge: '3 novos'
      },
      {
        id: 'transparencia',
        titulo: 'Transparência',
        href: '/transparencia',
        icone: 'mdi-scale-balance',
        solido: true
      },
      {
        id: 'contato',
        titulo: 'Fale conosco',
        descricao: 'Ouvidoria, dúvidas e solicitações direto com a Secretaria de Educação.',
        href: '/fale-conosco',
        icone: 'mdi-message-text-outline',
        wide: true
      }
    ])
  },

  cta: {
    type: Object,
    default: () => ({
      titulo: 'Ver todos os serviços',
      href: '/carta-de-servicos',
      icone: 'mdi-dots-grid'
    })
  }
})

const sectionEl = ref(null)
let observer = null

onMounted(() => {
  const nodes = sectionEl.value?.querySelectorAll('[data-reveal]')
  if (!nodes || !nodes.length) return

  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (prefersReducedMotion) {
    nodes.forEach((n) => n.classList.add('is-visible'))
    return
  }

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.15 }
  )

  nodes.forEach((n) => observer.observe(n))
})

onBeforeUnmount(() => {
  observer?.disconnect()
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap');
</style>

<style scoped>
.acesso-rapido{
  --teal: #037770;
  --teal-dark: #025c56;
  --gold: #F5B301;
  --blue: #0064B6;
  --blue-dark: #00508f;
  --ink: #17332f;
  --muted: #5c706d;
  --border: #e6ece9;

  background: #fbfdfc;
  font-family: 'Poppins', Arial, Helvetica, sans-serif;
  padding: 72px 24px 80px;
}

.acesso-rapido__inner{
  max-width: 1440px;
  margin: 0 auto;
}

.head{
  max-width: 640px;
  margin: 0 0 40px;
}
.eyebrow{
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: .4px;
  text-transform: uppercase;
  color: var(--teal);
  margin: 0 0 12px;
}
.eyebrow .mdi{ font-size: 16px; color: var(--gold); }
.title{
  font-weight: 800;
  font-size: 30px;
  line-height: 1.3;
  color: var(--ink);
  margin: 0 0 10px;
}
.subtitle{
  font-size: 15px;
  color: var(--muted);
  line-height: 1.65;
  margin: 0;
}

/* ============================= BENTO GRID ============================= */
.bento{
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: 180px 180px minmax(130px, auto);
  grid-template-areas:
    "feature feature b c"
    "feature feature d e"
    "f f f cta";
  gap: 18px;
}

.tile--feature{ grid-area: feature; }
.tile--slot-0{ grid-area: b; }
.tile--slot-1{ grid-area: c; }
.tile--slot-2{ grid-area: d; }
.tile--slot-3{ grid-area: e; }
.tile--slot-4{ grid-area: f; }
.tile--cta{ grid-area: cta; }

/* ---- base do tile ---- */
.tile{
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 6px;
  padding: 22px;
  border-radius: 18px;
  text-decoration: none;
  color: inherit;
  background: #fff;
  border: 1px solid var(--border);
  overflow: hidden;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity .5s ease, transform .5s ease, box-shadow .2s ease, border-color .2s ease;
}
.tile.is-visible{
  opacity: 1;
  transform: translateY(0);
  transition-delay: var(--delay, 0ms);
}
@media (prefers-reduced-motion: reduce){
  .tile{ transition: box-shadow .2s ease, border-color .2s ease; opacity: 1; transform: none; }
}

.tile:hover,
.tile:focus-visible{
  box-shadow: 0 16px 32px rgba(3, 119, 112, .12);
  border-color: var(--teal);
  outline: none;
}

.tile__icon{
  width: 46px;
  height: 46px;
  border-radius: 12px;
  background: #eef6f5;
  color: var(--teal);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  flex-shrink: 0;
  transition: transform .25s ease;
}
.tile:hover .tile__icon{ transform: translateY(-3px); }

.tile h3{
  font-size: 15.5px;
  font-weight: 700;
  color: var(--ink);
  margin: 0;
  line-height: 1.35;
}
.tile p{
  font-size: 13px;
  color: var(--muted);
  line-height: 1.55;
  margin: 4px 0 0;
}

.tile__arrow{
  position: absolute;
  top: 20px;
  right: 20px;
  font-size: 18px;
  color: var(--border);
  transition: color .2s ease, transform .2s ease;
}
.tile:hover .tile__arrow{
  color: var(--teal);
  transform: translateX(3px);
}

.tile__badge{
  align-self: flex-start;
  background: var(--gold);
  color: #4a3300;
  font-size: 11.5px;
  font-weight: 700;
  letter-spacing: .2px;
  padding: 4px 12px;
  border-radius: 999px;
  margin-bottom: auto;
}
.tile__badge--sm{
  position: absolute;
  top: 16px;
  right: 16px;
  margin: 0;
  font-size: 10.5px;
  padding: 3px 9px;
}

/* ---- tile em destaque ---- */
.tile--feature{
  background: linear-gradient(155deg, var(--teal) 0%, var(--teal-dark) 100%);
  color: #fff;
  padding: 28px;
  justify-content: space-between;
}
.tile--feature h3{ color: #fff; font-size: 21px; font-weight: 800; }
.tile--feature p{ color: rgba(255,255,255,.78); font-size: 13.5px; max-width: 320px; }
.tile--feature .tile__badge{ background: var(--gold); color: #4a3300; }

.tile__selo{
  position: absolute;
  right: -30px;
  bottom: -30px;
  width: 200px;
  height: 200px;
  color: rgba(255,255,255,.16);
  pointer-events: none;
}

.tile__icon--lg{
  width: 56px;
  height: 56px;
  border-radius: 14px;
  background: rgba(255,255,255,.14);
  color: #fff;
  font-size: 26px;
  margin-top: 12px;
}

.tile__cta{
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 14px;
  font-weight: 700;
  color: #fff;
  margin-top: 18px;
}
.tile__cta .mdi{ font-size: 17px; transition: transform .2s ease; }
.tile--feature:hover .tile__cta .mdi{ transform: translateX(4px); }

/* ---- tiles sólidos (cor cheia, sem branco) ---- */
.tile--solid{
  background: var(--blue);
  border-color: var(--blue);
  color: #fff;
}
.tile--solid h3{ color: #fff; }
.tile--solid .tile__icon{
  background: rgba(255,255,255,.16);
  color: #fff;
}
.tile--solid .tile__arrow{ color: rgba(255,255,255,.5); }
.tile--solid:hover{ box-shadow: 0 16px 32px rgba(0, 100, 182, .22); border-color: var(--blue-dark); }
.tile--solid:hover .tile__arrow{ color: #fff; }

/* ---- tile largo (fale conosco) ---- */
.tile--wide{
  flex-direction: row;
  align-items: center;
  gap: 18px;
  justify-content: flex-start;
}
.tile--wide .tile__text{ flex: 1; }
.tile--wide .tile__arrow{ position: static; margin-left: auto; }

/* ---- tile CTA (ver todos os serviços) ---- */
.tile--cta{
  align-items: center;
  justify-content: center;
  flex-direction: row;
  gap: 10px;
  background: #fff;
  border: 1.5px dashed var(--border);
  color: var(--teal);
  font-weight: 700;
  font-size: 14px;
  text-align: center;
}
.tile--cta .mdi{ font-size: 19px; }
.tile--cta:hover{
  border-style: solid;
  border-color: var(--teal);
  background: #f2faf9;
}

/* ============================= RESPONSIVO ============================= */
@media (max-width: 1108px){
  .acesso-rapido{ padding: 56px 16px 64px; }
  .title{ font-size: 25px; }

  .bento{
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 200px repeat(4, auto) 64px;
    grid-template-areas:
      "feature feature"
      "b c"
      "d e"
      "f f"
      "cta cta";
  }
  .tile--wide{ flex-direction: column; align-items: flex-start; }
  .tile--wide .tile__arrow{ position: absolute; top: 20px; right: 20px; margin-left: 0; }
}

@media (max-width: 640px){
  .bento{
    grid-template-columns: 1fr;
    grid-template-rows: 220px repeat(5, auto);
    grid-template-areas:
      "feature"
      "b" "c" "d" "e" "f"
      "cta";
  }
  .tile--feature p{ max-width: none; }
}
</style>