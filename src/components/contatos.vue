<template>
  <section class="contact" ref="sectionRef" :class="{ 'is-visible': visible }">
    <div class="contact__ambient" aria-hidden="true">
      <div class="facet"></div>
    </div>

    <div class="contact__inner">
      <div class="contact__head">
        <div class="eyebrow-row">
          <span class="eyebrow-mark" aria-hidden="true">✦</span>
          <p class="eyebrow">Fale conosco</p>
        </div>
        <h2 class="headline">Vamos <span class="word--accent">transformar</span> seu projeto em realidade</h2>
        <p class="subtext">
          Preencha o formulário e envie direto pelo WhatsApp — ou fale com a gente pelos canais abaixo.
        </p>
      </div>

      <div class="contact__grid">
        <!-- Formulário -->
        <form class="form" novalidate @submit="handleSubmit">
          <span class="form__badge">
            <span class="form__badge-dot"></span> Resposta em até 1h úteis
          </span>

          <div class="field" :class="{ 'is-invalid': touched.nome && errors.nome }">
            <label for="c-nome">Nome*</label>
            <input
              id="c-nome"
              v-model="form.nome"
              type="text"
              placeholder="Como podemos te chamar?"
              autocomplete="name"
              @blur="touched.nome = true"
            />
            <span class="field__icon">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M20 21a8 8 0 1 0-16 0" /><circle cx="12" cy="7" r="4" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.nome && errors.nome">{{ errors.nome }}</p>
          </div>

          <div class="field" :class="{ 'is-invalid': touched.telefone && errors.telefone }">
            <label for="c-tel">Telefone*</label>
            <input
              id="c-tel"
              v-model="form.telefone"
              type="tel"
              inputmode="numeric"
              placeholder="(00) 00000-0000"
              autocomplete="tel"
              @input="onPhoneInput"
              @blur="touched.telefone = true"
            />
            <span class="field__icon">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.telefone && errors.telefone">{{ errors.telefone }}</p>
          </div>

          <div class="field" :class="{ 'is-invalid': touched.email && errors.email }">
            <label for="c-email">E-mail <span class="field__optional">(opcional)</span></label>
            <input
              id="c-email"
              v-model="form.email"
              type="email"
              placeholder="Digite seu endereço de e-mail"
              autocomplete="email"
              @blur="touched.email = true"
            />
            <span class="field__icon">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <rect x="2" y="4" width="20" height="16" rx="2" /><path d="m22 6-10 7L2 6" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.email && errors.email">{{ errors.email }}</p>
          </div>

          <div class="field" :class="{ 'is-invalid': touched.mensagem && errors.mensagem }">
            <label for="c-msg">Mensagem*</label>
            <textarea
              id="c-msg"
              v-model="form.mensagem"
              rows="3"
              placeholder="Conte rapidamente o que você precisa..."
              @blur="touched.mensagem = true"
            ></textarea>
            <span class="field__icon field__icon--textarea">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" />
              </svg>
            </span>
            <p class="field__error" v-if="touched.mensagem && errors.mensagem">{{ errors.mensagem }}</p>
          </div>

          <button type="submit" class="submit" :class="status" :disabled="status === 'sending'">
            <span v-if="status === 'idle'" class="submit__content">
              Enviar sua mensagem agora
              <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z" /></svg>
            </span>
            <span v-else-if="status === 'sending'" class="submit__content">
              <svg class="spin" viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M12 2a10 10 0 0 1 10 10" /></svg>
              Preparando mensagem...
            </span>
            <span v-else class="submit__content">
              <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12l4 4 10-10" /></svg>
              Aberto no WhatsApp!
            </span>
          </button>

          <p class="form__fallback">
            Prefere e-mail? <a :href="mailtoLink">Escreva para {{ empresa.email }}</a>
          </p>
        </form>

        <!-- Painel lateral -->
        <aside class="side">
          <div class="about-card">
            <span class="about-card__badge">Desde 1992</span>
            <p class="about-card__title">Tropical Serralheria &amp; Vidraçaria</p>
            <p class="about-card__text">
              Transformando sonhos em realidade. Especializada em esquadrias metálicas e em alumínio.
            </p>
          </div>

          <div class="status-card" :class="{ 'is-open': isOpenNow }">
            <span class="status-card__dot"></span>
            <div>
              <p class="status-card__state">{{ isOpenNow ? 'Estamos atendendo agora' : 'Fora do horário de atendimento' }}</p>
              <p class="status-card__next">{{ nextChangeLabel }}</p>
            </div>
          </div>

          <ul class="channels">
            <li v-for="ch in channels" :key="ch.id" class="channel">
              <span class="channel__icon" :style="{ background: ch.bg, color: ch.fg }" v-html="ch.icon"></span>
              <div class="channel__text">
                <span class="channel__label">{{ ch.label }}</span>
                <span class="channel__value">{{ ch.value }}</span>
              </div>
              <button
                type="button"
                class="channel__action"
                :title="ch.action === 'copy' ? 'Copiar' : 'Abrir'"
                @click="ch.action === 'copy' ? copy(ch.raw, ch.id) : go(ch.href)"
              >
                <svg v-if="copiedId === ch.id" viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M5 12l4 4 10-10" /></svg>
                <svg v-else-if="ch.action === 'copy'" viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="11" height="11" rx="2" /><path d="M5 15V5a2 2 0 0 1 2-2h10" /></svg>
                <svg v-else viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M7 17L17 7M7 7h10v10" /></svg>
              </button>
            </li>
          </ul>

          <div class="hours-card">
            <p class="hours-card__title">Atendimento</p>
            <ul class="hours-card__list">
              <li v-for="(h, i) in schedule" :key="h.day" :class="{ 'is-today': i === todayIndex }">
                <span>{{ h.day }}</span>
                <strong>{{ formatPeriods(h) }}</strong>
              </li>
            </ul>
          </div>

          <div class="social">
            <a
              v-for="s in socials"
              :key="s.id"
              :href="s.href"
              target="_blank"
              rel="noopener"
              class="social__link"
              :aria-label="s.label"
              v-html="s.icon"
            ></a>
          </div>
        </aside>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive, ref, computed, onMounted, onBeforeUnmount } from 'vue'

/* ---------- Dados — troque pelos seus ---------- */
const empresa = reactive({
  whatsappRaw: '551334264615',
  phoneDisplay: '(13) 3426-4615',
  email: 'contato@tropicalserralheria.com.br',
  endereco: 'Av. Emília Alves Muller, 2018 — Savoy, Itanhaém-SP',
  instagram: '@tropical.serralheria',
  instagramHref: 'https://www.instagram.com/tropical.serralheria',
  facebookHref: 'https://www.facebook.com/Tropical.vidracaria',
  tiktokHref: 'https://www.tiktok.com/@tropical.serralheria'
})

/* domingo=0 ... sábado=6. periods: lista de {open, close} em "HH:MM". Lista vazia = fechado */
const schedule = [
  { day: 'Domingo', periods: [] },
  { day: 'Segunda', periods: [{ open: '08:00', close: '12:00' }, { open: '13:30', close: '17:00' }] },
  { day: 'Terça', periods: [{ open: '08:00', close: '12:00' }, { open: '13:30', close: '17:00' }] },
  { day: 'Quarta', periods: [{ open: '08:00', close: '12:00' }, { open: '13:30', close: '17:00' }] },
  { day: 'Quinta', periods: [{ open: '08:00', close: '12:00' }, { open: '13:30', close: '17:00' }] },
  { day: 'Sexta', periods: [{ open: '08:00', close: '12:00' }, { open: '13:30', close: '17:00' }] },
  { day: 'Sábado', periods: [{ open: '08:00', close: '12:00' }] }
]

function fmtTime(t) {
  const [h, m] = t.split(':')
  return m === '00' ? `${h}h` : `${h}h${m}`
}
function formatPeriods(day) {
  if (!day.periods.length) return 'Fechado'
  return day.periods.map((p) => `${fmtTime(p.open)} às ${fmtTime(p.close)}`).join(' · ')
}

const channels = [
  {
    id: 'whats',
    label: 'WhatsApp',
    value: empresa.phoneDisplay,
    raw: empresa.phoneDisplay,
    action: 'open',
    href: `https://wa.me/${empresa.whatsappRaw}`,
    bg: 'rgba(76, 175, 80, .12)', fg: '#2e7d32',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="currentColor"><path d="M12 2a10 10 0 0 0-8.6 15.1L2 22l5.05-1.36A10 10 0 1 0 12 2Zm5.68 14.2c-.24.68-1.4 1.3-1.94 1.38-.5.08-1.12.11-1.8-.11-.42-.13-.96-.31-1.65-.6-2.9-1.25-4.8-4.16-4.94-4.36-.14-.2-1.18-1.57-1.18-3 0-1.42.75-2.12 1.02-2.41.27-.29.58-.36.78-.36.2 0 .39 0 .56.01.18.01.42-.07.66.5.24.58.83 2 .9 2.14.07.15.12.32.02.52-.1.2-.15.32-.3.5-.15.17-.31.39-.44.52-.15.15-.3.31-.13.6.17.3.76 1.25 1.63 2.02 1.12 1 2.06 1.31 2.36 1.46.3.15.47.13.65-.08.18-.2.75-.87.95-1.17.2-.3.4-.24.66-.15.27.1 1.7.8 2 .95.29.14.48.21.55.33.07.12.07.7-.17 1.38Z"/></svg>'
  },
  {
    id: 'tel',
    label: 'Telefone',
    value: empresa.phoneDisplay,
    raw: empresa.phoneDisplay,
    action: 'copy',
    bg: 'rgba(76, 175, 80, .12)', fg: '#2e7d32',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"/></svg>'
  },
  {
    id: 'mail',
    label: 'E-mail',
    value: empresa.email,
    raw: empresa.email,
    action: 'copy',
    bg: 'rgba(58, 58, 58, .08)', fg: '#3A3A3A',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 6-10 7L2 6"/></svg>'
  },
  {
    id: 'endereco',
    label: 'Endereço',
    value: empresa.endereco,
    raw: empresa.endereco,
    action: 'open',
    href: `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(empresa.endereco)}`,
    bg: 'rgba(76, 175, 80, .12)', fg: '#2e7d32',
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"/><circle cx="12" cy="10" r="3"/></svg>'
  }
]

const socials = [
  {
    id: 'insta', label: 'Instagram', href: empresa.instagramHref,
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1"/></svg>'
  },
  {
    id: 'face', label: 'Facebook', href: empresa.facebookHref,
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="currentColor"><path d="M17 3H14a5 5 0 0 0-5 5v3H6v4h3v9h4v-9h3l1-4h-4V8a1 1 0 0 1 1-1h3z"/></svg>'
  },
  {
    id: 'tiktok', label: 'TikTok', href: empresa.tiktokHref,
    icon: '<svg viewBox="0 0 24 24" width="17" height="17" fill="currentColor"><path d="M16.5 2h-3v13.2a2.8 2.8 0 1 1-2.2-2.74v-3.06A6 6 0 1 0 16.5 15V8.8a7.6 7.6 0 0 0 4 1.15v-3.1A4.5 4.5 0 0 1 16.5 2Z"/></svg>'
  }
]

const mailtoLink = computed(() => {
  const subject = encodeURIComponent('Contato pelo site')
  const body = encodeURIComponent(`Olá! Meu nome é ${form.nome || '___'}.\n\n${form.mensagem || ''}`)
  return `mailto:${empresa.email}?subject=${subject}&body=${body}`
})

/* ---------- Status ao vivo ---------- */
const now = ref(new Date())
let clock = null
onMounted(() => { clock = setInterval(() => (now.value = new Date()), 30000) })
onBeforeUnmount(() => clearInterval(clock))

const todayIndex = computed(() => now.value.getDay())

function toMinutes(hhmm) {
  const [h, m] = hhmm.split(':').map(Number)
  return h * 60 + m
}
function activePeriod(day, mins) {
  return day.periods.find((p) => mins >= toMinutes(p.open) && mins < toMinutes(p.close))
}

const isOpenNow = computed(() => {
  const today = schedule[todayIndex.value]
  const mins = now.value.getHours() * 60 + now.value.getMinutes()
  return !!activePeriod(today, mins)
})

const nextChangeLabel = computed(() => {
  const mins = now.value.getHours() * 60 + now.value.getMinutes()
  const today = schedule[todayIndex.value]
  const current = activePeriod(today, mins)
  if (current) return `Fecha às ${fmtTime(current.close)}`

  const upcomingToday = today.periods.find((p) => toMinutes(p.open) > mins)
  if (upcomingToday) return `Abre hoje às ${fmtTime(upcomingToday.open)}`

  for (let i = 1; i < 8; i++) {
    const idx = (todayIndex.value + i) % 7
    const day = schedule[idx]
    if (day.periods.length) {
      const label = i === 1 ? 'amanhã' : day.day.toLowerCase()
      return `Abre ${label} às ${fmtTime(day.periods[0].open)}`
    }
  }
  return ''
})

/* ---------- Formulário ---------- */
const form = reactive({ nome: '', telefone: '', email: '', mensagem: '' })
const touched = reactive({ nome: false, telefone: false, email: false, mensagem: false })
const status = ref('idle') // idle | sending | success

const errors = computed(() => ({
  nome: form.nome.trim().length < 2 ? 'Conta seu nome pra gente te chamar certinho.' : '',
  telefone: form.telefone.replace(/\D/g, '').length < 10 ? 'Confere esse número, parece incompleto.' : '',
  email: form.email.trim() && !/^\S+@\S+\.\S+$/.test(form.email.trim()) ? 'Confere esse e-mail, parece incompleto.' : '',
  mensagem: form.mensagem.trim().length < 5 ? 'Conta um pouco mais sobre o que você precisa.' : ''
}))
const isValid = computed(() => !errors.value.nome && !errors.value.telefone && !errors.value.email && !errors.value.mensagem)

function onPhoneInput() {
  const digits = form.telefone.replace(/\D/g, '').slice(0, 11)
  let out = digits
  if (digits.length > 2) out = `(${digits.slice(0, 2)}) ${digits.slice(2)}`
  if (digits.length > 7) {
    const split = digits.length > 10 ? 7 : 6
    out = `(${digits.slice(0, 2)}) ${digits.slice(2, split)}-${digits.slice(split)}`
  }
  form.telefone = out
}

function handleSubmit(e) {
  e.preventDefault()
  touched.nome = true
  touched.telefone = true
  touched.email = true
  touched.mensagem = true
  if (!isValid.value) return

  status.value = 'sending'

  const lines = [
    `Olá! Meu nome é ${form.nome}.`,
    `Telefone: ${form.telefone}`
  ]
  if (form.email.trim()) lines.push(`E-mail: ${form.email.trim()}`)
  lines.push('', form.mensagem.trim())

  setTimeout(() => {
    window.open(`https://wa.me/${empresa.whatsappRaw}?text=${encodeURIComponent(lines.join('\n'))}`, '_blank', 'noopener')
    status.value = 'success'
    setTimeout(() => {
      status.value = 'idle'
      form.nome = ''
      form.telefone = ''
      form.email = ''
      form.mensagem = ''
      touched.nome = false
      touched.telefone = false
      touched.email = false
      touched.mensagem = false
    }, 3200)
  }, 700)
}

/* ---------- Ações dos cards ---------- */
const copiedId = ref('')
async function copy(text, id) {
  try {
    await navigator.clipboard.writeText(text)
  } catch {
    const ta = document.createElement('textarea')
    ta.value = text
    document.body.appendChild(ta)
    ta.select()
    document.execCommand('copy')
    document.body.removeChild(ta)
  }
  copiedId.value = id
  setTimeout(() => (copiedId.value = ''), 1800)
}
function go(href) { window.open(href, '_blank', 'noopener') }

/* ---------- Reveal on scroll ---------- */
const sectionRef = ref(null)
const visible = ref(false)
let observer = null
onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  if (prefersReducedMotion || !sectionRef.value) {
    visible.value = true
    return
  }
  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        visible.value = true
        observer.disconnect()
      }
    },
    { threshold: 0.1 }
  )
  observer.observe(sectionRef.value)
})
onBeforeUnmount(() => observer?.disconnect())
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@500;600;700;800&family=Plus+Jakarta+Sans:wght@600;700;800&display=swap');

.contact,
.contact *,
.contact *::before,
.contact *::after {
  box-sizing: border-box;
}

.contact {
  --paper: #F4F6F9;
  --ink: #2F2F2F;
  --ink-dim: #8A8F98;
  --green: #4CAF50;
  --green-hover: #43A047;
  --green-bright: #66BB6A;
  --green-deep: #2e7d32;
  --line: rgba(58, 58, 58, 0.08);

  position: relative;
  overflow: hidden;
  background: var(--paper);
  color: var(--ink);
  font-family: 'Manrope', sans-serif;
  padding: clamp(64px, 9vw, 120px) clamp(20px, 6vw, 64px);
}

.contact__ambient { position: absolute; inset: 0; pointer-events: none; overflow: hidden; z-index: 0; }
.facet {
  position: absolute;
  width: 32vw; height: 32vw; max-width: 460px; max-height: 460px;
  top: -12%; right: -10%;
  background: radial-gradient(circle, rgba(76, 175, 80, 0.16), transparent 70%);
  filter: blur(54px);
  mix-blend-mode: multiply;
}

.contact__inner { position: relative; z-index: 2; max-width: 1080px; margin: 0 auto; }

.contact__head { text-align: center; max-width: 620px; margin: 0 auto clamp(36px, 5vw, 52px); }
.eyebrow-row { display: flex; align-items: center; justify-content: center; gap: 10px; margin-bottom: 14px; }
.eyebrow-mark { color: var(--green); font-size: .85rem; }
.eyebrow { font-size: .78rem; letter-spacing: .22em; text-transform: uppercase; color: var(--green-deep); margin: 0; font-weight: 700; font-family: 'Manrope', sans-serif; }
.headline { font-family: 'Plus Jakarta Sans', sans-serif; font-weight: 800; letter-spacing: -0.01em; font-size: clamp(1.9rem, 3.6vw, 2.9rem); line-height: 1.15; margin: 0 0 16px; color: var(--ink); }
.word--accent {
  background: linear-gradient(100deg, var(--green-deep) 0%, var(--green-bright) 35%, var(--green) 55%, var(--green-bright) 75%, var(--green-deep) 100%);
  background-size: 220% 100%;
  -webkit-background-clip: text; background-clip: text; color: transparent;
}
.subtext { font-size: clamp(1rem, 1.2vw, 1.08rem); line-height: 1.65; color: var(--ink-dim); margin: 0; }

.contact__grid {
  display: grid;
  grid-template-columns: 1.15fr 0.85fr;
  gap: clamp(24px, 3.5vw, 48px);
  align-items: start;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}
.contact.is-visible .contact__grid { opacity: 1; transform: none; }

/* ---------- Formulário ---------- */
.form {
  position: relative;
  background: #fff;
  border: 1px solid var(--line);
  border-radius: 20px;
  padding: clamp(26px, 3vw, 36px);
  box-shadow: 0 24px 48px -28px rgba(58, 58, 58, .3);
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.form__badge {
  align-self: flex-start;
  display: inline-flex; align-items: center; gap: 7px;
  font-size: .72rem; font-weight: 700; letter-spacing: .03em;
  color: var(--green-deep); background: rgba(76, 175, 80, .12);
  padding: 6px 12px; border-radius: 999px; margin-bottom: 2px;
}
.form__badge-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--green); }

.field { position: relative; display: flex; flex-direction: column; gap: 6px; }
.field label { font-size: .78rem; font-weight: 700; color: var(--ink-dim); }
.field__optional { font-weight: 500; color: rgba(58,58,58,.4); }

.field input,
.field textarea {
  font-family: 'Manrope', sans-serif;
  font-size: .92rem;
  padding: 12px 14px 12px 40px;
  border-radius: 12px;
  border: 1px solid rgba(58, 58, 58, .14);
  background: #fbfcfa;
  color: var(--ink);
  outline: none;
  transition: border-color .2s ease, box-shadow .2s ease, background .2s ease;
  resize: vertical;
}
.field textarea { padding-left: 40px; min-height: 84px; }
.field input:focus,
.field textarea:focus {
  border-color: var(--green);
  background: #fff;
  box-shadow: 0 0 0 3px rgba(76, 175, 80, .14);
}
.field.is-invalid input,
.field.is-invalid textarea { border-color: #c94b4b; box-shadow: 0 0 0 3px rgba(201, 75, 75, .1); }

.field__icon {
  position: absolute; left: 13px; top: 33px;
  color: rgba(58, 58, 58, .35); pointer-events: none;
  display: flex;
}
.field__icon--textarea { top: 33px; }

.field__error { margin: 0; font-size: .74rem; color: #c94b4b; font-weight: 600; }

.submit {
  margin-top: 4px;
  display: inline-flex; align-items: center; justify-content: center;
  padding: 14px 20px; border-radius: 8px; border: none; cursor: pointer;
  font-family: 'Manrope', sans-serif; font-weight: 700; font-size: .94rem;
  color: #fff;
  background: var(--green);
  box-shadow: 0 14px 28px -14px rgba(76, 175, 80, .55);
  transition: transform .2s ease, box-shadow .2s ease, background .3s ease;
}
.submit:hover:not(:disabled) { background: var(--green-hover); transform: translateY(-1px); box-shadow: 0 18px 32px -14px rgba(76, 175, 80, .6); }
.submit:disabled { cursor: default; }
.submit.success { background: linear-gradient(120deg, #1f9c52, #25d366); box-shadow: 0 14px 28px -14px rgba(31, 156, 82, .5); }
.submit__content { display: inline-flex; align-items: center; gap: 9px; }
.submit .spin { animation: spin 1s linear infinite; }
@keyframes spin { to { transform: rotate(360deg); } }

.form__fallback { margin: 0; font-size: .8rem; color: var(--ink-dim); text-align: center; }
.form__fallback a { color: var(--green-deep); font-weight: 700; text-decoration: none; }
.form__fallback a:hover { text-decoration: underline; }

/* ---------- Lateral ---------- */
.side { display: flex; flex-direction: column; gap: 18px; }

.about-card {
  background: #fff; border: 1px solid var(--line); border-radius: 16px;
  padding: 18px 20px;
  box-shadow: 0 20px 40px -28px rgba(58, 58, 58, .28);
}
.about-card__badge {
  display: inline-block; font-size: .68rem; font-weight: 700; letter-spacing: .04em;
  color: var(--green-deep); background: rgba(76, 175, 80, .12);
  padding: 4px 10px; border-radius: 999px; margin-bottom: 10px;
}
.about-card__title { margin: 0 0 6px; font-family: 'Plus Jakarta Sans', sans-serif; font-weight: 700; font-size: .95rem; color: var(--ink); }
.about-card__text { margin: 0; font-size: .82rem; line-height: 1.55; color: var(--ink-dim); }

.status-card {
  display: flex; align-items: center; gap: 12px;
  background: #fff; border: 1px solid var(--line); border-radius: 16px;
  padding: 16px 18px;
  box-shadow: 0 20px 40px -28px rgba(58, 58, 58, .28);
}
.status-card__dot {
  width: 11px; height: 11px; border-radius: 50%; flex-shrink: 0;
  background: #b9b3a0;
}
.status-card.is-open .status-card__dot {
  background: #25d366;
  box-shadow: 0 0 0 0 rgba(37, 211, 102, .5);
  animation: dot-pulse 1.8s ease-out infinite;
}
@keyframes dot-pulse {
  0% { box-shadow: 0 0 0 0 rgba(37, 211, 102, .45); }
  100% { box-shadow: 0 0 0 9px rgba(37, 211, 102, 0); }
}
.status-card__state { margin: 0; font-weight: 700; font-size: .88rem; }
.status-card__next { margin: 2px 0 0; font-size: .78rem; color: var(--ink-dim); }

.channels { list-style: none; margin: 0; padding: 0; display: flex; flex-direction: column; gap: 10px; }
.channel {
  display: flex; align-items: center; gap: 12px;
  background: #fff; border: 1px solid var(--line); border-radius: 14px;
  padding: 12px 14px;
  box-shadow: 0 16px 32px -26px rgba(58, 58, 58, .28);
  transition: transform .2s ease;
}
.channel:hover { transform: translateY(-1px); }
.channel__icon {
  width: 38px; height: 38px; border-radius: 10px; flex-shrink: 0;
  display: flex; align-items: center; justify-content: center;
}
.channel__text { display: flex; flex-direction: column; min-width: 0; flex: 1; }
.channel__label { font-size: .7rem; font-weight: 700; text-transform: uppercase; letter-spacing: .04em; color: var(--ink-dim); }
.channel__value { font-size: .88rem; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.channel__action {
  width: 32px; height: 32px; border-radius: 9px; flex-shrink: 0;
  border: 1px solid rgba(58, 58, 58, .1); background: none; color: var(--ink-dim);
  display: flex; align-items: center; justify-content: center; cursor: pointer;
  transition: background .2s ease, color .2s ease, border-color .2s ease;
}
.channel__action:hover { background: var(--green); color: #fff; border-color: var(--green); }

.hours-card {
  background: #fff; border: 1px solid var(--line); border-radius: 16px;
  padding: 18px 20px;
  box-shadow: 0 20px 40px -28px rgba(58, 58, 58, .28);
}
.hours-card__title { margin: 0 0 12px; font-size: .78rem; font-weight: 700; color: var(--ink-dim); }
.hours-card__list { list-style: none; margin: 0; padding: 0; display: flex; flex-direction: column; gap: 7px; }
.hours-card__list li { display: flex; justify-content: space-between; gap: 12px; font-size: .82rem; padding: 3px 8px; border-radius: 8px; }
.hours-card__list span { color: var(--ink-dim); }
.hours-card__list strong { font-weight: 700; }
.hours-card__list li.is-today { background: rgba(76, 175, 80, .12); }
.hours-card__list li.is-today span,
.hours-card__list li.is-today strong { color: var(--green-deep); }

.social { display: flex; gap: 10px; }
.social__link {
  width: 42px; height: 42px; border-radius: 12px;
  background: #fff; border: 1px solid var(--line); color: var(--ink);
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 16px 32px -26px rgba(58, 58, 58, .28);
  transition: transform .2s ease, background .2s ease, color .2s ease;
}
.social__link:hover { transform: translateY(-2px); background: var(--green); color: #fff; }

@media (max-width: 860px) {
  .contact__grid { grid-template-columns: 1fr; }
  .field input, .field textarea { padding-left: 38px; }
}

@media (prefers-reduced-motion: reduce) {
  .contact__grid { opacity: 1 !important; transform: none !important; transition: none !important; }
  .status-card.is-open .status-card__dot { animation: none; }
  .submit .spin { animation: none; }
}
</style>