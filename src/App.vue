<template>
  <div class="birthday-wrapper">
    <canvas ref="confettiCanvas" class="confetti-canvas"></canvas>

    <div class="content-container">
      <h1 class="title">С Днём Рождения, <span class="highlight">Папа</span>! 🎉</h1>
      <p class="subtitle">Тебе 38 — ты наш герой, защитник и самый лучший папа!</p>

      <section class="family-section">
        <h2 class="section-title">💌 Твоя любимая семья:</h2>
<div class="family-photos">
  <div v-for="(member, i) in familyMembers" :key="i" class="photo-wrapper">
    <img :src="member.src" :alt="member.name" class="family-photo" />
    <p class="member-name">{{ member.name }}</p>
  </div>
</div>

      </section>

      <section class="scratch-section">
        <h2 class="scratch-title">✨ Я хочу, чтобы ты вспомнил</h2>
        <div class="scratch-container">
          <img src="/family.jpeg" alt="Семейное воспоминание" class="scratch-image" />
        </div>
      </section>
    </div>
  </div>
  <section class="family-message-section">
  <h2 class="section-title">📸 Особенные пожелания:</h2>
  <div v-for="(member, index) in familyMembers" :key="index" class="message-block" :class="{ 'reverse': index % 2 !== 0 }">
    <img :src="member.src" :alt="member.name" class="message-photo" />
    <div class="message-text-block">
      <h3 class="member-name-large">{{ member.name }}</h3>
      <p class="personal-message">{{ member.message }}</p>
    </div>
  </div>
  <section class="life-counter">
  <h2 class="section-title">Ты живёшь уже:</h2>
  <p class="life-time">{{ lifeTime }}</p>
</section>

</section>

</template>
<script setup>
import confetti from 'canvas-confetti'
import { ref, onMounted, onUnmounted } from 'vue'

const lifeTime = ref('')

// Время рождения (Астана GMT+6)
function plural(n, one, few, many) {
  const mod10 = n % 10
  const mod100 = n % 100

  if (mod10 === 1 && mod100 !== 11) return one
  if (mod10 >= 2 && mod10 <= 4 && (mod100 < 10 || mod100 >= 20)) return few
  return many
}

const birthDate = new Date('1987-08-03T07:52:00Z') // 13:52 по Астане = 07:52 UTC

function updateLifeTime() {
  const now = new Date()

  // Смещение часового пояса Астаны вручную (UTC+6)
  const astanaOffset = 6 * 60 * 60 * 1000
  const nowAstana = new Date(now.getTime() + astanaOffset)

  let diff = nowAstana.getTime() - birthDate.getTime()

  const minutes = Math.floor(diff / (1000 * 60)) % 60
  const hours = Math.floor(diff / (1000 * 60 * 60)) % 24
  const daysTotal = Math.floor(diff / (1000 * 60 * 60 * 24))

  // Примерный расчёт лет и месяцев
  const birthYear = birthDate.getUTCFullYear()
  const birthMonth = birthDate.getUTCMonth()
  const birthDay = birthDate.getUTCDate()

  const nowYear = nowAstana.getUTCFullYear()
  const nowMonth = nowAstana.getUTCMonth()
  const nowDay = nowAstana.getUTCDate()

  let years = nowYear - birthYear
  let months = nowMonth - birthMonth
  let days = nowDay - birthDay

  if (days < 0) {
    months -= 1
    const prevMonth = new Date(nowYear, nowMonth, 0).getDate()
    days += prevMonth
  }

  if (months < 0) {
    years -= 1
    months += 12
  }

lifeTime.value = `${years} лет ${months} месяцев ${days} дней, ${hours} ${plural(hours, 'час', 'часа', 'часов')} ${minutes} ${plural(minutes, 'минута', 'минуты', 'минут')}`
}

let interval

onMounted(() => {
  updateLifeTime()
  interval = setInterval(updateLifeTime, 60 * 1000) // обновление каждую минуту
})

onUnmounted(() => {
  clearInterval(interval)
})


const confettiCanvas = ref(null)

const familyMembers = [
  {
    name: 'Твоя любимая жена',
    src: '/mama.jpeg',
    message: 'С днём рождения солнце! Я тебя очень люблю! Ты у нас молодец, ты наш защитник и опора. Желаю здоровья, счастья и радости в жизни❤️!'
  },
  {
    name: 'Илиан',
    src: '/ilian.jpeg',
    message: 'С днём рождения папа, я тебя очень люблю! Ты мой защитник и супергерой! Желаю тебе здоровья, счастья и побольше денег🤑! --поэтому я и подарил тебе этот сайт'
  },
  {
    name: 'Демид',
    src: '/demid.jpeg',
    message: 'С днём рождения! Папа желаю счастья, здоровья, и удачи!🍀'
  },
  {
    name: 'Эмилия',
    src: '/emily.jpeg',
    message: 'Агу Агу Агу Агу Агу! Папа, ты самый лучший! Люблю тебя! Агу!😍'
  }
]


onMounted(() => {
  setInterval(() => {
    confetti.create(confettiCanvas.value, { resize: true })({
      particleCount: 100,
      spread: 120,
      origin: { y: Math.random() }
    })
  }, 3000)
})
</script>

<style scoped>
/* Гипер-стилизация для праздничного WOW-эффекта */

.birthday-wrapper {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #fffce8 0%, #ffe8e8 50%, #ffe0f7 100%);
  overflow: hidden;
  position: relative;
  animation: bgMove 12s ease-in-out infinite alternate;
}
@keyframes bgMove {
  0% { background-position: 0 0; }
  100% { background-position: 100% 100%; }
}

.confetti-canvas {
  position: fixed;
  top: 0; left: 0;
  width: 100vw; height: 100vh;
  pointer-events: none;
  z-index: 999;
}

.content-container {
  max-width: 960px;
  margin: 0 auto;
  padding: 2.5rem 2rem 2rem 2rem;
  text-align: center;
  border-radius: 24px;
  background: rgba(255,255,255,0.85);
  box-shadow: 0 12px 48px 0 rgba(255, 140, 0, 0.10), 0 2px 8px rgba(255, 87, 34, 0.10);
  position: relative;
  z-index: 1;
  backdrop-filter: blur(2px);
  animation: fadeIn 1.2s;
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(40px);}
  to { opacity: 1; transform: none;}
}

.title {
  font-size: 3.2rem;
  font-weight: bold;
  color: #ff5722;
  margin-bottom: 0.5rem;
  text-shadow: 0 4px 24px #ffd180, 0 2px 8px #ffb74d;
  letter-spacing: 2px;
  animation: glow 2s infinite alternate;
}
@keyframes glow {
  from { text-shadow: 0 4px 24px #ffd180, 0 2px 8px #ffb74d; }
  to   { text-shadow: 0 8px 32px #ff9800, 0 4px 16px #ffb300; }
}

.highlight {
  color: #ff4081;
  text-shadow: 0 2px 12px #ffb6d5;
  background: linear-gradient(90deg, #ffb6d5 0%, #ffe082 100%);
  border-radius: 8px;
  padding: 0 8px;
  animation: highlightPulse 1.5s infinite alternate;
}
@keyframes highlightPulse {
  from { filter: brightness(1); }
  to   { filter: brightness(1.15); }
}

.subtitle {
  font-size: 2rem;
  color: #ff9800;
  margin-bottom: 2.5rem;
  font-weight: 500;
  text-shadow: 0 2px 8px #fffde7;
}

.family-section {
  margin-top: 2.5rem;
}

.section-title {
  font-size: 2.2rem;
  color: #d84315;
  margin-bottom: 1.5rem;
  font-weight: bold;
  letter-spacing: 1px;
  text-shadow: 0 2px 12px #ffe0b2;
}

.family-photos {
  display: flex;
  justify-content: center;
  gap: 2.5rem;
  flex-wrap: wrap;
  margin-top: 1.5rem;
}

.photo-wrapper {
  text-align: center;
  transition: transform 0.3s cubic-bezier(.4,2,.6,1), box-shadow 0.3s;
  filter: drop-shadow(0 4px 16px #ffd180);
}
.photo-wrapper:hover {
  transform: scale(1.12) rotate(-2deg);
  filter: drop-shadow(0 8px 32px #ffb300);
}

.family-photo {
  width: 180px;
  height: 180px;
  object-fit: cover;
  border-radius: 50%;
  border: 8px solid #ffa726;
  box-shadow: 0 8px 32px 0 #ffb74d, 0 0 40px #ffecb3;
  transition: box-shadow 0.3s, transform 0.3s;
  background: linear-gradient(135deg, #fffde7 60%, #ffe0b2 100%);
  animation: photoPop 2.5s infinite alternate;
}
@keyframes photoPop {
  from { box-shadow: 0 8px 32px 0 #ffb74d, 0 0 40px #ffecb3; }
  to   { box-shadow: 0 16px 48px 0 #ffb300, 0 0 60px #fffde7; }
}

.family-photo:hover {
  box-shadow: 0 16px 48px #ffb300, 0 0 60px #fffde7;
  transform: scale(1.15) rotate(2deg);
}

.member-name {
  margin-top: 0.8rem;
  font-size: 1.25rem;
  font-weight: bold;
  color: #ff7043;
  letter-spacing: 1px;
  text-shadow: 0 2px 8px #fff3e0;
}

.scratch-section {
  margin-top: 3rem;
}

.scratch-title {
  font-size: 1.7rem;
  color: #ff4081;
  margin-bottom: 1.2rem;
  font-weight: 600;
  text-shadow: 0 2px 8px #ffe0e0;
}

.scratch-container {
  margin: 0 auto;
  width: 340px;
  border-radius: 32px;
  overflow: hidden;
  box-shadow: 0 12px 48px rgba(255, 140, 0, 0.25), 0 2px 8px rgba(0,0,0,0.18);
  background: linear-gradient(120deg, #fffbe8 60%, #ffe0e0 100%);
  border: 5px solid #ffb74d;
  position: relative;
  transition: box-shadow 0.3s, transform 0.3s;
  animation: scratchPop 2.5s infinite alternate;
}
@keyframes scratchPop {
  from { box-shadow: 0 12px 48px rgba(255, 140, 0, 0.25); }
  to   { box-shadow: 0 24px 64px rgba(255, 140, 0, 0.35); }
}

.scratch-container:hover {
  box-shadow: 0 24px 64px rgba(255, 140, 0, 0.35), 0 4px 16px rgba(0,0,0,0.22);
  transform: scale(1.04) rotate(-2deg);
}

.scratch-image {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 28px;
  box-shadow: 0 4px 32px rgba(255, 183, 77, 0.25);
  border: 3px solid #ffd180;
  filter: brightness(1.08) saturate(1.15) drop-shadow(0 0 24px #ffe082);
  transition: filter 0.3s, box-shadow 0.3s;
  background: linear-gradient(135deg, #fffde7 60%, #ffe0b2 100%);
  animation: imagePulse 2.5s infinite alternate;
}
@keyframes imagePulse {
  from { filter: brightness(1.08) saturate(1.15) drop-shadow(0 0 24px #ffe082);}
  to   { filter: brightness(1.15) saturate(1.25) drop-shadow(0 0 40px #ffd54f);}
}

.scratch-container:hover .scratch-image {
  filter: brightness(1.18) saturate(1.25) drop-shadow(0 0 40px #ffd54f);
  box-shadow: 0 8px 40px rgba(255, 183, 77, 0.35);
}

.family-message-section {
  margin-top: 4rem;
  padding: 2.5rem 2rem;
  background: linear-gradient(120deg, #fffaf2 60%, #ffe0e0 100%);
  border-radius: 24px;
  box-shadow: 0 8px 32px rgba(255, 193, 7, 0.10);
  animation: fadeIn 1.5s;
}

.message-block {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2.5rem;
  margin-bottom: 3.5rem;
  flex-wrap: wrap;
  background: rgba(255,255,255,0.7);
  border-radius: 18px;
  box-shadow: 0 4px 24px #ffe08233;
  padding: 1.5rem 1rem;
  transition: box-shadow 0.3s, background 0.3s;
}
.message-block:hover {
  box-shadow: 0 8px 32px #ffd54f55;
  background: #fffde7cc;
}

.message-block.reverse {
  flex-direction: row-reverse;
}

.message-photo {
  width: 300px;
  height: 300px;
  object-fit: cover;
  border-radius: 32px;
  box-shadow: 0 12px 32px rgba(255, 193, 7, 0.18), 0 0 40px #ffe082;
  border: 7px solid #ffc107;
  background: linear-gradient(135deg, #fffde7 60%, #ffe0b2 100%);
  transition: box-shadow 0.3s, transform 0.3s;
  animation: photoPop 2.5s infinite alternate;
}
.message-photo:hover {
  box-shadow: 0 20px 48px #ffd54f, 0 0 60px #fffde7;
  transform: scale(1.07) rotate(2deg);
}

.message-text-block {
  flex: 1;
  background: #fff3e0cc;
  padding: 2rem 2.5rem;
  border-radius: 18px;
  box-shadow: 0 8px 24px rgba(255, 180, 0, 0.13);
  font-size: 1.25rem;
  font-weight: 500;
  color: #e65100;
  text-align: left;
  transition: box-shadow 0.3s, background 0.3s;
}

.member-name-large {
  font-size: 2.2rem;
  color: #ff7043;
  margin-bottom: 1rem;
  font-weight: bold;
  text-shadow: 0 2px 12px #ffe0b2;
}

.personal-message {
  font-size: 2rem;
  color: #444;
  line-height: 1.7;
  text-shadow: 0 1px 4px #fffde7;
}

.life-counter {
  margin-top: 3.5rem;
  text-align: center;
  padding: 2rem;
  background: linear-gradient(120deg, #fff9e6 60%, #ffe0b2 100%);
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(255, 193, 7, 0.13);
  animation: fadeIn 2s;
}

.life-counter .section-title {
  font-size: 2.1rem;
  color: #d84315;
  margin-bottom: 1.2rem;
  text-shadow: 0 2px 12px #ffe0b2;
}

.life-time {
  font-size: 1.7rem;
  font-weight: bold;
  color: #ff9800;
  text-shadow: 0 2px 8px #fffde7;
  letter-spacing: 1px;
}

@media (max-width: 1024px) {
  .content-container, .family-message-section {
    padding: 1.2rem;
  }
  .scratch-container {
    width: 95vw;
    max-width: 340px;
  }
  .message-photo {
    width: 200px;
    height: 200px;
  }
}

@media (max-width: 768px) {
  .message-block,
  .message-block.reverse {
    flex-direction: column !important;
    align-items: center;
    text-align: center;
    padding: 1rem 0.5rem;
  }
  .message-photo {
    width: 90vw;
    max-width: 280px;
    height: auto;
  }
  .message-text-block {
    margin-top: 1rem;
    padding: 1rem;
    font-size: 1rem;
  }
  .member-name-large {
    font-size: 1.3rem;
  }
  .personal-message {
    font-size: 1rem;
  }
  .scratch-container {
    width: 98vw;
    max-width: 340px;
  }
}

::-webkit-scrollbar {
  width: 10px;
  background: #ffe0b2;
}
::-webkit-scrollbar-thumb {
  background: #ffb74d;
  border-radius: 8px;
}

</style>
