<template>
  <section class="novidades" aria-labelledby="novidades-titulo">
    <div class="novidades__inner">

      <div class="novidades__head">
        <div class="novidades__heading">
          <p class="eyebrow"><i class="mdi mdi-lightning-bolt-outline" aria-hidden="true"></i>Novidades</p>
          <h2 id="novidades-titulo" class="title">{{ titulo }}</h2>
        </div>

        <div class="novidades__controls">
          <button
            ref="prevBtn"
            type="button"
            class="icon-round"
            :class="{ 'is-disabled': isBeginning }"
            aria-label="Notícia anterior"
          >
            <i class="mdi mdi-chevron-left" aria-hidden="true"></i>
          </button>
          <button
            ref="nextBtn"
            type="button"
            class="icon-round"
            :class="{ 'is-disabled': isEnd }"
            aria-label="Próxima notícia"
          >
            <i class="mdi mdi-chevron-right" aria-hidden="true"></i>
          </button>
        </div>
      </div>

      <Swiper
        class="novidades__swiper"
        :modules="modules"
        :slides-per-view="1.08"
        :space-between="20"
        :speed="500"
        :a11y="{ prevSlideMessage: 'Notícia anterior', nextSlideMessage: 'Próxima notícia' }"
        :keyboard="{ enabled: true }"
        :navigation="{ prevEl: prevBtn, nextEl: nextBtn }"
        :pagination="{ el: paginationEl, clickable: true }"
        :autoplay="autoplay ? { delay: autoplayDelay, disableOnInteraction: true } : false"
        :breakpoints="{
          640:  { slidesPerView: 1.6,  spaceBetween: 20 },
          900:  { slidesPerView: 2.3,  spaceBetween: 24 },
          1108: { slidesPerView: 3,    spaceBetween: 28 }
        }"
        @swiper="onSwiper"
        @slide-change="onSlideChange"
      >
        <SwiperSlide v-for="item in noticias" :key="item.id">
          <a :href="item.href" class="card">
            <div class="card__media">
              <img :src="item.imagem" :alt="item.imagemAlt || item.titulo" loading="lazy" />
              <span class="card__data">
                <strong>{{ item.dia }}</strong>
                <span>{{ item.mes }}</span>
              </span>
              <span v-if="item.categoria" class="card__categoria">{{ item.categoria }}</span>
            </div>
            <div class="card__body">
              <h3>{{ item.titulo }}</h3>
              <p>{{ item.resumo }}</p>
              <span class="card__link">
                Leia mais
                <i class="mdi mdi-arrow-right" aria-hidden="true"></i>
              </span>
            </div>
          </a>
        </SwiperSlide>
      </Swiper>

      <div ref="paginationEl" class="novidades__pagination" role="tablist" aria-label="Selecionar notícia"></div>

    </div>
  </section>
</template>

<script setup>
import { ref, shallowRef } from 'vue'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation, Pagination, Autoplay, A11y, Keyboard } from 'swiper/modules'

import 'swiper/css'
import 'swiper/css/navigation'
import 'swiper/css/pagination'

/**
 * NovidadesCarousel
 * Carrossel institucional de notícias/avisos, construído sobre o Swiper.
 * Segue a mesma paleta e tipografia do MainNavHeader (teal / gold / blue,
 * Poppins, ícones mdi) para manter a mesma identidade visual do topo do site.
 */

const props = defineProps({
  titulo: { type: String, default: 'Fique por dentro do que está acontecendo na rede municipal' },
  autoplay: { type: Boolean, default: true },
  autoplayDelay: { type: Number, default: 6000 },
  noticias: {
    type: Array,
    // cada item: { id, titulo, resumo, href, imagem, imagemAlt, dia, mes, categoria }
    default: () => ([
      {
        id: 1,
        titulo: 'Matrículas para 2027 já estão abertas',
        resumo: 'Famílias podem realizar matrícula e renovação pelo portal ou nas unidades escolares.',
        href: '#',
        imagem: 'https://images.unsplash.com/photo-1503676260728-1c00da094a0b?w=800&q=80',
        dia: '12',
        mes: 'AGO',
        categoria: 'Matrícula'
      },
      {
        id: 2,
        titulo: 'Rede municipal recebe novos laboratórios de informática',
        resumo: 'Investimento contempla 14 escolas com equipamentos e conexão de internet dedicada.',
        href: '#',
        imagem: 'https://images.unsplash.com/photo-1509062522246-3755977927d7?w=800&q=80',
        dia: '08',
        mes: 'AGO',
        categoria: 'Infraestrutura'
      },
      {
        id: 3,
        titulo: 'Resultado do processo seletivo para professores substitutos',
        resumo: 'Lista de aprovados e próximos passos para convocação já estão disponíveis.',
        href: '#',
        imagem: 'https://images.unsplash.com/photo-1523240795612-9a054b0db644?w=800&q=80',
        dia: '05',
        mes: 'AGO',
        categoria: 'Editais'
      },
      {
        id: 4,
        titulo: 'Calendário letivo 2027 é publicado',
        resumo: 'Confira datas de início e término do ano letivo, recessos e feriados escolares.',
        href: '#',
        imagem: 'https://images.unsplash.com/photo-1522202176988-66273c2fd55f?w=800&q=80',
        dia: '01',
        mes: 'AGO',
        categoria: 'Calendário'
      }
    ])
  }
})

const modules = [Navigation, Pagination, Autoplay, Keyboard, A11y]

const prevBtn = ref(null)
const nextBtn = ref(null)
const paginationEl = ref(null)

const swiperInstance = shallowRef(null)
const isBeginning = ref(true)
const isEnd = ref(false)

function onSwiper(swiper) {
  swiperInstance.value = swiper
  isBeginning.value = swiper.isBeginning
  isEnd.value = swiper.isEnd
}

function onSlideChange(swiper) {
  isBeginning.value = swiper.isBeginning
  isEnd.value = swiper.isEnd
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap');
</style>

<style scoped>
.novidades{
  --teal: #037770;
  --teal-dark: #025c56;
  --gold: #F5B301;
  --blue: #0064B6;
  --blue-dark: #00508f;
  --ink: #17332f;
  --muted: #5c706d;
  --border: #e6ece9;

  background: #ffffff;
  font-family: 'Poppins', Arial, Helvetica, sans-serif;
  padding: 64px 24px 72px;
}

.novidades__inner{
  max-width: 1440px;
  margin: 0 auto;
}

.novidades__head{
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 24px;
  margin-bottom: 32px;
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
  margin: 0 0 10px;
}
.eyebrow .mdi{ font-size: 16px; color: var(--gold); }

.title{
  font-family: 'Poppins', Arial, sans-serif;
  font-weight: 800;
  font-size: 28px;
  line-height: 1.3;
  color: var(--ink);
  margin: 0;
  max-width: 620px;
}

.novidades__controls{
  display: flex;
  gap: 8px;
  flex-shrink: 0;
}

.icon-round{
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: 1.5px solid #e3e9e8;
  background: #fff;
  color: var(--teal);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  cursor: pointer;
  transition: background .15s ease, color .15s ease, border-color .15s ease;
}
.icon-round:hover{ background: var(--teal); color: #fff; border-color: var(--teal); }
.icon-round.is-disabled{
  opacity: .35;
  pointer-events: none;
}

.novidades__swiper{
  padding-bottom: 4px;
}

.card{
  display: flex;
  flex-direction: column;
  height: 100%;
  background: #fff;
  border: 1px solid #e6ece9;
  border-radius: 14px;
  overflow: hidden;
  text-decoration: none;
  color: inherit;
  transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease;
}
.card:hover,
.card:focus-visible{
  transform: translateY(-4px);
  box-shadow: 0 16px 32px rgba(3, 119, 112, .12);
  border-color: var(--teal);
  outline: none;
}

.card__media{
  position: relative;
  aspect-ratio: 16 / 10;
  overflow: hidden;
  background: #eef3f2;
}
.card__media img{
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform .35s ease;
}
.card:hover .card__media img{ transform: scale(1.05); }

.card__data{
  position: absolute;
  left: 14px;
  bottom: -18px;
  background: #fff;
  border-radius: 10px;
  padding: 8px 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  line-height: 1.05;
  box-shadow: 0 8px 18px rgba(0,0,0,.14);
}
.card__data strong{
  font-size: 17px;
  font-weight: 800;
  color: var(--ink);
}
.card__data span{
  font-size: 10.5px;
  font-weight: 700;
  letter-spacing: .4px;
  color: var(--teal);
}

.card__categoria{
  position: absolute;
  top: 12px;
  right: 12px;
  background: var(--gold);
  color: #4a3300;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: .3px;
  text-transform: uppercase;
  padding: 4px 10px;
  border-radius: 999px;
}

.card__body{
  padding: 30px 20px 22px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex: 1;
}

.card__body h3{
  font-size: 16.5px;
  font-weight: 700;
  line-height: 1.4;
  color: var(--ink);
  margin: 0;
}

.card__body p{
  font-size: 13.5px;
  color: var(--muted);
  line-height: 1.6;
  margin: 0;
  flex: 1;
}

.card__link{
  margin-top: 4px;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 13px;
  font-weight: 700;
  color: var(--blue);
}
.card__link .mdi{
  font-size: 16px;
  transition: transform .18s ease;
}
.card:hover .card__link .mdi{ transform: translateX(3px); }

.novidades__pagination{
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 28px;
}
.novidades__pagination :deep(.swiper-pagination-bullet){
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #d7e2e0;
  opacity: 1;
  margin: 0 !important;
  transition: background .2s ease, width .2s ease;
  cursor: pointer;
}
.novidades__pagination :deep(.swiper-pagination-bullet-active){
  background: var(--teal);
  width: 22px;
  border-radius: 5px;
}

/* ============================= RESPONSIVO ============================= */
@media (max-width: 1108px){
  .novidades{ padding: 48px 16px 56px; }
  .title{ font-size: 23px; }
}

@media (max-width: 640px){
  .novidades__head{
    flex-direction: column;
    align-items: flex-start;
    gap: 16px;
  }
  .novidades__controls{ display: none; }
  .card__body{ padding: 28px 16px 18px; }
}
</style>