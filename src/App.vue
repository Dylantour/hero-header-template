<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const menuOpen = ref(false)
const activeSlide = ref(1)
const hoveredSlide = ref(null)
const faqOpen = ref(null)
let timer = null

const companies = [
  {
    name: 'הפניקס',
    type: 'גוף מוסדי',
    text: 'בדיקת אפשרויות הקשורות לחיסכון ולקופות.',
    icon: '◈'
  },
  {
    name: 'הראל',
    type: 'גוף מוסדי',
    text: 'בחינת אפשרויות בהתאם לנתוני הקופה.',
    icon: '◇'
  },
  {
    name: 'מגדל',
    type: 'גוף מוסדי',
    text: 'אפשרויות בחיסכון ובמוצרים פיננסיים.',
    icon: '✦'
  },
  {
    name: 'כלל',
    type: 'גוף מוסדי',
    text: 'בדיקה והבנת האפשרויות הקיימות.',
    icon: '◉'
  },
  {
    name: 'מנורה',
    type: 'גוף מוסדי',
    text: 'בחינת אפשרויות בהתאם לתנאים הרלוונטיים.',
    icon: '◆'
  },
  {
    name: 'מיטב',
    type: 'בית השקעות',
    text: 'בחינת קופות וחסכונות בהתאם לנתונים.',
    icon: '✧'
  }
]

const services = [
  {
    title: 'משיכת כספים',
    text: 'בדיקה של אפשרויות המשיכה בהתאם לסוג הקופה, הכספים והנסיבות.',
    icon: '₪'
  },
  {
    title: 'קרנות השתלמות',
    text: 'הבנת מצב הקרן ובחינת האפשרויות הקיימות.',
    icon: '↗'
  },
  {
    title: 'קופות גמל',
    text: 'בדיקה של הכספים והאפשרויות שניתן לבחון.',
    icon: '◇'
  },
  {
    title: 'הלוואה כנגד קופה',
    text: 'בחינת אפשרות לקבלת מימון כנגד חיסכון קיים, בכפוף לתנאים.',
    icon: '+'
  }
]

const faqs = [
  {
    q: 'אפשר למשוך כסף מקופת גמל או קרן השתלמות?',
    a: 'זה תלוי בסוג המוצר, במועד ההפקדות, בסוג הכספים ובנסיבות האישיות. אנחנו מתחילים בבדיקה ומסבירים מה ניתן לבחון.'
  },
  {
    q: 'האם תמיד עדיף למשוך את הכסף?',
    a: 'לא. במקרים מסוימים ניתן לבחון חלופות כמו הלוואה כנגד הקופה. המטרה היא להבין את האפשרויות לפני שמקבלים החלטה.'
  },
  {
    q: 'האם הבדיקה הראשונית מחייבת?',
    a: 'לא. השארת הפרטים מאפשרת לנו להבין את הצורך ולבחון את האפשרויות הרלוונטיות.'
  },
  {
    q: 'כמה מס משלמים על משיכה?',
    a: 'המס, אם חל, משתנה בהתאם למוצר, לסוג הכספים ולנסיבות. לכן אין סכום אחיד שמתאים לכל לקוח.'
  }
]

const visibleCompanies = computed(() => {
  const result = []
  for (let i = -1; i <= 1; i++) {
    const index =
      (activeSlide.value + i + companies.length) % companies.length
    result.push({
      ...companies[index],
      position: i
    })
  }
  return result
})

function nextSlide() {
  activeSlide.value =
    (activeSlide.value + 1) % companies.length
}

function prevSlide() {
  activeSlide.value =
    (activeSlide.value - 1 + companies.length) % companies.length
}

function startSlider() {
  stopSlider()
  timer = setInterval(nextSlide, 3500)
}

function stopSlider() {
  if (timer) {
    clearInterval(timer)
    timer = null
  }
}

function toggleFaq(index) {
  faqOpen.value = faqOpen.value === index ? null : index
}

onMounted(startSlider)
onUnmounted(stopSlider)
</script>

<template>
  <div dir="rtl" class="site">

    <!-- HEADER -->
    <header class="header">
      <div class="nav">

        <a href="#" class="logo">
          <span class="logo-mark">פ</span>
          <span>
            <strong>פנסרה</strong>
            <small>פתרונות פיננסיים</small>
          </span>
        </a>

        <nav class="desktop-nav">
          <a href="#services">מה אפשר לעשות?</a>
          <a href="#how">איך זה עובד?</a>
          <a href="#companies">גופים מוסדיים</a>
          <a href="#faq">שאלות נפוצות</a>
        </nav>

        <a href="#contact" class="nav-button">
          להתחלת הבדיקה
          <span>«</span>
        </a>

        <button
          class="mobile-menu"
          @click="menuOpen = !menuOpen"
          aria-label="תפריט"
        >
          {{ menuOpen ? '×' : '☰' }}
        </button>

      </div>

      <div v-if="menuOpen" class="mobile-nav">
        <a href="#services" @click="menuOpen = false">מה אפשר לעשות?</a>
        <a href="#how" @click="menuOpen = false">איך זה עובד?</a>
        <a href="#companies" @click="menuOpen = false">גופים מוסדיים</a>
        <a href="#faq" @click="menuOpen = false">שאלות נפוצות</a>
      </div>
    </header>


    <!-- MOVING TOP STRIP -->
    <div class="ticker">
      <div class="ticker-track">
        <span>קרנות השתלמות</span>
        <b>»</b>
        <span>קופות גמל</span>
        <b>»</b>
        <span>משיכת כספים</span>
        <b>»</b>
        <span>הלוואות כנגד קופות</span>
        <b>»</b>
        <span>חיסכון פנסיוני</span>
        <b>»</b>

        <span>קרנות השתלמות</span>
        <b>»</b>
        <span>קופות גמל</span>
        <b>»</b>
        <span>משיכת כספים</span>
        <b>»</b>
        <span>הלוואות כנגד קופות</span>
        <b>»</b>
      </div>
    </div>


    <!-- HERO -->
    <section class="hero">

      <div class="hero-grid"></div>

      <div class="hero-glow glow-one"></div>
      <div class="hero-glow glow-two"></div>

      <div class="hero-content">

        <!-- FORM -->
        <div
          id="contact"
          class="lead-card"
          @mouseenter="stopSlider"
          @mouseleave="startSlider"
        >

          <div class="card-badge">
            <span></span>
            בדיקה ראשונית — ללא התחייבות
          </div>

          <h2>
            בואו נבדוק
            <br />
            <strong>מה אפשר לעשות</strong>
            עם הכסף שלכם.
          </h2>

          <p class="form-subtitle">
            משאירים פרטים. מבינים את המצב.
            בוחנים את האפשרויות.
          </p>

          <div class="progress">
            <span></span>
          </div>

          <label>שם מלא *</label>
          <input
            type="text"
            placeholder="לדוגמה: דני לוי"
          />

          <label>מספר טלפון *</label>
          <input
            type="tel"
            dir="ltr"
            placeholder="050-0000000"
          />

          <label>מה מעניין אתכם?</label>

          <select>
            <option>בחרו אפשרות</option>
            <option>משיכת כספים</option>
            <option>קרן השתלמות</option>
            <option>קופת גמל</option>
            <option>הלוואה כנגד קופה</option>
            <option>אני לא בטוח/ה</option>
          </select>

          <label class="check">
            <input type="checkbox" />
            <span>
              אני מאשר/ת יצירת קשר בהתאם למדיניות הפרטיות.
            </span>
          </label>

          <button class="submit-button">
            לשליחת הפרטים לבדיקה
            <span>«</span>
          </button>

          <div class="security">
            <span>🔒</span>
            <span>פרטים מאובטחים</span>
            <span>•</span>
            <span>ללא התחייבות</span>
          </div>

        </div>


        <!-- HERO TEXT -->
        <div class="hero-text">

          <div class="live-pill">
            <span class="live-dot"></span>
            מתחילים בבדיקה פשוטה
          </div>

          <p class="eyebrow">
            פנסרה
            <span>»</span>
          </p>

          <h1>
            יש לכם כסף
            <br />
            <span>בקופות?</span>
            <br />
            <strong>בואו נבדוק</strong>
            <br />
            מה אפשר לעשות איתו.
          </h1>

          <p class="hero-description">
            קרנות השתלמות, קופות גמל, חיסכון פנסיוני
            והלוואות כנגד קופות — אנחנו עוזרים לכם להבין
            את האפשרויות הקיימות ולבחון מה רלוונטי עבורכם.
          </p>

          <div class="hero-actions">

            <a href="#contact" class="primary-button">
              להתחלת הבדיקה
              <span>«</span>
            </a>

            <a href="#how" class="secondary-button">
              איך זה עובד?
              <span>↓</span>
            </a>

          </div>

          <div class="trust-row">
            <span>✓ תהליך פשוט</span>
            <span>✓ יחס אישי</span>
            <span>✓ מידע ברור</span>
          </div>

        </div>

      </div>

    </section>


    <!-- HOW -->
    <section id="how" class="section dark-section">

      <div class="section-heading">

        <p class="eyebrow">
          איך זה עובד?
          <span>»</span>
        </p>

        <h2>
          3 שלבים.
          <span>בלי כאב ראש.</span>
        </h2>

        <p>
          מתחילים בבדיקה. מבינים את הנתונים.
          ורק אז בוחנים את האפשרויות.
        </p>

      </div>


      <div class="steps">

        <div class="step">
          <div class="step-number">01</div>
          <div class="step-arrow">«</div>

          <h3>משאירים פרטים</h3>

          <p>
            שם, טלפון וכמה פרטים בסיסיים.
            זה הכול.
          </p>
        </div>


        <div class="step active-step">
          <div class="step-number">02</div>
          <div class="step-arrow">«</div>

          <h3>בודקים את האפשרויות</h3>

          <p>
            בוחנים את הנתונים ומבינים
            אילו אפשרויות ניתן לבדוק.
          </p>
        </div>


        <div class="step">
          <div class="step-number">03</div>

          <h3>מבינים ומחליטים</h3>

          <p>
            מסבירים את האפשרויות
            ואת המשמעות שלהן.
          </p>
        </div>

      </div>

    </section>


    <!-- SERVICES -->
    <section id="services" class="section dark-section services-section">

      <div class="section-heading right-heading">

        <p class="eyebrow">
          מה אפשר לעשות?
          <span>»</span>
        </p>

        <h2>
          הכסף שלכם.
          <br />
          <span>רק צריך לדעת איפה.</span>
        </h2>

      </div>


      <div class="services-grid">

        <article
          v-for="(service, index) in services"
          :key="service.title"
          class="service-card"
          :class="{ featured: index === 0 }"
        >

          <div class="service-icon">
            {{ service.icon }}
          </div>

          <div class="service-number">
            0{{ index + 1 }}
          </div>

          <h3>{{ service.title }}</h3>

          <p>{{ service.text }}</p>

          <a href="#contact">
            לבדיקה ראשונית
            <span>«</span>
          </a>

        </article>

      </div>

    </section>


    <!-- COMPANIES SLIDER -->
    <section
      id="companies"
      class="section dark-section company-section"
      @mouseenter="stopSlider"
      @mouseleave="startSlider"
    >

      <div class="section-heading">

        <p class="eyebrow">
          גופים מוסדיים
          <span>»</span>
        </p>

        <h2>
          בודקים את
          <span>האפשרויות.</span>
        </h2>

        <p>
          במידת הרלוונטיות, ניתן לבחון נתונים
          הקשורים לקופות ולחסכונות המנוהלים בגופים שונים.
        </p>

      </div>


      <div class="company-slider">

        <button
          class="slider-arrow arrow-right"
          @click="prevSlide"
          aria-label="הקודם"
        >
          «
        </button>

        <div class="company-track">

          <article
            v-for="company in visibleCompanies"
            :key="company.name"
            class="company-card"
            :class="{
              center: company.position === 0,
              dim: company.position !== 0,
              hovered: hoveredSlide === company.name
            }"
            @mouseenter="hoveredSlide = company.name"
            @mouseleave="hoveredSlide = null"
            @click="activeSlide = companies.findIndex(c => c.name === company.name)"
          >

            <div class="company-top">

              <div class="company-icon">
                {{ company.icon }}
              </div>

              <span class="company-type">
                {{ company.type }}
              </span>

            </div>

            <h3>
              {{ company.name }}
            </h3>

            <p>
              {{ company.text }}
            </p>

            <div class="company-bottom">
              <span>פרטים נוספים</span>
              <b>»</b>
            </div>

          </article>

        </div>


        <button
          class="slider-arrow arrow-left"
          @click="nextSlide"
          aria-label="הבא"
        >
          »
        </button>

      </div>


      <div class="slider-dots">

        <button
          v-for="(_, index) in companies"
          :key="index"
          :class="{ selected: index === activeSlide }"
          @click="activeSlide = index"
          :aria-label="`שקופית ${index + 1}`"
        ></button>

      </div>

    </section>


    <!-- CTA -->
    <section class="cta-section">

      <div class="cta-glow"></div>

      <div class="cta-content">

        <p class="eyebrow">
          לא יודעים מאיפה להתחיל?
        </p>

        <h2>
          לא צריך לנחש.
          <br />
          <span>מתחילים בבדיקה.</span>
        </h2>

        <p>
          השאירו פרטים ונבין יחד מה רלוונטי
          למצב שלכם.
        </p>

        <a href="#contact" class="primary-button">
          להתחלת הבדיקה
          <span>«</span>
        </a>

      </div>

    </section>


    <!-- FAQ -->
    <section id="faq" class="section faq-section">

      <div class="section-heading">

        <p class="eyebrow">
          שאלות נפוצות
          <span>»</span>
        </p>

        <h2>
          יש שאלה?
          <span>בדקנו גם אותה.</span>
        </h2>

      </div>


      <div class="faq-list">

        <div
          v-for="(faq, index) in faqs"
          :key="faq.q"
          class="faq-item"
          :class="{ open: faqOpen === index }"
        >

          <button
            @click="toggleFaq(index)"
          >

            <span>{{ faq.q }}</span>

            <b>
              {{ faqOpen === index ? '−' : '+' }}
            </b>

          </button>

          <div
            v-if="faqOpen === index"
            class="faq-answer"
          >
            {{ faq.a }}
          </div>

        </div>

      </div>

    </section>


    <!-- FOOTER -->
    <footer>

      <div class="footer-main">

        <div class="logo footer-logo">
          <span class="logo-mark">פ</span>
          <span>
            <strong>פנסרה</strong>
            <small>פתרונות פיננסיים</small>
          </span>
        </div>

        <div class="footer-links">
          <a href="#services">שירותים</a>
          <a href="#how">איך זה עובד?</a>
          <a href="#companies">גופים מוסדיים</a>
          <a href="#faq">שאלות נפוצות</a>
        </div>

      </div>

      <div class="footer-disclaimer">
        המידע באתר הינו כללי ואינו מהווה ייעוץ פנסיוני,
        ייעוץ השקעות, התחייבות לקבלת הלוואה או התחייבות
        לביצוע משיכה. האפשרויות והתנאים כפופים לנתונים
        האישיים ולתנאי הגופים הרלוונטיים.
      </div>

      <div class="footer-copy">
        © {{ new Date().getFullYear() }} פנסרה. כל הזכויות שמורות.
      </div>

    </footer>


    <!-- ACCESSIBILITY -->
    <button class="accessibility">
      i
    </button>

    <!-- FLOATING CTA -->
    <a href="#contact" class="floating-cta">
      בדיקה ראשונית
    </a>

  </div>
</template>


<style>
@import url('https://fonts.googleapis.com/css2?family=Heebo:wght@400;500;600;700;800;900&display=swap');

:root {
  --bg: #080b10;
  --bg-soft: #0d1118;
  --card: #171b21;
  --card-light: #20252c;
  --cyan: #37c9f5;
  --cyan-light: #5edcff;
  --text: #f5f7fa;
  --muted: #929aa8;
  --border: rgba(255,255,255,.11);
}

* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
  font-family: 'Heebo', Arial, sans-serif;
}

a {
  color: inherit;
  text-decoration: none;
}

button,
input,
select {
  font-family: inherit;
}


/* HEADER */

.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background: rgba(7,10,15,.88);
  border-bottom: 1px solid rgba(255,255,255,.08);
  backdrop-filter: blur(18px);
}

.nav {
  height: 72px;
  max-width: 1100px;
  margin: auto;
  padding: 0 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo-mark {
  width: 38px;
  height: 38px;
  border-radius: 11px;
  background: var(--cyan);
  color: #061018;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 900;
  font-size: 20px;
}

.logo strong {
  display: block;
  font-size: 20px;
  line-height: 20px;
}

.logo small {
  display: block;
  color: #727b88;
  font-size: 10px;
}

.desktop-nav {
  display: flex;
  align-items: center;
  gap: 30px;
  color: #9ba3b0;
  font-size: 14px;
  font-weight: 600;
}

.desktop-nav a {
  transition: .25s;
}

.desktop-nav a:hover {
  color: var(--cyan);
}

.nav-button,
.primary-button {
  background: var(--cyan);
  color: #061018;
  font-weight: 900;
  border-radius: 999px;
  padding: 12px 22px;
  transition: .3s;
}

.nav-button:hover,
.primary-button:hover {
  background: var(--cyan-light);
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(55,201,245,.2);
}

.nav-button span,
.primary-button span {
  margin-right: 8px;
}

.mobile-menu {
  display: none;
  background: transparent;
  border: 0;
  color: white;
  font-size: 25px;
}

.mobile-nav {
  padding: 20px 24px 25px;
  display: flex;
  flex-direction: column;
  gap: 18px;
  border-top: 1px solid var(--border);
}

.mobile-nav a {
  color: #b8bec8;
}


/* TICKER */

.ticker {
  position: relative;
  z-index: 5;
  margin-top: 72px;
  height: 50px;
  overflow: hidden;
  background: #0c1016;
  border-bottom: 1px solid var(--border);
}

.ticker-track {
  height: 100%;
  display: flex;
  width: max-content;
  align-items: center;
  gap: 30px;
  color: #777f8d;
  font-size: 13px;
  font-weight: 700;
  animation: ticker 28s linear infinite;
}

.ticker b {
  color: var(--cyan);
}

@keyframes ticker {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(50%);
  }
}


/* HERO */

.hero {
  position: relative;
  min-height: 790px;
  overflow: hidden;
  background:
    radial-gradient(circle at 76% 28%, rgba(26,134,164,.28), transparent 32%),
    radial-gradient(circle at 10% 80%, rgba(125,74,43,.16), transparent 30%),
    #090c11;
}

.hero-grid {
  position: absolute;
  inset: 0;
  opacity: .16;
  background-image:
    linear-gradient(rgba(255,255,255,.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,.08) 1px, transparent 1px);
  background-size: 72px 72px;
  mask-image: linear-gradient(to bottom, black, transparent 90%);
}

.hero-glow {
  position: absolute;
  border-radius: 50%;
  filter: blur(70px);
  pointer-events: none;
}

.glow-one {
  width: 400px;
  height: 400px;
  right: -120px;
  top: 100px;
  background: rgba(37,198,239,.12);
}

.glow-two {
  width: 300px;
  height: 300px;
  left: 10%;
  bottom: 30px;
  background: rgba(230,112,56,.07);
}

.hero-content {
  position: relative;
  z-index: 2;
  max-width: 1100px;
  min-height: 790px;
  margin: auto;
  padding: 75px 24px 70px;
  display: grid;
  grid-template-columns: 410px 1fr;
  align-items: center;
  gap: 100px;
}


/* FORM */

.lead-card {
  position: relative;
  background: #f8f9fb;
  color: #111722;
  border: 2px solid var(--cyan);
  border-radius: 18px;
  padding: 30px;
  box-shadow:
    0 0 0 5px rgba(55,201,245,.05),
    0 25px 80px rgba(0,0,0,.45),
    0 0 50px rgba(55,201,245,.13);
}

.card-badge {
  position: absolute;
  top: -15px;
  right: 25px;
  background: var(--cyan);
  color: #061018;
  padding: 6px 14px;
  border-radius: 999px;
  font-size: 12px;
  font-weight: 900;
}

.card-badge span {
  display: inline-block;
  width: 7px;
  height: 7px;
  margin-left: 6px;
  background: #063d4c;
  border-radius: 50%;
}

.lead-card h2 {
  margin: 12px 0 8px;
  font-size: 28px;
  line-height: 1.15;
  font-weight: 900;
}

.lead-card h2 strong {
  color: #1a98bf;
}

.form-subtitle {
  color: #687181;
  font-size: 14px;
  line-height: 1.7;
  margin-bottom: 18px;
}

.progress {
  height: 5px;
  background: #e1e5ea;
  border-radius: 10px;
  margin-bottom: 17px;
}

.progress span {
  display: block;
  width: 35%;
  height: 100%;
  border-radius: inherit;
  background: var(--cyan);
}

.lead-card label:not(.check) {
  display: block;
  margin: 12px 0 6px;
  font-size: 13px;
  font-weight: 800;
}

.lead-card input[type="text"],
.lead-card input[type="tel"],
.lead-card select {
  width: 100%;
  border: 1px solid #dbe0e7;
  border-radius: 10px;
  padding: 13px 14px;
  background: #f3f5f8;
  color: #17202c;
  outline: none;
  font-size: 15px;
  transition: .2s;
}

.lead-card input:focus,
.lead-card select:focus {
  border-color: var(--cyan);
  background: white;
  box-shadow: 0 0 0 3px rgba(55,201,245,.12);
}

.check {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  margin-top: 14px;
  color: #697382;
  font-size: 11px;
  line-height: 1.5;
}

.check input {
  margin-top: 3px;
}

.submit-button {
  width: 100%;
  margin-top: 17px;
  padding: 14px;
  border: 0;
  border-radius: 10px;
  background: #28afd6;
  color: #061018;
  font-size: 15px;
  font-weight: 900;
  cursor: pointer;
  transition: .3s;
}

.submit-button:hover {
  background: #42c9ed;
  box-shadow: 0 10px 30px rgba(40,175,214,.25);
}

.submit-button span {
  margin-right: 7px;
}

.security {
  margin-top: 18px;
  padding-top: 14px;
  border-top: 1px dashed #d5dae0;
  display: flex;
  justify-content: center;
  gap: 8px;
  color: #727b88;
  font-size: 11px;
}


/* HERO TEXT */

.hero-text {
  text-align: right;
}

.live-pill {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 7px 13px;
  border: 1px solid rgba(55,201,245,.25);
  background: rgba(55,201,245,.06);
  border-radius: 999px;
  color: #8da5b1;
  font-size: 12px;
}

.live-dot {
  width: 7px;
  height: 7px;
  background: var(--cyan);
  border-radius: 50%;
  box-shadow: 0 0 12px var(--cyan);
  animation: pulse 1.8s infinite;
}

@keyframes pulse {
  50% {
    opacity: .35;
    transform: scale(.75);
  }
}

.eyebrow {
  color: var(--cyan);
  font-size: 13px;
  font-weight: 900;
  letter-spacing: 2px;
  margin: 0 0 16px;
}

.eyebrow span {
  margin-right: 8px;
}

.hero-text .eyebrow {
  margin-top: 34px;
}

.hero-text h1 {
  margin: 0;
  font-size: clamp(48px, 5vw, 78px);
  line-height: .98;
  letter-spacing: -2px;
  font-weight: 900;
}

.hero-text h1 span {
  color: var(--cyan);
}

.hero-text h1 strong {
  color: white;
}

.hero-description {
  max-width: 610px;
  margin: 28px 0 0;
  color: #89919e;
  font-size: 17px;
  line-height: 1.9;
}

.hero-actions {
  margin-top: 32px;
  display: flex;
  gap: 12px;
  justify-content: flex-start;
}

.primary-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.secondary-button {
  padding: 12px 22px;
  border: 1px solid rgba(255,255,255,.15);
  border-radius: 999px;
  color: #b2bac5;
  transition: .3s;
}

.secondary-button:hover {
  border-color: var(--cyan);
  color: var(--cyan);
}

.trust-row {
  margin-top: 25px;
  display: flex;
  gap: 25px;
  color: #69727e;
  font-size: 12px;
}


/* SECTIONS */

.section {
  padding: 125px 24px;
  position: relative;
}

.dark-section {
  background: #0b0e14;
  border-top: 1px solid rgba(255,255,255,.05);
}

.section-heading {
  max-width: 900px;
  margin: 0 auto 60px;
  text-align: center;
}

.section-heading h2 {
  margin: 0;
  font-size: clamp(42px, 5vw, 66px);
  line-height: 1;
  font-weight: 900;
  letter-spacing: -2px;
}

.section-heading h2 span {
  color: var(--cyan);
}

.section-heading p:last-child {
  max-width: 620px;
  margin: 22px auto 0;
  color: #7f8793;
  line-height: 1.8;
}


/* STEPS */

.steps {
  max-width: 1040px;
  margin: auto;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}

.step {
  position: relative;
  min-height: 210px;
  padding: 30px;
  background: #171a20;
  border: 1px solid var(--border);
  border-radius: 17px;
  transition: .35s;
}

.step:hover {
  transform: translateY(-7px);
  border-color: rgba(55,201,245,.5);
  box-shadow: 0 20px 45px rgba(0,0,0,.3);
}

.active-step {
  border-color: var(--cyan);
  box-shadow: 0 15px 45px rgba(55,201,245,.09);
}

.step-number {
  color: var(--cyan);
  font-size: 37px;
  font-weight: 900;
}

.step-arrow {
  position: absolute;
  left: 25px;
  top: 35px;
  color: var(--cyan);
  font-size: 27px;
  opacity: .65;
}

.step h3 {
  font-size: 20px;
  margin: 25px 0 8px;
}

.step p {
  color: #777f8c;
  line-height: 1.7;
  margin: 0;
}


/* SERVICES */

.services-section {
  background:
    radial-gradient(circle at 80% 20%, rgba(30,154,193,.08), transparent 30%),
    #0b0e14;
}

.right-heading {
  text-align: right;
}

.right-heading p:last-child {
  margin-right: 0;
}

.services-grid {
  max-width: 1040px;
  margin: auto;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

.service-card {
  min-height: 270px;
  position: relative;
  overflow: hidden;
  padding: 30px;
  border: 1px solid var(--border);
  border-radius: 18px;
  background: #171a20;
  transition: .4s;
}

.service-card:hover {
  transform: translateY(-7px) scale(1.015);
  border-color: rgba(55,201,245,.65);
  box-shadow: 0 0 40px rgba(55,201,245,.08);
}

.service-card.featured {
  background: #1d2229;
  border-color: rgba(55,201,245,.45);
}

.service-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(55,201,245,.13);
  color: var(--cyan);
  font-size: 22px;
  font-weight: 900;
}

.service-number {
  position: absolute;
  left: 25px;
  top: 20px;
  color: rgba(255,255,255,.05);
  font-size: 65px;
  font-weight: 900;
}

.service-card h3 {
  margin: 32px 0 7px;
  font-size: 23px;
}

.service-card p {
  max-width: 500px;
  color: #858d99;
  line-height: 1.8;
}

.service-card a {
  display: inline-block;
  margin-top: 15px;
  color: var(--cyan);
  font-weight: 800;
}

.service-card a span {
  margin-right: 8px;
}


/* COMPANY SLIDER */

.company-section {
  overflow: hidden;
  background:
    radial-gradient(circle at 50% 60%, rgba(55,201,245,.05), transparent 35%),
    #0b0e14;
}

.company-slider {
  position: relative;
  max-width: 1100px;
  margin: auto;
  display: flex;
  align-items: center;
}

.company-track {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 25px;
}

.company-card {
  min-height: 280px;
  padding: 28px;
  background: #171a20;
  border: 1px solid rgba(255,255,255,.09);
  border-radius: 18px;
  transition:
    transform .5s cubic-bezier(.2,.8,.2,1),
    opacity .5s,
    box-shadow .5s,
    border-color .5s;
  cursor: pointer;
}

.company-card.dim {
  opacity: .55;
  transform: scale(.91);
}

.company-card.center {
  opacity: 1;
  transform: scale(1.03);
  border-color: var(--cyan);
  box-shadow:
    0 0 0 1px rgba(55,201,245,.15),
    0 20px 70px rgba(55,201,245,.12);
}

.company-card.hovered {
  transform: scale(1.075) translateY(-8px);
  z-index: 5;
  opacity: 1;
  border-color: var(--cyan);
  box-shadow:
    0 0 35px rgba(55,201,245,.16),
    0 25px 60px rgba(0,0,0,.45);
}

.company-card.dim.hovered {
  transform: scale(1.03) translateY(-6px);
}

.company-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.company-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: #203e48;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--cyan);
  font-size: 22px;
  box-shadow: inset 0 0 20px rgba(55,201,245,.06);
}

.company-type {
  color: #68727f;
  font-size: 11px;
}

.company-card h3 {
  margin: 50px 0 8px;
  font-size: 27px;
  font-weight: 900;
}

.company-card p {
  color: #858e9a;
  line-height: 1.7;
  min-height: 55px;
}

.company-bottom {
  margin-top: 20px;
  padding-top: 15px;
  border-top: 1px dashed rgba(255,255,255,.1);
  display: flex;
  justify-content: space-between;
  color: #67717e;
  font-size: 12px;
}

.company-bottom b {
  color: var(--cyan);
  font-size: 20px;
}

.slider-arrow {
  position: absolute;
  z-index: 10;
  width: 54px;
  height: 54px;
  border: 1px solid rgba(55,201,245,.35);
  border-radius: 50%;
  background: #111820;
  color: var(--cyan);
  font-size: 25px;
  cursor: pointer;
  box-shadow: 0 0 25px rgba(55,201,245,.1);
  transition: .3s;
}

.slider-arrow:hover {
  transform: scale(1.15);
  background: var(--cyan);
  color: #061018;
  box-shadow: 0 0 35px rgba(55,201,245,.4);
}

.arrow-right {
  right: -27px;
}

.arrow-left {
  left: -27px;
}

.slider-dots {
  margin-top: 35px;
  display: flex;
  justify-content: center;
  gap: 8px;
}

.slider-dots button {
  width: 7px;
  height: 7px;
  padding: 0;
  border: 0;
  border-radius: 50%;
  background: #3c444e;
  cursor: pointer;
  transition: .3s;
}

.slider-dots button.selected {
  width: 24px;
  border-radius: 20px;
  background: var(--cyan);
  box-shadow: 0 0 12px rgba(55,201,245,.5);
}


/* CTA */

.cta-section {
  position: relative;
  overflow: hidden;
  padding: 130px 24px;
  text-align: center;
  background:
    radial-gradient(circle at 50% 100%, rgba(55,201,245,.15), transparent 45%),
    #080b10;
}

.cta-glow {
  position: absolute;
  width: 400px;
  height: 400px;
  left: 50%;
  top: 40%;
  transform: translate(-50%,-50%);
  border-radius: 50%;
  background: rgba(55,201,245,.06);
  filter: blur(70px);
}

.cta-content {
  position: relative;
  z-index: 2;
}

.cta-content h2 {
  margin: 0;
  font-size: clamp(45px, 6vw, 76px);
  line-height: 1;
}

.cta-content h2 span {
  color: var(--cyan);
}

.cta-content > p:not(.eyebrow) {
  color: #858e9a;
  font-size: 17px;
  margin: 25px auto 35px;
}

.cta-content .primary-button {
  display: inline-flex;
}


/* FAQ */

.faq-section {
  background: #f4f6f8;
  color: #10151d;
}

.faq-section .eyebrow {
  color: #1195ba;
}

.faq-section .section-heading h2 {
  color: #10151d;
}

.faq-section .section-heading h2 span {
  color: #119bc1;
}

.faq-list {
  max-width: 820px;
  margin: auto;
}

.faq-item {
  margin-bottom: 10px;
  background: white;
  border: 1px solid #e0e4e8;
  border-radius: 14px;
  overflow: hidden;
}

.faq-item button {
  width: 100%;
  padding: 23px 25px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 0;
  background: white;
  text-align: right;
  cursor: pointer;
  color: #151b24;
  font-size: 16px;
  font-weight: 800;
}

.faq-item button b {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #eaf8fc;
  color: #159cc3;
  font-size: 20px;
}

.faq-answer {
  padding: 0 25px 23px;
  color: #68727e;
  line-height: 1.8;
}


/* FOOTER */

footer {
  background: #070a0f;
  padding: 45px 24px 30px;
  border-top: 1px solid var(--border);
}

.footer-main {
  max-width: 1100px;
  margin: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.footer-links {
  display: flex;
  gap: 25px;
  color: #69727e;
  font-size: 13px;
}

.footer-links a:hover {
  color: var(--cyan);
}

.footer-disclaimer,
.footer-copy {
  max-width: 1100px;
  margin: 30px auto 0;
  color: #4f5864;
  font-size: 11px;
  line-height: 1.7;
}

.footer-copy {
  margin-top: 15px;
}


/* FLOATING */

.accessibility {
  position: fixed;
  z-index: 90;
  bottom: 25px;
  right: 18px;
  width: 43px;
  height: 43px;
  border-radius: 50%;
  border: 2px solid white;
  background: #1455d8;
  color: white;
  font-weight: 900;
  cursor: pointer;
}

.floating-cta {
  position: fixed;
  z-index: 90;
  bottom: 22px;
  left: 18px;
  padding: 10px 18px;
  border-radius: 8px;
  background: var(--cyan);
  color: #061018;
  font-size: 13px;
  font-weight: 900;
  box-shadow: 0 8px 25px rgba(0,0,0,.35);
}


/* MOBILE */

@media (max-width: 850px) {

  .desktop-nav,
  .nav-button {
    display: none;
  }

  .mobile-menu {
    display: block;
  }

  .hero-content {
    grid-template-columns: 1fr;
    gap: 55px;
    padding-top: 70px;
  }

  .hero {
    min-height: auto;
  }

  .hero-content {
    min-height: auto;
  }

  .hero-text {
    order: -1;
  }

  .hero-text h1 {
    font-size: 48px;
  }

  .hero-description {
    font-size: 15px;
  }

  .lead-card {
    max-width: 500px;
    width: 100%;
    margin: auto;
  }

  .steps {
    grid-template-columns: 1fr;
  }

  .services-grid {
    grid-template-columns: 1fr;
  }

  .company-track {
    grid-template-columns: 1fr;
  }

  .company-card.dim {
    display: none;
  }

  .company-card.center {
    transform: scale(1);
  }

  .arrow-right {
    right: -10px;
  }

  .arrow-left {
    left: -10px;
  }

  .footer-main {
    flex-direction: column;
    align-items: flex-start;
    gap: 25px;
  }

  .footer-links {
    flex-wrap: wrap;
  }
}


@media (max-width: 500px) {

  .section {
    padding: 90px 18px;
  }

  .hero-content {
    padding-left: 18px;
    padding-right: 18px;
  }

  .hero-text h1 {
    font-size: 43px;
    letter-spacing: -1px;
  }

  .hero-actions {
    flex-direction: column;
  }

  .primary-button,
  .secondary-button {
    text-align: center;
    width: 100%;
  }

  .trust-row {
    flex-wrap: wrap;
    gap: 10px 18px;
  }

  .lead-card {
    padding: 24px 19px;
  }

  .section-heading h2 {
    font-size: 42px;
  }

  .company-card {
    min-height: 300px;
  }

  .floating-cta {
    bottom: 15px;
    left: 15px;
  }

  .accessibility {
    bottom: 15px;
    right: 15px;
  }
}


@media (prefers-reduced-motion: reduce) {

  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    animation-duration: .01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: .01ms !important;
  }

}
</style>
