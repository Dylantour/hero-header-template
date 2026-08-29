<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const language = ref('he')
const openFaq = ref(null)
const mobileMenu = ref(false)
const formSent = ref(false)
const currentSlide = ref(0)

const languages = [
  { id: 'he', label: 'עברית', flag: '🇮🇱' },
  { id: 'ru', label: 'Русский', flag: '🇷🇺' },
  { id: 'ar', label: 'العربية', flag: null }
]

const services = [
  'איתור כספים',
  'קרנות השתלמות',
  'קופות גמל',
  'קרנות פנסיה',
  'בדיקת אפשרות למשיכה',
  'הלוואה כנגד קופה',
  'חסכונות פנסיוניים',
  'בדיקה ללא עלות וללא התחייבות'
]

const translations = {
  he: {
    navHome: 'ראשי',
    navServices: 'השירותים שלנו',
    navHow: 'איך זה עובד',
    navFaq: 'שאלות נפוצות',
    navContact: 'השארת פרטים',

    badge: 'בדיקה ללא עלות וללא התחייבות',

    heroTitle: 'יש לכם כספים בקופות?',
    heroTitle2: 'בואו נבדוק מה אפשר לעשות איתם.',
    heroText:
      'אנחנו עוזרים לכם לאתר כספים וחסכונות, להבין מה נמצא שם ולבחון אילו אפשרויות עשויות להיות רלוונטיות עבורכם.',
    cta: 'בדיקה ללא עלות',
    secondary: 'איך זה עובד?',

    stats1: 'משיכות שבוצעו',
    stats2: 'בדיקות בתהליך',
    stats3: 'לקוחות שקיבלו מענה',

    sliderTitle: 'עושים סדר בכספים שלכם',
    sliderText:
      'בדיקה פשוטה וברורה שמתחילה ללא עלות וללא התחייבות.',

    servicesTitle: 'מה אפשר לבדוק?',
    servicesText:
      'אנחנו מתחילים בבדיקה ומסבירים לכם בצורה פשוטה מה האפשרויות.',

    service1: 'איתור כספים',
    service1Text:
      'בדיקה ואיתור של חסכונות וקופות שעשויים להיות רשומים על שמכם.',
    service2: 'בדיקת אפשרות למשיכה',
    service2Text:
      'הבנת האפשרויות הקיימות סביב הכספים והאם קיימת אפשרות למשיכה.',
    service3: 'הלוואה כנגד קופה',
    service3Text:
      'בדיקה האם קיימת אפשרות להלוואה כנגד כספים בקופות מסוימות.',
    service4: 'סדר והבנה',
    service4Text:
      'להבין מה יש לכם, איפה הכספים נמצאים ומה אפשר לבדוק הלאה.',

    howTitle: 'איך זה עובד?',
    step1: 'משאירים פרטים',
    step1Text: 'פחות מדקה.',
    step2: 'נציג חוזר אליכם',
    step2Text: 'בדרך כלל בתוך 24 שעות.',
    step3: 'בודקים את האפשרויות',
    step3Text: 'מקבלים הסבר ברור.',
    step4: 'אתם מחליטים',
    step4Text: 'ללא התחייבות להמשיך.',

    faqTitle: 'שאלות נפוצות',

    faq1Q: 'כמה זמן זה לוקח?',
    faq1A:
      'מילוי הטופס — פחות מדקה. נציג יחזור תוך 24 שעות (ימים א׳-ה׳, 9:00-18:00). משיכת הכסף — בדרך כלל 7-14 ימי עסקים, בכפוף לתנאים ולגוף המנהל.',

    faq2Q: 'איך זה יכול להיות בחינם?',
    faq2A:
      'הבדיקה היא חינמית לגמרי — הבדיקה וההדרכה על הכספים שאיתרנו — לגמרי חינם. במקרים שנדרש יועץ או בעל רישיון להשלמת פעולה, נציג בפניכם את האפשרויות והעלויות, ותחליטו אם להמשיך.',

    faq3Q: 'האם אני חייב למשוך את הכסף?',
    faq3A:
      'לא. הבדיקה אינה מחייבת אתכם למשוך כסף. המטרה היא להבין מה קיים ומה האפשרויות העומדות בפניכם.',

    faq4Q: 'האם אפשר לקבל הלוואה כנגד קופה?',
    faq4A:
      'בחלק מהמוצרים והקופות עשויה להיות אפשרות לקבלת הלוואה כנגד הכספים. הזכאות והתנאים נקבעים על ידי הגוף המנהל.',

    faq5Q: 'האם יש התחייבות?',
    faq5A:
      'לא. הבדיקה הראשונית היא ללא עלות וללא התחייבות.',

    formTitle: 'רוצים לבדוק מה יש לכם?',
    formText:
      'השאירו פרטים ונציג יחזור אליכם. הבדיקה הראשונית ללא עלות וללא התחייבות.',
    name: 'שם מלא',
    phone: 'טלפון',
    submit: 'בדיקה ללא עלות',

    successTitle: 'הפרטים התקבלו בהצלחה',
    successText: 'נציג יחזור אליכם בהקדם.',

    footer:
      'פנסרה מספקת שירותי מידע, איתור והכוונה. שירותים הדורשים רישיון יינתנו באמצעות בעל רישיון מתאים, ככל שנדרש על פי דין.'
  },

  ru: {
    navHome: 'Главная',
    navServices: 'Услуги',
    navHow: 'Как это работает',
    navFaq: 'Вопросы',
    navContact: 'Оставить данные',

    badge: 'Проверка бесплатно и без обязательств',

    heroTitle: 'У вас есть деньги в кассах?',
    heroTitle2: 'Давайте проверим, что можно с ними сделать.',
    heroText:
      'Помогаем найти накопления, понять что у вас есть и какие варианты могут быть доступны.',
    cta: 'Бесплатная проверка',
    secondary: 'Как это работает?',

    stats1: 'Выполненные снятия',
    stats2: 'Проверки в процессе',
    stats3: 'Клиенты получили ответ',

    sliderTitle: 'Наводим порядок в ваших накоплениях',
    sliderText:
      'Простая и понятная проверка, которая начинается бесплатно.',

    servicesTitle: 'Что можно проверить?',
    servicesText:
      'Начинаем с проверки и простыми словами объясняем возможные варианты.',

    service1: 'Поиск денег',
    service1Text:
      'Проверка накоплений и касс, которые могут быть записаны на ваше имя.',
    service2: 'Проверка возможности снятия',
    service2Text:
      'Объясняем возможные варианты использования накоплений.',
    service3: 'Кредит под накопления',
    service3Text:
      'Проверяем возможность кредита под деньги в определённых кассах.',
    service4: 'Порядок и понимание',
    service4Text:
      'Помогаем понять, что у вас есть и что можно проверить дальше.',

    howTitle: 'Как это работает?',
    step1: 'Оставляете данные',
    step1Text: 'Меньше минуты.',
    step2: 'Представитель связывается',
    step2Text: 'Обычно в течение 24 часов.',
    step3: 'Проверяем варианты',
    step3Text: 'Получаете понятное объяснение.',
    step4: 'Решаете сами',
    step4Text: 'Без обязательств.',

    faqTitle: 'Частые вопросы',

    faq1Q: 'Сколько это занимает?',
    faq1A:
      'Заполнение формы — меньше минуты. Представитель свяжется с вами в течение 24 часов (вс–чт, 9:00–18:00). Получение денег обычно занимает 7–14 рабочих дней.',

    faq2Q: 'Как это может быть бесплатно?',
    faq2A:
      'Первичная проверка полностью бесплатна. Если для дальнейшего действия потребуется лицензированный специалист, вам объяснят варианты и стоимость, и вы сами решите, продолжать или нет.',

    faq3Q: 'Я обязан снимать деньги?',
    faq3A:
      'Нет. Проверка не обязывает вас снимать деньги.',

    faq4Q: 'Можно ли получить кредит под кассу?',
    faq4A:
      'В некоторых продуктах может существовать такая возможность. Условия зависят от конкретной кассы.',

    faq5Q: 'Есть ли обязательство?',
    faq5A:
      'Нет. Первичная проверка бесплатна и без обязательств.',

    formTitle: 'Хотите проверить свои накопления?',
    formText:
      'Оставьте данные, и представитель свяжется с вами.',
    name: 'Имя и фамилия',
    phone: 'Телефон',
    submit: 'Бесплатная проверка',

    successTitle: 'Данные получены',
    successText: 'Мы свяжемся с вами в ближайшее время.',

    footer:
      'Пансера предоставляет информационные услуги, поиск и навигацию. Услуги, требующие лицензии, предоставляются соответствующим специалистом.'
  },

  ar: {
    navHome: 'الرئيسية',
    navServices: 'الخدمات',
    navHow: 'كيف يعمل؟',
    navFaq: 'الأسئلة الشائعة',
    navContact: 'ترك التفاصيل',

    badge: 'فحص مجاني وبدون التزام',

    heroTitle: 'لديكم أموال في الصناديق؟',
    heroTitle2: 'دعونا نفحص ما يمكن فعله بها.',
    heroText:
      'نساعدكم في العثور على المدخرات وفهم ما لديكم وما هي الخيارات التي قد تكون متاحة لكم.',
    cta: 'فحص مجاني',
    secondary: 'كيف يعمل؟',

    stats1: 'عمليات سحب تمت',
    stats2: 'فحوصات قيد المعالجة',
    stats3: 'عملاء حصلوا على رد',

    sliderTitle: 'نرتب الصورة المالية لكم',
    sliderText:
      'فحص بسيط وواضح يبدأ مجاناً وبدون التزام.',

    servicesTitle: 'ماذا يمكننا فحصه؟',
    servicesText:
      'نبدأ بالفحص ونشرح لكم الخيارات بطريقة بسيطة.',

    service1: 'العثور على الأموال',
    service1Text:
      'فحص المدخرات والصناديق التي قد تكون مسجلة باسمكم.',
    service2: 'فحص إمكانية السحب',
    service2Text:
      'شرح الخيارات الممكنة المتعلقة بالأموال الموجودة.',
    service3: 'قرض مقابل الصندوق',
    service3Text:
      'فحص إمكانية الحصول على قرض مقابل الأموال الموجودة في بعض الصناديق.',
    service4: 'ترتيب وفهم',
    service4Text:
      'نساعدكم على فهم ما لديكم وما الذي يمكن فحصه بعد ذلك.',

    howTitle: 'كيف يعمل الأمر؟',
    step1: 'تتركون التفاصيل',
    step1Text: 'أقل من دقيقة.',
    step2: 'نتواصل معكم',
    step2Text: 'عادة خلال 24 ساعة.',
    step3: 'نفحص الخيارات',
    step3Text: 'تحصلون على شرح واضح.',
    step4: 'أنتم تقررون',
    step4Text: 'بدون التزام.',

    faqTitle: 'الأسئلة الشائعة',

    faq1Q: 'كم يستغرق الأمر؟',
    faq1A:
      'تعبئة النموذج — أقل من دقيقة. سيتواصل معكم ممثل خلال 24 ساعة (الأحد–الخميس، 9:00–18:00). سحب الأموال يستغرق عادةً 7–14 يوم عمل.',

    faq2Q: 'كيف يمكن أن يكون الفحص مجانياً؟',
    faq2A:
      'الفحص الأولي مجاني تماماً. إذا احتاجت العملية إلى مختص مرخص، سنوضح لكم الخيارات والتكلفة وأنتم تقررون إذا كنتم تريدون المتابعة.',

    faq3Q: 'هل يجب عليّ سحب الأموال؟',
    faq3A:
      'لا. الفحص لا يلزمكم بسحب الأموال.',

    faq4Q: 'هل يمكن الحصول على قرض مقابل الصندوق؟',
    faq4A:
      'في بعض المنتجات قد تكون هناك إمكانية للحصول على قرض مقابل المدخرات. الشروط تعتمد على الصندوق.',

    faq5Q: 'هل يوجد أي التزام؟',
    faq5A:
      'لا. الفحص الأولي مجاني وبدون أي التزام.',

    formTitle: 'هل تريدون فحص مدخراتكم؟',
    formText:
      'اتركوا التفاصيل وسيتواصل معكم ممثل.',
    name: 'الاسم الكامل',
    phone: 'الهاتف',
    submit: 'فحص مجاني',

    successTitle: 'تم استلام البيانات',
    successText: 'سنتواصل معكم في أقرب وقت.',

    footer:
      'بانسرا تقدم خدمات معلومات وبحث وتوجيه. الخدمات التي تتطلب ترخيصاً يتم تقديمها بواسطة مختص مرخص عند الحاجة.'
  }
}

const t = computed(() => translations[language.value])

const direction = computed(() =>
  language.value === 'ru' ? 'ltr' : 'rtl'
)

const currentServices = computed(() => [
  {
    icon: '⌕',
    title: t.value.service1,
    text: t.value.service1Text
  },
  {
    icon: '₪',
    title: t.value.service2,
    text: t.value.service2Text
  },
  {
    icon: '↗',
    title: t.value.service3,
    text: t.value.service3Text
  },
  {
    icon: '✓',
    title: t.value.service4,
    text: t.value.service4Text
  }
])

const steps = computed(() => [
  {
    number: '01',
    title: t.value.step1,
    text: t.value.step1Text
  },
  {
    number: '02',
    title: t.value.step2,
    text: t.value.step2Text
  },
  {
    number: '03',
    title: t.value.step3,
    text: t.value.step3Text
  },
  {
    number: '04',
    title: t.value.step4,
    text: t.value.step4Text
  }
])

const faqs = computed(() => [
  { q: t.value.faq1Q, a: t.value.faq1A },
  { q: t.value.faq2Q, a: t.value.faq2A },
  { q: t.value.faq3Q, a: t.value.faq3A },
  { q: t.value.faq4Q, a: t.value.faq4A },
  { q: t.value.faq5Q, a: t.value.faq5A }
])

function changeLanguage(id) {
  language.value = id
}

function toggleFaq(index) {
  openFaq.value =
    openFaq.value === index
      ? null
      : index
}

function submitForm() {
  formSent.value = true

  setTimeout(() => {
    formSent.value = false
  }, 5000)
}

let sliderTimer

onMounted(() => {
  sliderTimer = setInterval(() => {
    currentSlide.value =
      (currentSlide.value + 1) % services.length
  }, 2400)
})

onUnmounted(() => {
  clearInterval(sliderTimer)
})
</script>

<template>
  <div
    class="site"
    :dir="direction"
    :lang="language"
  >

    <!-- NAVBAR -->

    <header class="navbar">
      <div class="nav-inner">

        <a
          href="#home"
          class="logo"
        >
          <span class="logo-mark">
            פ
          </span>

          <span>
            פנסרה
          </span>
        </a>

        <nav
          class="nav-links"
          :class="{ open: mobileMenu }"
        >
          <a href="#home">
            {{ t.navHome }}
          </a>

          <a href="#services">
            {{ t.navServices }}
          </a>

          <a href="#how">
            {{ t.navHow }}
          </a>

          <a href="#faq">
            {{ t.navFaq }}
          </a>

          <a href="#contact">
            {{ t.navContact }}
          </a>
        </nav>

        <!-- LANGUAGES -->

        <div class="languages">

          <button
            v-for="item in languages"
            :key="item.id"
            class="language-button"
            :class="{ active: language === item.id }"
            @click="changeLanguage(item.id)"
          >
            <span
              v-if="item.flag"
              class="flag"
            >
              {{ item.flag }}
            </span>

            <span
              v-else
              class="arabic-symbol"
            >
              ع
            </span>

            <span class="language-name">
              {{ item.label }}
            </span>
          </button>

        </div>

        <button
          class="hamburger"
          @click="mobileMenu = !mobileMenu"
        >
          <span></span>
          <span></span>
          <span></span>
        </button>

      </div>
    </header>

    <!-- HERO -->

    <section
      id="home"
      class="hero"
    >

      <div class="hero-orb orb-1"></div>
      <div class="hero-orb orb-2"></div>

      <div class="hero-inner">

        <div class="hero-content">

          <div class="free-badge">
            <span class="live-dot"></span>
            {{ t.badge }}
          </div>

          <h1>
            {{ t.heroTitle }}
            <br />
            <span>
              {{ t.heroTitle2 }}
            </span>
          </h1>

          <p class="hero-description">
            {{ t.heroText }}
          </p>

          <div class="hero-buttons">

            <a
              href="#contact"
              class="primary-button"
            >
              {{ t.cta }}

              <span>
                ←
              </span>
            </a>

            <a
              href="#how"
              class="secondary-button"
            >
              {{ t.secondary }}
            </a>

          </div>

          <div class="hero-checks">

            <span>
              ✓ {{ t.badge }}
            </span>

            <span>
              ✓ ללא התחייבות
            </span>

          </div>

        </div>

        <!-- HERO CARD -->

        <div class="hero-visual">

          <div class="glow-ring"></div>

          <div class="finance-card">

            <div class="finance-card-top">
              <strong>
                פנסרה
              </strong>

              <span class="verified">
                ✓
              </span>
            </div>

            <div class="finance-icon">
              ₪
            </div>

            <div class="finance-lines">
              <span></span>
              <span></span>
              <span></span>
            </div>

            <div class="finance-bottom">
              <span>
                חסכונות וקופות
              </span>

              <strong>
                בדיקה
              </strong>
            </div>

          </div>

          <div class="floating-stat floating-top">
            <div class="float-icon">
              ₪
            </div>

            <div>
              <strong>
                בדיקה
              </strong>

              <small>
                ללא עלות
              </small>
            </div>
          </div>

          <div class="floating-stat floating-bottom">
            <div class="float-check">
              ✓
            </div>

            <div>
              <strong>
                ללא התחייבות
              </strong>

              <small>
                פשוט וברור
              </small>
            </div>
          </div>

        </div>

      </div>

      <!-- HERO STATS -->

      <div class="stats-bar">

        <div class="stats-inner">

          <div class="stat">
            <strong>
              386
            </strong>

            <span>
              {{ t.stats1 }}
            </span>
          </div>

          <div class="stat-divider"></div>

          <div class="stat">
            <strong>
              24
            </strong>

            <span>
              {{ t.stats2 }}
            </span>
          </div>

          <div class="stat-divider"></div>

          <div class="stat">
            <strong>
              —
            </strong>

            <span>
              {{ t.stats3 }}
            </span>
          </div>

        </div>

      </div>

    </section>

    <!-- RUNNING SLIDER -->

    <section class="ticker-section">

      <div class="ticker-glow"></div>

      <div class="section-heading compact">

        <span class="eyebrow">
          פנסרה
        </span>

        <h2>
          {{ t.sliderTitle }}
        </h2>

        <p>
          {{ t.sliderText }}
        </p>

      </div>

      <div class="ticker-window">

        <div
          class="ticker-track"
          :style="{
            transform:
              `translateX(-${currentSlide * 25}%)`
          }"
        >

          <!-- duplicated items for seamless visual -->

          <div
            v-for="(item, index) in [...services, ...services]"
            :key="index"
            class="ticker-card"
          >

            <span class="ticker-icon">
              ✦
            </span>

            <strong>
              {{ item }}
            </strong>

            <span class="ticker-arrow">
              →
            </span>

          </div>

        </div>

      </div>

    </section>

    <!-- SERVICES -->

    <section
      id="services"
      class="services-section"
    >

      <div class="section-heading">

        <span class="eyebrow">
          השירותים שלנו
        </span>

        <h2>
          {{ t.servicesTitle }}
        </h2>

        <p>
          {{ t.servicesText }}
        </p>

      </div>

      <div class="services-grid">

        <article
          v-for="(service, index) in currentServices"
          :key="index"
          class="service-card"
        >

          <span class="service-number">
            0{{ index + 1 }}
          </span>

          <div class="service-icon">
            {{ service.icon }}
          </div>

          <h3>
            {{ service.title }}
          </h3>

          <p>
            {{ service.text }}
          </p>

          <div class="service-bottom">
            <span></span>
            →
          </div>

        </article>

      </div>

    </section>

    <!-- HOW -->

    <section
      id="how"
      class="how-section"
    >

      <div class="section-heading">

        <span class="eyebrow">
          פשוט וברור
        </span>

        <h2>
          {{ t.howTitle }}
        </h2>

      </div>

      <div class="steps">

        <article
          v-for="(step, index) in steps"
          :key="index"
          class="step"
        >

          <div class="step-circle">
            {{ step.number }}
          </div>

          <h3>
            {{ step.title }}
          </h3>

          <p>
            {{ step.text }}
          </p>

          <div
            v-if="index < 3"
            class="step-connector"
          ></div>

        </article>

      </div>

    </section>

    <!-- CTA -->

    <section class="big-cta">

      <div class="cta-glow"></div>

      <div class="cta-inner">

        <div class="cta-check">
          ✓
        </div>

        <div class="cta-copy">

          <span>
            {{ t.badge }}
          </span>

          <h2>
            {{ t.formTitle }}
          </h2>

          <p>
            {{ t.formText }}
          </p>

        </div>

        <a
          href="#contact"
          class="white-button"
        >
          {{ t.cta }}
          <span>
            ←
          </span>
        </a>

      </div>

    </section>

    <!-- FAQ -->

    <section
      id="faq"
      class="faq-section"
    >

      <div class="section-heading">

        <span class="eyebrow">
          FAQ
        </span>

        <h2>
          {{ t.faqTitle }}
        </h2>

      </div>

      <div class="faq-list">

        <article
          v-for="(faq, index) in faqs"
          :key="index"
          class="faq-item"
          :class="{ open: openFaq === index }"
        >

          <button
            class="faq-question"
            @click="toggleFaq(index)"
          >

            <span>
              {{ faq.q }}
            </span>

            <strong>
              {{ openFaq === index ? '−' : '+' }}
            </strong>

          </button>

          <div
            v-if="openFaq === index"
            class="faq-answer"
          >
            {{ faq.a }}
          </div>

        </article>

      </div>

    </section>

    <!-- CONTACT -->

    <section
      id="contact"
      class="contact-section"
    >

      <div class="contact-box">

        <div class="contact-info">

          <div class="free-badge">
            <span class="live-dot"></span>
            {{ t.badge }}
          </div>

          <h2>
            {{ t.formTitle }}
          </h2>

          <p>
            {{ t.formText }}
          </p>

          <div class="contact-list">

            <span>
              ✓ {{ t.badge }}
            </span>

            <span>
              ✓ {{ t.secondary }}
            </span>

            <span>
              ✓ ללא התחייבות
            </span>

          </div>

        </div>

        <form
          class="contact-form"
          @submit.prevent="submitForm"
        >

          <template v-if="!formSent">

            <label>
              {{ t.name }}

              <input
                type="text"
                required
                autocomplete="name"
              />
            </label>

            <label>
              {{ t.phone }}

              <input
                type="tel"
                required
                autocomplete="tel"
              />
            </label>

            <button
              type="submit"
              class="form-submit"
            >
              {{ t.submit }}

              <span>
                ←
              </span>
            </button>

            <small>
              השארת פרטים אינה מהווה התחייבות.
            </small>

          </template>

          <div
            v-else
            class="success"
          >

            <div class="success-icon">
              ✓
            </div>

            <h3>
              {{ t.successTitle }}
            </h3>

            <p>
              {{ t.successText }}
            </p>

          </div>

        </form>

      </div>

    </section>

    <!-- FOOTER -->

    <footer class="footer">

      <div class="footer-inner">

        <div class="footer-brand">

          <span class="logo-mark">
            פ
          </span>

          <strong>
            פנסרה
          </strong>

        </div>

        <p>
          {{ t.footer }}
        </p>

        <span class="copyright">
          © {{ new Date().getFullYear() }}
        </span>

      </div>

    </footer>

  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Assistant:wght@300;400;500;600;700;800&display=swap');

:root {
  --cream: #faf8f2;
  --cream-2: #f4f1e9;
  --white: #ffffff;

  --blue: #12a9c9;
  --blue-dark: #087f9b;
  --blue-light: #58d6e9;

  --text: #17262c;
  --muted: #66777e;

  --border: rgba(23, 38, 44, .10);

  --shadow:
    0 25px 70px rgba(24, 59, 69, .09);
}

* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;

  background: var(--cream);

  color: var(--text);

  font-family:
    'Assistant',
    Arial,
    sans-serif;

  -webkit-font-smoothing: antialiased;
}

button,
input {
  font: inherit;
}

button {
  border: 0;
}

a {
  text-decoration: none;
  color: inherit;
}

/* NAV */

.navbar {
  position: absolute;

  top: 0;
  left: 0;

  width: 100%;

  z-index: 100;

  background:
    rgba(255, 253, 248, .78);

  backdrop-filter:
    blur(18px);

  border-bottom:
    1px solid
    rgba(23,38,44,.06);
}

.nav-inner {
  width: min(94vw, 1400px);

  min-height: 88px;

  margin: auto;

  display: flex;
  align-items: center;

  gap: 25px;
}

.logo {
  display: flex;
  align-items: center;

  gap: 10px;

  font-size: 27px;
  font-weight: 800;

  white-space: nowrap;
}

.logo-mark {
  width: 43px;
  height: 43px;

  display: grid;
  place-items: center;

  border-radius: 14px;

  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue)
    );

  box-shadow:
    0 10px 25px
    rgba(18,169,201,.20);
}

.nav-links {
  display: flex;
  align-items: center;

  gap: 30px;

  margin:
    0 auto;
}

.nav-links a {
  color: #586a71;

  font-size: 17px;
  font-weight: 700;

  transition: .25s;
}

.nav-links a:hover {
  color: var(--blue);
}

/* LANGUAGE */

.languages {
  display: flex;
  align-items: center;

  gap: 5px;

  padding: 5px;

  background: white;

  border:
    1px solid
    rgba(23,38,44,.09);

  border-radius: 17px;

  box-shadow:
    0 8px 25px
    rgba(25,55,65,.07);
}

.language-button {
  min-height: 44px;

  padding:
    6px 11px;

  display: flex;
  align-items: center;
  justify-content: center;

  gap: 6px;

  border-radius: 12px;

  background: transparent;

  color: #66777e;

  font-size: 15px;
  font-weight: 800;

  cursor: pointer;

  transition: .25s;
}

.language-button:hover,
.language-button.active {
  color: white;

  background:
    var(--blue);

  box-shadow:
    0 7px 20px
    rgba(18,169,201,.22);
}

.flag {
  font-size: 19px;
}

.arabic-symbol {
  width: 24px;
  height: 24px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: var(--blue);

  background:
    rgba(18,169,201,.10);
}

.language-button.active .arabic-symbol {
  color: white;

  background:
    rgba(255,255,255,.18);
}

.hamburger {
  display: none;

  width: 46px;
  height: 46px;

  border-radius: 13px;

  background: white;

  cursor: pointer;
}

.hamburger span {
  display: block;

  width: 22px;
  height: 2px;

  margin: 5px auto;

  background: var(--text);
}

/* HERO */

.hero {
  position: relative;

  min-height: 850px;

  overflow: hidden;

  background:
    radial-gradient(
      circle at 76% 25%,
      rgba(18,169,201,.13),
      transparent 32%
    ),

    radial-gradient(
      circle at 12% 85%,
      rgba(216,195,156,.15),
      transparent 35%
    ),

    linear-gradient(
      135deg,
      #fffefa,
      #faf8f2 60%,
      #f1eee5
    );
}

.hero-inner {
  position: relative;
  z-index: 2;

  width: min(92vw, 1300px);

  min-height: 750px;

  padding-top: 135px;

  margin: auto;

  display: grid;

  grid-template-columns:
    1fr
    .85fr;

  align-items: center;

  gap: 80px;
}

.hero-content {
  max-width: 700px;
}

.free-badge {
  display: inline-flex;
  align-items: center;

  gap: 10px;

  padding:
    10px 17px;

  border-radius: 999px;

  color:
    var(--blue-dark);

  background:
    rgba(18,169,201,.09);

  border:
    1px solid
    rgba(18,169,201,.17);

  font-size: 16px;
  font-weight: 800;
}

.live-dot {
  width: 9px;
  height: 9px;

  border-radius: 50%;

  background:
    var(--blue);

  box-shadow:
    0 0 0 6px
    rgba(18,169,201,.09);

  animation:
    pulse 1.8s infinite;
}

@keyframes pulse {
  50% {
    transform: scale(1.4);
    opacity: .65;
  }
}

.hero h1 {
  margin:
    25px 0 0;

  font-size:
    clamp(45px, 5.5vw, 76px);

  line-height: 1.02;

  letter-spacing: -2.7px;

  font-weight: 800;
}

.hero h1 span {
  color: var(--blue);
}

.hero-description {
  max-width: 640px;

  margin:
    28px 0 0;

  color:
    var(--muted);

  font-size: 21px;

  line-height: 1.65;
}

.hero-buttons {
  display: flex;

  flex-wrap: wrap;

  gap: 13px;

  margin-top: 34px;
}

.primary-button,
.secondary-button {
  min-height: 61px;

  padding:
    0 27px;

  display: inline-flex;
  align-items: center;
  justify-content: center;

  gap: 14px;

  border-radius: 16px;

  font-size: 18px;
  font-weight: 800;

  transition: .3s;
}

.primary-button {
  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue)
    );

  box-shadow:
    0 15px 35px
    rgba(18,169,201,.23);
}

.primary-button:hover {
  transform:
    translateY(-4px);

  box-shadow:
    0 20px 45px
    rgba(18,169,201,.32);
}

.primary-button span {
  font-size: 25px;
}

.secondary-button {
  background:
    rgba(255,255,255,.78);

  border:
    1px solid
    var(--border);

  color:
    var(--text);
}

.secondary-button:hover {
  transform:
    translateY(-3px);

  border-color:
    rgba(18,169,201,.35);
}

.hero-checks {
  display: flex;

  flex-wrap: wrap;

  gap: 22px;

  margin-top: 23px;

  color:
    #718188;

  font-size: 15px;
  font-weight: 700;
}

.hero-checks span {
  display: flex;

  align-items: center;

  gap: 6px;
}

/* HERO VISUAL */

.hero-visual {
  position: relative;

  min-height: 520px;

  display: grid;
  place-items: center;
}

.finance-card {
  position: relative;
  z-index: 2;

  width: 390px;
  height: 470px;

  padding: 32px;

  overflow: hidden;

  border-radius: 34px;

  background:
    linear-gradient(
      145deg,
      #ffffff,
      #e9f9fb
    );

  border:
    1px solid
    rgba(18,169,201,.14);

  box-shadow:
    0 35px 90px
    rgba(25,65,75,.13);

  transform:
    rotate(3deg);

  transition: .5s;
}

.finance-card:hover {
  transform:
    rotate(0deg)
    scale(1.025);
}

.finance-card-top {
  display: flex;

  justify-content: space-between;

  align-items: center;

  font-size: 24px;
}

.verified {
  width: 39px;
  height: 39px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background:
    var(--blue);
}

.finance-icon {
  width: 150px;
  height: 150px;

  margin:
    85px auto 50px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background:
    linear-gradient(
      145deg,
      var(--blue-light),
      var(--blue-dark)
    );

  font-size: 57px;
  font-weight: 800;

  box-shadow:
    0 25px 60px
    rgba(18,169,201,.25);
}

.finance-lines {
  display: grid;

  gap: 13px;
}

.finance-lines span {
  height: 10px;

  border-radius: 20px;

  background:
    rgba(18,169,201,.11);
}

.finance-lines span:nth-child(1) {
  width: 100%;
}

.finance-lines span:nth-child(2) {
  width: 70%;
}

.finance-lines span:nth-child(3) {
  width: 85%;
}

.finance-bottom {
  position: absolute;

  bottom: 28px;
  left: 32px;
  right: 32px;

  display: flex;
  justify-content: space-between;

  color:
    #74858c;
}

.finance-bottom strong {
  color:
    var(--blue);
}

.glow-ring {
  position: absolute;

  width: 440px;
  height: 440px;

  border-radius: 50%;

  border:
    1px solid
    rgba(18,169,201,.12);

  box-shadow:
    0 0 80px
    rgba(18,169,201,.10);
}

.floating-stat {
  position: absolute;
  z-index: 5;

  min-width: 185px;

  padding:
    15px 18px;

  display: flex;
  align-items: center;

  gap: 12px;

  border-radius: 18px;

  background:
    rgba(255,255,255,.95);

  border:
    1px solid
    rgba(18,169,201,.12);

  box-shadow:
    0 20px 45px
    rgba(30,60,70,.11);

  animation:
    float 4s ease-in-out infinite;
}

.floating-top {
  top: 55px;
  right: 0;
}

.floating-bottom {
  bottom: 55px;
  left: 0;

  animation-delay:
    -1.5s;
}

@keyframes float {
  50% {
    transform:
      translateY(-12px);
  }
}

.float-icon,
.float-check {
  width: 43px;
  height: 43px;

  display: grid;
  place-items: center;

  border-radius: 14px;

  color: white;

  background:
    var(--blue);

  font-weight: 800;
}

.floating-stat strong {
  display: block;
}

.floating-stat small {
  color:
    #84949a;
}

/* ORBS */

.hero-orb {
  position: absolute;

  border-radius: 50%;

  pointer-events: none;
}

.orb-1 {
  width: 500px;
  height: 500px;

  top: 25%;
  right: -300px;

  border:
    1px solid
    rgba(18,169,201,.09);
}

.orb-2 {
  width: 300px;
  height: 300px;

  bottom: -190px;
  left: -100px;

  background:
    rgba(215,194,153,.08);
}

/* STATS */

.stats-bar {
  position: absolute;

  bottom: 0;
  left: 0;

  width: 100%;

  z-index: 10;

  background:
    rgba(255,255,255,.88);

  border-top:
    1px solid
    rgba(23,38,44,.07);

  backdrop-filter:
    blur(15px);
}

.stats-inner {
  width: min(92vw, 950px);

  min-height: 105px;

  margin: auto;

  display: grid;

  grid-template-columns:
    1fr
    1px
    1fr
    1px
    1fr;

  align-items: center;
}

.stat {
  text-align: center;
}

.stat strong {
  display: block;

  color:
    var(--blue);

  font-size: 39px;

  line-height: 1;

  font-weight: 800;
}

.stat span {
  display: block;

  margin-top: 6px;

  color:
    #687980;

  font-size: 15px;
  font-weight: 700;
}

.stat-divider {
  width: 1px;
  height: 42px;

  background:
    rgba(23,38,44,.10);
}

/* TICKER */

.ticker-section {
  position: relative;

  overflow: hidden;

  padding:
    105px 0 100px;

  background:
    #fffdf8;
}

.ticker-glow {
  position: absolute;

  width: 500px;
  height: 200px;

  top: 45%;

  left: 50%;

  transform:
    translate(-50%, -50%);

  border-radius: 50%;

  background:
    rgba(18,169,201,.08);

  filter:
    blur(70px);

  pointer-events: none;
}

.section-heading {
  position: relative;
  z-index: 2;

  width: min(90vw, 850px);

  margin:
    0 auto 50px;

  text-align: center;
}

.section-heading.compact {
  margin-bottom: 42px;
}

.eyebrow {
  display: inline-block;

  margin-bottom: 10px;

  color:
    var(--blue);

  font-size: 14px;
  font-weight: 800;

  letter-spacing: 1.5px;
}

.section-heading h2 {
  margin: 0;

  font-size:
    clamp(35px, 4vw, 53px);

  line-height: 1.08;

  letter-spacing: -1.6px;

  font-weight: 800;
}

.section-heading p {
  max-width: 650px;

  margin:
    18px auto 0;

  color:
    var(--muted);

  font-size: 19px;

  line-height: 1.6;
}

.ticker-window {
  position: relative;
  z-index: 2;

  width: 100%;

  overflow: hidden;

  padding:
    15px 0 30px;
}

.ticker-track {
  width: max-content;

  display: flex;

  gap: 18px;

  transition:
    transform 1s
    cubic-bezier(.22,.8,.2,1);

  will-change: transform;
}

.ticker-card {
  position: relative;

  width: 270px;
  min-height: 115px;

  padding:
    25px;

  flex: 0 0 270px;

  display: flex;
  align-items: center;

  gap: 15px;

  border-radius: 22px;

  background:
    rgba(255,255,255,.94);

  border:
    1px solid
    rgba(18,169,201,.11);

  box-shadow:
    0 15px 40px
    rgba(25,60,70,.07);

  transition: .35s;

  overflow: hidden;
}

.ticker-card::before {
  content: '';

  position: absolute;

  width: 100px;
  height: 100px;

  top: -55px;
  right: -40px;

  border-radius: 50%;

  background:
    rgba(18,169,201,.08);

  filter:
    blur(2px);
}

.ticker-card::after {
  content: '';

  position: absolute;

  top: 0;
  left: -100%;

  width: 70%;
  height: 100%;

  background:
    linear-gradient(
      90deg,
      transparent,
      rgba(255,255,255,.75),
      transparent
    );

  transform:
    skewX(-20deg);

  animation:
    shine 4.5s infinite;
}

@keyframes shine {
  0% {
    left: -100%;
  }

  45%,
  100% {
    left: 150%;
  }
}

.ticker-card:hover {
  transform:
    translateY(-7px)
    scale(1.04);

  border-color:
    rgba(18,169,201,.38);

  box-shadow:
    0 25px 55px
    rgba(18,169,201,.14);
}

.ticker-icon {
  position: relative;
  z-index: 2;

  width: 48px;
  height: 48px;

  flex: 0 0 auto;

  display: grid;
  place-items: center;

  border-radius: 15px;

  color: white;

  background:
    linear-gradient(
      145deg,
      var(--blue-light),
      var(--blue)
    );

  box-shadow:
    0 8px 22px
    rgba(18,169,201,.22);
}

.ticker-card strong {
  position: relative;
  z-index: 2;

  font-size: 18px;
  font-weight: 800;
}

.ticker-arrow {
  position: relative;
  z-index: 2;

  margin-right: auto;

  color:
    var(--blue);

  font-size: 22px;
}

/* SERVICES */

.services-section {
  padding:
    110px 0;

  background:
    var(--cream);
}

.services-grid {
  width: min(92vw, 1200px);

  margin: auto;

  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  gap: 20px;
}

.service-card {
  position: relative;

  min-height: 325px;

  padding: 32px;

  overflow: hidden;

  border-radius: 24px;

  background:
    rgba(255,255,255,.88);

  border:
    1px solid
    var(--border);

  box-shadow:
    0 12px 40px
    rgba(30,50,60,.05);

  transition: .4s;
}

.service-card:hover {
  transform:
    translateY(-8px);

  border-color:
    rgba(18,169,201,.35);

  box-shadow:
    0 25px 55px
    rgba(18,169,201,.11);
}

.service-number {
  position: absolute;

  top: 15px;
  left: 23px;

  color:
    rgba(18,169,201,.10);

  font-size: 57px;
  font-weight: 800;
}

.service-icon {
  width: 64px;
  height: 64px;

  margin-bottom: 34px;

  display: grid;
  place-items: center;

  border-radius: 18px;

  color: white;

  background:
    linear-gradient(
      145deg,
      var(--blue-light),
      var(--blue)
    );

  font-size: 27px;
  font-weight: 800;

  box-shadow:
    0 12px 25px
    rgba(18,169,201,.19);
}

.service-card h3 {
  margin: 0;

  font-size: 24px;
  font-weight: 800;
}

.service-card p {
  margin-top: 11px;

  color:
    var(--muted);

  font-size: 17px;

  line-height: 1.55;
}

.service-bottom {
  position: absolute;

  bottom: 24px;
  left: 32px;
  right: 32px;

  display: flex;

  justify-content: space-between;

  color:
    var(--blue);

  font-size: 25px;
}

.service-bottom span {
  width: 45px;
  height: 3px;

  margin-top: 12px;

  border-radius: 20px;

  background:
    var(--blue);
}

/* HOW */

.how-section {
  padding:
    110px 0;

  background:
    #fffdf8;
}

.steps {
  width: min(90vw, 1150px);

  margin: auto;

  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  gap: 20px;
}

.step {
  position: relative;

  text-align: center;
}

.step-circle {
  width: 88px;
  height: 88px;

  margin:
    0 auto 22px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color:
    var(--blue);

  background:
    rgba(18,169,201,.08);

  border:
    2px solid
    rgba(18,169,201,.15);

  font-size: 22px;
  font-weight: 800;

  transition: .35s;
}

.step:hover .step-circle {
  color: white;

  background:
    var(--blue);

  transform:
    scale(1.08);
}

.step h3 {
  margin: 0;

  font-size: 22px;
}

.step p {
  margin-top: 7px;

  color:
    var(--muted);

  font-size: 16px;
}

.step-connector {
  position: absolute;

  top: 44px;

  left: calc(50% + 65px);

  width: calc(100% - 130px);

  height: 2px;

  background:
    linear-gradient(
      90deg,
      rgba(18,169,201,.24),
      transparent
    );
}

/* BIG CTA */

.big-cta {
  position: relative;

  width: min(92vw, 1250px);

  margin:
    20px auto 110px;

  padding:
    45px 55px;

  overflow: hidden;

  border-radius: 30px;

  color: white;

  background:
    linear-gradient(
      135deg,
      #15a7c5,
      #087f9b
    );

  box-shadow:
    0 30px 70px
    rgba(18,169,201,.21);
}

.cta-glow {
  position: absolute;

  width: 500px;
  height: 500px;

  top: -300px;
  right: -100px;

  border-radius: 50%;

  background:
    rgba(255,255,255,.10);

  filter:
    blur(5px);
}

.cta-inner {
  position: relative;
  z-index: 2;

  display: flex;
  align-items: center;

  gap: 25px;
}

.cta-check {
  width: 76px;
  height: 76px;

  flex: 0 0 auto;

  display: grid;
  place-items: center;

  border-radius: 22px;

  background:
    rgba(255,255,255,.14);

  border:
    1px solid
    rgba(255,255,255,.22);

  font-size: 34px;
  font-weight: 800;
}

.cta-copy span {
  opacity: .85;

  font-size: 14px;
  font-weight: 700;
}

.cta-copy h2 {
  margin:
    3px 0 0;

  font-size:
    clamp(28px, 4vw, 43px);
}

.cta-copy p {
  margin:
    7px 0 0;

  opacity: .83;

  font-size: 17px;
}

.white-button {
  margin-right: auto;

  min-height: 58px;

  padding:
    0 24px;

  display: inline-flex;
  align-items: center;

  gap: 13px;

  white-space: nowrap;

  border-radius: 15px;

  color:
    var(--blue-dark);

  background:
    white;

  font-weight: 800;

  transition: .3s;
}

.white-button:hover {
  transform:
    translateY(-4px);

  box-shadow:
    0 15px 30px
    rgba(0,0,0,.14);
}

/* FAQ */

.faq-section {
  padding:
    110px 0;

  background:
    var(--cream);
}

.faq-list {
  width: min(90vw, 900px);

  margin: auto;
}

.faq-item {
  margin-bottom: 12px;

  overflow: hidden;

  border-radius: 17px;

  background:
    rgba(255,255,255,.82);

  border:
    1px solid
    var(--border);

  transition: .3s;
}

.faq-item.open {
  border-color:
    rgba(18,169,201,.35);

  box-shadow:
    0 10px 30px
    rgba(25,60,70,.06);
}

.faq-question {
  width: 100%;

  padding:
    22px 25px;

  display: flex;
  justify-content: space-between;
  align-items: center;

  gap: 20px;

  background: transparent;

  color:
    var(--text);

  text-align: start;

  font-size: 20px;
  font-weight: 800;

  cursor: pointer;
}

.faq-question strong {
  width: 34px;
  height: 34px;

  flex: 0 0 auto;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background:
    var(--blue);

  font-size: 22px;
}

.faq-answer {
  padding:
    0 25px 25px;

  color:
    var(--muted);

  font-size: 18px;

  line-height: 1.65;
}

/* CONTACT */

.contact-section {
  padding:
    110px 0;

  background:
    #fffdf8;
}

.contact-box {
  width: min(92vw, 1150px);

  margin: auto;

  padding:
    60px;

  display: grid;

  grid-template-columns:
    1fr
    .75fr;

  gap: 65px;

  border-radius: 32px;

  background:
    linear-gradient(
      145deg,
      #eefafb,
      #ffffff
    );

  border:
    1px solid
    rgba(18,169,201,.10);

  box-shadow:
    var(--shadow);
}

.contact-info h2 {
  margin:
    25px 0 0;

  font-size:
    clamp(38px, 4vw, 56px);

  line-height: 1.05;
}

.contact-info > p {
  max-width: 540px;

  color:
    var(--muted);

  font-size: 19px;

  line-height: 1.6;
}

.contact-list {
  display: grid;

  gap: 10px;

  margin-top: 28px;

  color:
    #607279;

  font-weight: 700;
}

.contact-form {
  min-height: 330px;

  padding: 32px;

  display: flex;
  align-items: center;

  border-radius: 24px;

  background: white;

  box-shadow:
    0 20px 50px
    rgba(30,60,70,.08);
}

.contact-form > * {
  width: 100%;
}

.contact-form label {
  display: grid;

  gap: 7px;

  margin-bottom: 17px;

  color:
    #53656c;

  font-size: 15px;
  font-weight: 700;
}

.contact-form input {
  width: 100%;

  height: 56px;

  padding:
    0 16px;

  border:
    1px solid
    #d8e1e3;

  border-radius: 13px;

  outline: none;

  background:
    #fbfcfc;

  font-size: 17px;

  transition: .25s;
}

.contact-form input:focus {
  border-color:
    var(--blue);

  box-shadow:
    0 0 0 4px
    rgba(18,169,201,.09);
}

.form-submit {
  width: 100%;

  min-height: 58px;

  display: flex;
  justify-content: center;
  align-items: center;

  gap: 14px;

  border-radius: 14px;

  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue)
    );

  font-size: 18px;
  font-weight: 800;

  cursor: pointer;

  transition: .3s;
}

.form-submit:hover {
  transform:
    translateY(-3px);
}

.contact-form small {
  display: block;

  margin-top: 13px;

  color:
    #87969b;

  text-align: center;
}

.success {
  text-align: center;
}

.success-icon {
  width: 80px;
  height: 80px;

  margin:
    0 auto 20px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background:
    var(--blue);

  font-size: 35px;
  font-weight: 800;
}

.success h3 {
  margin: 0;

  font-size: 26px;
}

.success p {
  color:
    var(--muted);

  font-size: 17px;
}

/* FOOTER */

.footer {
  padding:
    45px 0;

  background:
    #17262c;

  color: white;
}

.footer-inner {
  width: min(92vw, 1200px);

  margin: auto;

  display: flex;
  align-items: center;

  gap: 28px;
}

.footer-brand {
  display: flex;
  align-items: center;

  gap: 10px;

  font-size: 23px;

  white-space: nowrap;
}

.footer-brand .logo-mark {
  width: 38px;
  height: 38px;

  border-radius: 11px;
}

.footer-inner p {
  max-width: 650px;

  margin: 0;

  color:
    rgba(255,255,255,.65);

  font-size: 13px;

  line-height: 1.6;
}

.copyright {
  margin-right: auto;

  color:
    rgba(255,255,255,.50);

  white-space: nowrap;

  font-size: 13px;
}

/* MOBILE */

@media (max-width: 1050px) {

  .nav-links {
    gap: 16px;
  }

  .services-grid {
    grid-template-columns:
      repeat(2, 1fr);
  }

  .steps {
    grid-template-columns:
      repeat(2, 1fr);

    row-gap: 55px;
  }

  .step-connector {
    display: none;
  }

  .hero-inner {
    gap: 40px;
  }
}

@media (max-width: 850px) {

  .nav-inner {
    min-height: 78px;
  }

  .nav-links {
    position: absolute;

    top: 78px;

    left: 3vw;
    right: 3vw;

    display: none;

    flex-direction: column;

    align-items: stretch;

    gap: 0;

    padding: 20px;

    border-radius: 20px;

    background: white;

    box-shadow:
      0 20px 50px
      rgba(25,50,60,.14);
  }

  .nav-links.open {
    display: flex;
  }

  .nav-links a {
    padding:
      15px;

    border-bottom:
      1px solid
      rgba(23,38,44,.07);
  }

  .hamburger {
    display: block;
  }

  .language-name {
    display: none;
  }

  .language-button {
    width: 43px;

    padding:
      6px;
  }

  .hero {
    min-height: auto;
  }

  .hero-inner {
    min-height: auto;

    padding:
      125px 0 140px;

    grid-template-columns: 1fr;

    text-align: center;
  }

  .hero-content {
    margin: auto;
  }

  .hero-buttons {
    justify-content: center;
  }

  .hero-checks {
    justify-content: center;
  }

  .hero-visual {
    min-height: 430px;
  }

  .stats-inner {
    min-height: 90px;
  }

  .stat strong {
    font-size: 31px;
  }

  .stat span {
    font-size: 13px;
  }

  .contact-box {
    grid-template-columns: 1fr;

    padding:
      40px 25px;
  }

  .cta-inner {
    flex-direction: column;

    text-align: center;
  }

  .white-button {
    margin: 0;
  }

  .footer-inner {
    flex-direction: column;

    text-align: center;
  }

  .copyright {
    margin: 0;
  }
}

@media (max-width: 600px) {

  .logo {
    font-size: 23px;
  }

  .logo-mark {
    width: 39px;
    height: 39px;
  }

  .hero h1 {
    font-size:
      clamp(39px, 11vw, 56px);

    letter-spacing: -1.7px;
  }

  .hero-description {
    font-size: 18px;
  }

  .hero-buttons {
    width: 100%;
  }

  .primary-button,
  .secondary-button {
    width: 100%;
  }

  .finance-card {
    width: 310px;
    height: 390px;
  }

  .finance-icon {
    width: 120px;
    height: 120px;

    margin:
      60px auto 40px;
  }

  .floating-stat {
    min-width: 150px;

    padding: 11px;
  }

  .floating-top {
    top: 20px;
    right: 0;
  }

  .floating-bottom {
    bottom: 20px;
    left: 0;
  }

  .stats-inner {
    width: 94vw;
  }

  .stat strong {
    font-size: 27px;
  }

  .stat span {
    font-size: 11px;
  }

  .stat-divider {
    height: 32px;
  }

  .ticker-card {
    width: 235px;
    flex-basis: 235px;

    min-height: 100px;
  }

  .services-grid {
    width: 90vw;

    grid-template-columns: 1fr;
  }

  .steps {
    width: 90vw;

    grid-template-columns: 1fr;
  }

  .big-cta {
    width: 90vw;

    padding:
      35px 25px;

    margin-bottom: 70px;
  }

  .faq-list {
    width: 90vw;
  }

  .contact-box {
    width: 90vw;
  }

  .contact-form {
    padding: 22px;
  }
}
</style>
