<script setup>
import { ref, computed } from 'vue'

const language = ref('he')
const mobileMenu = ref(false)
const openFaq = ref(null)
const currentSlide = ref(0)
const formSent = ref(false)

const languages = [
  { id: 'he', label: 'עברית', icon: '🇮🇱', dir: 'rtl' },
  { id: 'ru', label: 'Русский', icon: '🇷🇺', dir: 'ltr' },
  { id: 'ar', label: 'العربية', icon: 'ع', dir: 'rtl' }
]

const translations = {
  he: {
    navHome: 'ראשי',
    navServices: 'השירותים שלנו',
    navHow: 'איך זה עובד',
    navFaq: 'שאלות נפוצות',
    navContact: 'צור קשר',

    badge: 'בדיקה ללא עלות וללא התחייבות',

    heroTitle: 'יש לכם כספים בקופות?',
    heroTitle2: 'בואו נבדוק מה אפשר לעשות איתם.',
    heroText:
      'אנחנו עוזרים לכם לאתר את החסכונות והקופות שלכם, להבין מה נמצא שם ולקבל הכוונה לגבי האפשרויות העומדות בפניכם.',
    heroButton: 'בדיקה ללא עלות',
    heroSub: 'פשוט. ברור. ללא התחייבות.',

    freeTitle: 'מתחילים בבדיקה ללא עלות',
    freeText:
      'הבדיקה הראשונית וההדרכה על הכספים שאיתרנו עבורכם הן ללא עלות וללא התחייבות.',

    sliderTitle: 'איפה יכולים להיות הכספים שלכם?',
    sliderText:
      'החסכונות שלכם יכולים להיות מפוזרים בין גופים שונים. אנחנו עוזרים לעשות סדר.',

    servicesTitle: 'מה אנחנו יכולים לעזור לכם לבדוק?',
    servicesText:
      'שירות פשוט וברור שמתחיל בבדיקה ראשונית ללא עלות.',

    service1Title: 'איתור כספים',
    service1Text:
      'בדיקה ואיתור של חסכונות, קופות וקרנות שעשויים להיות רשומים על שמכם.',

    service2Title: 'בדיקת אפשרויות משיכה',
    service2Text:
      'הבנת האפשרויות הקיימות סביב הכספים והאם קיימת אפשרות למשיכה בהתאם לנסיבות שלכם.',

    service3Title: 'הלוואה כנגד קופה',
    service3Text:
      'בדיקה האם קיימת אפשרות לקבלת הלוואה כנגד כספים שנמצאים בקופות מסוימות.',

    service4Title: 'סדר והבנה',
    service4Text:
      'עוזרים לכם להבין מה יש לכם, איפה הכספים נמצאים ומה כדאי לבדוק הלאה.',

    howTitle: 'איך זה עובד?',
    step1: 'משאירים פרטים',
    step1Text: 'פחות מדקה.',
    step2: 'נציג חוזר אליכם',
    step2Text: ' תוך 24 שעות',
    step3: 'בודקים את הזכאות',
    step3Text: 'מקבלים הסבר ברור לפני שמחליטים.',
    step4: 'מחליטים אם להמשיך',
    step4Text: 'אין התחייבות להמשיך בתהליך.',

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
      'בחלק מהמוצרים והקופות עשויה להיות אפשרות לקבלת הלוואה כנגד הכספים. הזכאות והתנאים נקבעים על ידי הגוף המנהל ובהתאם לנתוני הלקוח.',

    faq5Q: 'האם יש התחייבות?',
    faq5A:
      'לא. הבדיקה הראשונית היא ללא עלות וללא התחייבות.',

    formTitle: 'רוצים לבדוק מה יש לכם?',
    formText:
      'השאירו פרטים ונציג יחזור אליכם. הבדיקה הראשונית ללא עלות וללא התחייבות.',
    name: 'שם מלא',
    phone: 'טלפון',
    submit: 'בדיקה ללא עלות',
    sending: 'שולח...',
    sent: 'הפרטים התקבלו בהצלחה',
    sentText: 'נציג יחזור אליכם בהקדם.',

    privacy:
      'השארת פרטים אינה מהווה התחייבות לביצוע פעולה כלשהי.',

    footerText:
      'פנסרה מספקת שירותי מידע, איתור והכוונה. שירותים הדורשים רישיון יינתנו באמצעות בעל רישיון מתאים, ככל שנדרש על פי דין.',
    rights: 'כל הזכויות שמורות.'
  },

  ru: {
    navHome: 'Главная',
    navServices: 'Наши услуги',
    navHow: 'Как это работает',
    navFaq: 'Частые вопросы',
    navContact: 'Связаться',

    badge: 'Проверка бесплатно и без обязательств',

    heroTitle: 'У вас есть деньги в пенсионных кассах?',
    heroTitle2: 'Давайте проверим, что с ними можно сделать.',
    heroText:
      'Мы помогаем найти ваши накопления и кассы, понять что там находится и узнать, какие возможности могут быть доступны.',
    heroButton: 'Бесплатная проверка',
    heroSub: 'Просто. Понятно. Без обязательств.',

    freeTitle: 'Начинаем с бесплатной проверки',
    freeText:
      'Первичная проверка и объяснение найденных средств полностью бесплатны и ни к чему вас не обязывают.',

    sliderTitle: 'Где могут находиться ваши деньги?',
    sliderText:
      'Ваши накопления могут находиться в разных компаниях. Мы помогаем навести порядок.',

    servicesTitle: 'Что мы можем проверить?',
    servicesText:
      'Простой сервис, который начинается с бесплатной первичной проверки.',

    service1Title: 'Поиск денег',
    service1Text:
      'Проверяем, какие накопления, кассы и фонды могут быть записаны на ваше имя.',

    service2Title: 'Проверка возможности снять деньги',
    service2Text:
      'Объясняем, какие варианты могут быть доступны в вашей ситуации.',

    service3Title: 'Кредит под накопления',
    service3Text:
      'Проверяем, возможно ли получить кредит под деньги, которые находятся в определённых кассах.',

    service4Title: 'Порядок и понимание',
    service4Text:
      'Помогаем понять, что у вас есть, где находятся деньги и что можно проверить дальше.',

    howTitle: 'Как это работает?',
    step1: 'Заполняете форму',
    step1Text: 'Меньше минуты.',
    step2: 'Мы связываемся с вами',
    step2Text: 'Обычно в течение 24 часов.',
    step3: 'Проверяем варианты',
    step3Text: 'Получаете понятное объяснение.',
    step4: 'Решаете сами',
    step4Text: 'Никаких обязательств.',

    faqTitle: 'Частые вопросы',

    faq1Q: 'Сколько это занимает?',
    faq1A:
      'Заполнение формы — меньше минуты. Представитель свяжется с вами в течение 24 часов (вс–чт, 9:00–18:00). Получение денег обычно занимает 7–14 рабочих дней, в зависимости от условий и управляющей организации.',

    faq2Q: 'Как это может быть бесплатно?',
    faq2A:
      'Первичная проверка полностью бесплатна. Проверка и объяснение найденных средств — бесплатно. Если для дальнейшего действия потребуется лицензированный специалист, вам объяснят варианты и стоимость, и вы сами решите, продолжать или нет.',

    faq3Q: 'Я обязан снимать деньги?',
    faq3A:
      'Нет. Проверка не обязывает вас снимать деньги. Вы сначала узнаёте, что у вас есть и какие варианты существуют.',

    faq4Q: 'Можно ли получить кредит под кассу?',
    faq4A:
      'В некоторых продуктах может существовать возможность кредита под накопления. Условия зависят от конкретной кассы и данных клиента.',

    faq5Q: 'Есть ли обязательство?',
    faq5A:
      'Нет. Первичная проверка проводится бесплатно и без обязательств.',

    formTitle: 'Хотите проверить свои накопления?',
    formText:
      'Оставьте данные, и представитель свяжется с вами. Первичная проверка бесплатна.',
    name: 'Имя и фамилия',
    phone: 'Телефон',
    submit: 'Бесплатная проверка',
    sending: 'Отправка...',
    sent: 'Данные получены',
    sentText: 'Мы свяжемся с вами как можно скорее.',

    privacy:
      'Оставление данных не означает обязательство выполнить какую-либо операцию.',

    footerText:
      'Пансера предоставляет информационные услуги, поиск и навигацию. Услуги, требующие лицензии, предоставляются соответствующим лицензированным специалистом, когда это требуется законом.',
    rights: 'Все права защищены.'
  },

  ar: {
    navHome: 'الرئيسية',
    navServices: 'الخدمات',
    navHow: 'كيف يعمل؟',
    navFaq: 'الأسئلة الشائعة',
    navContact: 'تواصل معنا',

    badge: 'فحص مجاني وبدون التزام',

    heroTitle: 'لديكم أموال في صناديق التوفير؟',
    heroTitle2: 'دعونا نفحص ما يمكن فعله بها.',
    heroText:
      'نساعدكم في العثور على المدخرات والصناديق المسجلة باسمكم، وفهم ما الموجود فيها وما هي الخيارات الممكنة.',
    heroButton: 'فحص مجاني',
    heroSub: 'بسيط. واضح. بدون التزام.',

    freeTitle: 'نبدأ بفحص مجاني',
    freeText:
      'الفحص الأولي وشرح الأموال التي تم العثور عليها مجانيان تماماً وبدون أي التزام.',

    sliderTitle: 'أين يمكن أن تكون أموالكم؟',
    sliderText:
      'قد تكون المدخرات موزعة بين جهات مختلفة. نحن نساعدكم على ترتيب الصورة.',

    servicesTitle: 'ما الذي يمكننا مساعدتكم في فحصه؟',
    servicesText:
      'خدمة بسيطة وواضحة تبدأ بفحص أولي مجاني.',

    service1Title: 'العثور على الأموال',
    service1Text:
      'فحص المدخرات والصناديق التي قد تكون مسجلة باسمكم.',

    service2Title: 'فحص إمكانية السحب',
    service2Text:
      'شرح الخيارات التي قد تكون متاحة حسب وضعكم وشروط الصندوق.',

    service3Title: 'قرض مقابل الصندوق',
    service3Text:
      'فحص إمكانية الحصول على قرض مقابل أموال موجودة في بعض الصناديق.',

    service4Title: 'ترتيب وفهم',
    service4Text:
      'نساعدكم على فهم ما لديكم وأين توجد الأموال وما الذي يمكن فحصه بعد ذلك.',

    howTitle: 'كيف يعمل الأمر؟',
    step1: 'تعبئة النموذج',
    step1Text: 'أقل من دقيقة.',
    step2: 'نتواصل معكم',
    step2Text: 'عادة خلال 24 ساعة.',
    step3: 'نفحص الخيارات',
    step3Text: 'تحصلون على شرح واضح.',
    step4: 'أنتم تقررون',
    step4Text: 'بدون أي التزام.',

    faqTitle: 'الأسئلة الشائعة',

    faq1Q: 'كم يستغرق الأمر؟',
    faq1A:
      'تعبئة النموذج — أقل من دقيقة. سيتواصل معكم ممثل خلال 24 ساعة (الأحد–الخميس، 9:00–18:00). سحب الأموال يستغرق عادةً 7–14 يوم عمل، حسب الشروط والجهة المديرة.',

    faq2Q: 'كيف يمكن أن يكون الفحص مجانياً؟',
    faq2A:
      'الفحص الأولي مجاني تماماً. الفحص وشرح الأموال التي تم العثور عليها مجانيان. إذا احتاجت العملية إلى مختص مرخّص، سنوضح لكم الخيارات والتكلفة، وأنتم تقررون إذا كنتم تريدون المتابعة.',

    faq3Q: 'هل يجب عليّ سحب الأموال؟',
    faq3A:
      'لا. الفحص لا يلزمكم بسحب الأموال. الهدف هو فهم ما لديكم وما هي الخيارات المتاحة.',

    faq4Q: 'هل يمكن الحصول على قرض مقابل الصندوق؟',
    faq4A:
      'في بعض المنتجات قد تكون هناك إمكانية للحصول على قرض مقابل المدخرات. الشروط تعتمد على الصندوق وبيانات العميل.',

    faq5Q: 'هل يوجد أي التزام؟',
    faq5A:
      'لا. الفحص الأولي مجاني وبدون أي التزام.',

    formTitle: 'هل تريدون فحص مدخراتكم؟',
    formText:
      'اتركوا بياناتكم وسيتواصل معكم ممثل. الفحص الأولي مجاني وبدون التزام.',
    name: 'الاسم الكامل',
    phone: 'الهاتف',
    submit: 'فحص مجاني',
    sending: 'جارٍ الإرسال...',
    sent: 'تم استلام البيانات',
    sentText: 'سنتواصل معكم في أقرب وقت.',

    privacy:
      'إرسال البيانات لا يعني الالتزام بتنفيذ أي عملية.',

    footerText:
      'بانسرا تقدم خدمات معلومات وبحث وتوجيه. الخدمات التي تتطلب ترخيصاً يتم تقديمها بواسطة مختص مرخص عند الحاجة وفقاً للقانون.',
    rights: 'جميع الحقوق محفوظة.'
  }
}

const t = computed(() => translations[language.value])
const currentDir = computed(() => {
  return languages.find(x => x.id === language.value)?.dir || 'rtl'
})

const companies = [
  {
    name: 'חברות ביטוח',
    icon: '◈',
    text: 'קופות, קרנות ומוצרי חיסכון'
  },
  {
    name: 'בתי השקעות',
    icon: '◆',
    text: 'חסכונות ומוצרים פיננסיים'
  },
  {
    name: 'קרנות השתלמות',
    icon: '◇',
    text: 'בדיקה ואיתור כספים'
  },
  {
    name: 'קופות גמל',
    icon: '◉',
    text: 'קופות וחסכונות'
  },
  {
    name: 'קרנות פנסיה',
    icon: '✦',
    text: 'מוצרי חיסכון פנסיוני'
  }
]

const services = computed(() => [
  {
    icon: '⌕',
    title: t.value.service1Title,
    text: t.value.service1Text
  },
  {
    icon: '₪',
    title: t.value.service2Title,
    text: t.value.service2Text
  },
  {
    icon: '↗',
    title: t.value.service3Title,
    text: t.value.service3Text
  },
  {
    icon: '✓',
    title: t.value.service4Title,
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

function setLanguage(id) {
  language.value = id
  mobileMenu.value = false
}

function toggleFaq(index) {
  openFaq.value = openFaq.value === index ? null : index
}

function nextSlide() {
  currentSlide.value =
    (currentSlide.value + 1) % companies.length
}

function previousSlide() {
  currentSlide.value =
    (currentSlide.value - 1 + companies.length) % companies.length
}

function slideClass(index) {
  const diff =
    (index - currentSlide.value + companies.length) %
    companies.length

  if (diff === 0) return 'active'
  if (diff === 1) return 'next'
  if (diff === companies.length - 1) return 'previous'

  return 'hidden-slide'
}

function submitForm() {
  formSent.value = true

  setTimeout(() => {
    formSent.value = false
  }, 5000)
}
</script>

<template>
  <div
    class="site"
    :dir="currentDir"
    :lang="language"
  >

    <!-- NAVBAR -->
    <header class="navbar">
      <div class="nav-inner">

        <a href="#" class="logo">
          <span class="logo-mark">פ</span>
          <span class="logo-name">פנסרה</span>
        </a>

        <nav
          class="desktop-nav"
          :class="{ open: mobileMenu }"
        >
          <a href="#home">{{ t.navHome }}</a>
          <a href="#services">{{ t.navServices }}</a>
          <a href="#how">{{ t.navHow }}</a>
          <a href="#faq">{{ t.navFaq }}</a>
          <a href="#contact">{{ t.navContact }}</a>
        </nav>

        <!-- LANGUAGE SWITCHER -->
        <div class="language-switcher">

          <button
            v-for="item in languages"
            :key="item.id"
            class="language-button"
            :class="{ selected: language === item.id }"
            @click="setLanguage(item.id)"
            :aria-label="item.label"
          >
            <span
              v-if="item.id !== 'ar'"
              class="language-icon"
            >
              {{ item.icon }}
            </span>

            <span
              v-else
              class="arabic-icon"
            >
              ع
            </span>

            <span class="language-label">
              {{ item.label }}
            </span>
          </button>

        </div>

        <button
          class="mobile-menu-button"
          @click="mobileMenu = !mobileMenu"
          aria-label="Menu"
        >
          <span></span>
          <span></span>
          <span></span>
        </button>

      </div>
    </header>

    <!-- HERO -->
    <main id="home">

      <section class="hero">

        <div class="hero-background-circle circle-one"></div>
        <div class="hero-background-circle circle-two"></div>

        <div class="hero-inner">

          <div class="hero-content">

            <div class="free-badge">
              <span class="pulse-dot"></span>
              {{ t.badge }}
            </div>

            <h1>
              {{ t.heroTitle }}
              <br />
              <span>{{ t.heroTitle2 }}</span>
            </h1>

            <p class="hero-text">
              {{ t.heroText }}
            </p>

            <div class="hero-actions">

              <a
                href="#contact"
                class="primary-button"
              >
                {{ t.heroButton }}
                <span class="button-arrow">←</span>
              </a>

              <a
                href="#how"
                class="secondary-button"
              >
                {{ t.heroSub }}
              </a>

            </div>

            <div class="hero-trust">

              <div>
                <span class="trust-icon">✓</span>
                {{ t.badge }}
              </div>

              <div>
                <span class="trust-icon">✓</span>
                ללא התחייבות
              </div>

            </div>

          </div>

          <!-- HERO VISUAL -->
          <div class="hero-visual">

            <div class="floating-card card-top">
              <span class="mini-icon">₪</span>
              <div>
                <strong>+</strong>
                <small>חסכונות</small>
              </div>
            </div>

            <div class="main-visual-card">

              <div class="visual-glow"></div>

              <div class="visual-header">
                <span>פנסרה</span>
                <span class="secure">✓</span>
              </div>

              <div class="visual-number">
                ₪
              </div>

              <div class="visual-lines">
                <span></span>
                <span></span>
                <span></span>
              </div>

              <div class="visual-bottom">
                <span>קופות וחסכונות</span>
                <strong>בדיקה</strong>
              </div>

            </div>

            <div class="floating-card card-bottom">
              <span class="check-circle">✓</span>
              <div>
                <strong>בדיקה</strong>
                <small>ללא עלות</small>
              </div>
            </div>

          </div>

        </div>

        <!-- FREE STRIP -->
        <div class="free-strip">
          <div class="free-strip-inner">
            <span class="strip-icon">✓</span>
            <strong>{{ t.freeTitle }}</strong>
            <span>{{ t.freeText }}</span>
          </div>
        </div>

      </section>

      <!-- COMPANIES SLIDER -->
      <section class="companies-section">

        <div class="section-heading">
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

        <div class="company-slider">

          <button
            class="slider-arrow slider-arrow-left"
            @click="previousSlide"
            aria-label="Previous"
          >
            {{ currentDir === 'rtl' ? '→' : '←' }}
          </button>

          <div class="company-track">

            <article
              v-for="(company, index) in companies"
              :key="company.name"
              class="company-card"
              :class="slideClass(index)"
              @mouseenter="currentSlide = index"
            >

              <div class="company-icon">
                {{ company.icon }}
              </div>

              <h3>
                {{ company.name }}
              </h3>

              <p>
                {{ company.text }}
              </p>

              <div class="company-line"></div>

              <span class="company-arrow">
                ←
              </span>

            </article>

          </div>

          <button
            class="slider-arrow slider-arrow-right"
            @click="nextSlide"
            aria-label="Next"
          >
            {{ currentDir === 'rtl' ? '←' : '→' }}
          </button>

        </div>

        <div class="slider-dots">

          <button
            v-for="(_, index) in companies"
            :key="index"
            :class="{ active: currentSlide === index }"
            @click="currentSlide = index"
          ></button>

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
            v-for="(service, index) in services"
            :key="index"
            class="service-card"
          >

            <div class="service-number">
              0{{ index + 1 }}
            </div>

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
              <b>→</b>
            </div>

          </article>

        </div>

      </section>

      <!-- HOW IT WORKS -->
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

            <div class="step-number">
              {{ step.number }}
            </div>

            <div class="step-content">

              <h3>
                {{ step.title }}
              </h3>

              <p>
                {{ step.text }}
              </p>

            </div>

            <div
              v-if="index < steps.length - 1"
              class="step-line"
            ></div>

          </article>

        </div>

      </section>

      <!-- FREE CTA -->
      <section class="free-cta">

        <div class="free-cta-glow"></div>

        <div class="free-cta-content">

          <div class="big-free-icon">
            ✓
          </div>

          <div>
            <span class="cta-small">
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
            {{ t.heroButton }}
            <span>←</span>
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
            :class="{ opened: openFaq === index }"
          >

            <button
              class="faq-question"
              @click="toggleFaq(index)"
            >

              <span>
                {{ faq.q }}
              </span>

              <span class="faq-plus">
                {{ openFaq === index ? '−' : '+' }}
              </span>

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

      <!-- CONTACT FORM -->
      <section
        id="contact"
        class="contact-section"
      >

        <div class="contact-container">

          <div class="contact-copy">

            <div class="free-badge dark-badge">
              <span class="pulse-dot"></span>
              {{ t.badge }}
            </div>

            <h2>
              {{ t.formTitle }}
            </h2>

            <p>
              {{ t.formText }}
            </p>

            <div class="contact-benefits">

              <div>
                <span>✓</span>
                {{ t.badge }}
              </div>

              <div>
                <span>✓</span>
                {{ t.privacy }}
              </div>

            </div>

          </div>

          <form
            class="contact-form"
            @submit.prevent="submitForm"
          >

            <div
              v-if="!formSent"
              class="form-fields"
            >

              <label>
                {{ t.name }}
                <input
                  type="text"
                  required
                  autocomplete="name"
                  placeholder=""
                />
              </label>

              <label>
                {{ t.phone }}
                <input
                  type="tel"
                  required
                  autocomplete="tel"
                  placeholder=""
                />
              </label>

              <button
                type="submit"
                class="form-button"
              >
                {{ t.submit }}
                <span>←</span>
              </button>

              <small>
                {{ t.privacy }}
              </small>

            </div>

            <div
              v-else
              class="success-message"
            >

              <div class="success-icon">
                ✓
              </div>

              <h3>
                {{ t.sent }}
              </h3>

              <p>
                {{ t.sentText }}
              </p>

            </div>

          </form>

        </div>

      </section>

    </main>

    <!-- FOOTER -->
    <footer class="footer">

      <div class="footer-inner">

        <div class="footer-logo">
          <span class="logo-mark">פ</span>
          <strong>פנסרה</strong>
        </div>

        <p>
          {{ t.footerText }}
        </p>

        <span class="footer-rights">
          © {{ new Date().getFullYear() }} {{ t.rights }}
        </span>

      </div>

    </footer>

  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Assistant:wght@300;400;500;600;700;800&family=Rubik:wght@300;400;500;600;700;800&display=swap');

:root {
  --cream: #faf8f2;
  --cream-dark: #f1eee5;
  --white: #ffffff;

  --blue: #16a7c7;
  --blue-dark: #087d99;
  --blue-light: #55cde4;

  --text: #18252b;
  --muted: #65757d;

  --border: rgba(24, 37, 43, .10);

  --shadow:
    0 20px 60px rgba(35, 65, 75, .09);

  --radius: 24px;
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
    'Rubik',
    Arial,
    sans-serif;

  -webkit-font-smoothing: antialiased;
}

button,
input {
  font-family: inherit;
}

button {
  border: 0;
}

a {
  color: inherit;
  text-decoration: none;
}

/* =========================
   NAVBAR
========================= */

.navbar {
  position: absolute;
  z-index: 100;
  top: 0;
  left: 0;

  width: 100%;

  background:
    rgba(255, 253, 248, .72);

  backdrop-filter:
    blur(18px);

  border-bottom:
    1px solid
    rgba(24, 37, 43, .06);
}

.nav-inner {
  width: min(94vw, 1450px);
  min-height: 92px;

  margin: auto;

  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 28px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;

  font-size: 28px;
  font-weight: 800;
}

.logo-mark {
  width: 44px;
  height: 44px;

  display: grid;
  place-items: center;

  color: white;

  border-radius: 14px;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue)
    );

  box-shadow:
    0 10px 25px
    rgba(22, 167, 199, .20);
}

.logo-name {
  letter-spacing: -1px;
}

.desktop-nav {
  display: flex;
  align-items: center;
  gap: 34px;

  margin-right: auto;
  margin-left: auto;
}

.desktop-nav a {
  color: #51636a;
  font-size: 17px;
  font-weight: 600;

  transition: .25s;
}

.desktop-nav a:hover {
  color: var(--blue);
}

/* =========================
   LANGUAGES
========================= */

.language-switcher {
  display: flex;
  align-items: center;
  gap: 7px;

  padding: 6px;

  background:
    rgba(255,255,255,.90);

  border:
    1px solid
    rgba(24,37,43,.10);

  border-radius: 18px;

  box-shadow:
    0 8px 25px
    rgba(25,50,60,.06);
}

.language-button {
  min-height: 45px;

  padding: 7px 12px;

  display: flex;
  align-items: center;
  justify-content: center;
  gap: 7px;

  border-radius: 13px;

  background: transparent;

  color: #65757d;

  font-size: 15px;
  font-weight: 700;

  cursor: pointer;

  transition: .25s;
}

.language-button:hover,
.language-button.selected {
  background:
    var(--blue);

  color: white;

  transform:
    translateY(-1px);

  box-shadow:
    0 6px 18px
    rgba(22,167,199,.25);
}

.language-icon {
  font-size: 20px;
}

.arabic-icon {
  width: 25px;
  height: 25px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  font-size: 16px;
  font-weight: 800;

  background:
    rgba(22,167,199,.10);

  color:
    var(--blue);
}

.language-button.selected .arabic-icon {
  background: rgba(255,255,255,.20);
  color: white;
}

.mobile-menu-button {
  display: none;

  width: 48px;
  height: 48px;

  border-radius: 14px;

  background: white;

  cursor: pointer;

  box-shadow:
    0 8px 25px
    rgba(25,50,60,.08);
}

.mobile-menu-button span {
  display: block;

  width: 22px;
  height: 2px;

  margin: 5px auto;

  background: var(--text);
}

/* =========================
   HERO
========================= */

.hero {
  position: relative;

  min-height: 820px;

  overflow: hidden;

  background:
    radial-gradient(
      circle at 75% 25%,
      rgba(22,167,199,.13),
      transparent 34%
    ),

    radial-gradient(
      circle at 10% 80%,
      rgba(210,190,150,.16),
      transparent 35%
    ),

    linear-gradient(
      135deg,
      #fffdf8 0%,
      #faf8f2 55%,
      #f1eee6 100%
    );
}

.hero-inner {
  position: relative;
  z-index: 2;

  width: min(92vw, 1350px);

  min-height: 730px;

  margin: auto;

  padding-top: 155px;

  display: grid;

  grid-template-columns:
    1.05fr
    .95fr;

  align-items: center;

  gap: 80px;
}

.hero-content {
  max-width: 720px;
}

.free-badge {
  display: inline-flex;
  align-items: center;
  gap: 10px;

  padding: 10px 17px;

  border-radius: 999px;

  color:
    var(--blue-dark);

  background:
    rgba(22,167,199,.10);

  border:
    1px solid
    rgba(22,167,199,.18);

  font-size: 16px;
  font-weight: 800;

  margin-bottom: 25px;
}

.pulse-dot {
  width: 9px;
  height: 9px;

  border-radius: 50%;

  background: var(--blue);

  box-shadow:
    0 0 0 6px
    rgba(22,167,199,.10);

  animation:
    pulse 1.8s infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }

  50% {
    transform: scale(1.35);
    opacity: .7;
  }
}

.hero h1 {
  margin: 0;

  font-size:
    clamp(46px, 5.3vw, 76px);

  line-height: 1.02;

  letter-spacing: -2.8px;

  font-weight: 800;

  color: var(--text);
}

.hero h1 span {
  color: var(--blue);
}

.hero-text {
  max-width: 650px;

  margin:
    30px 0 0;

  color:
    var(--muted);

  font-size: 21px;

  line-height: 1.65;

  font-weight: 400;
}

.hero-actions {
  display: flex;
  align-items: center;
  gap: 14px;

  margin-top: 34px;

  flex-wrap: wrap;
}

.primary-button,
.secondary-button {
  min-height: 61px;

  padding:
    0 28px;

  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 16px;

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
    rgba(22,167,199,.23);
}

.primary-button:hover {
  transform:
    translateY(-4px);

  box-shadow:
    0 20px 45px
    rgba(22,167,199,.32);
}

.button-arrow {
  font-size: 24px;
}

.secondary-button {
  color:
    var(--text);

  background:
    rgba(255,255,255,.78);

  border:
    1px solid
    var(--border);
}

.secondary-button:hover {
  transform:
    translateY(-3px);

  border-color:
    rgba(22,167,199,.35);
}

.hero-trust {
  display: flex;
  flex-wrap: wrap;

  gap: 24px;

  margin-top: 25px;

  color:
    #74838a;

  font-size: 15px;
  font-weight: 600;
}

.hero-trust div {
  display: flex;
  align-items: center;
  gap: 7px;
}

.trust-icon {
  width: 22px;
  height: 22px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);

  font-size: 13px;
}

/* HERO VISUAL */

.hero-visual {
  position: relative;

  min-height: 500px;

  display: grid;
  place-items: center;
}

.main-visual-card {
  position: relative;

  width: min(420px, 80vw);
  height: 500px;

  padding: 32px;

  overflow: hidden;

  border-radius: 36px;

  background:
    linear-gradient(
      145deg,
      #ffffff,
      #edf9fb
    );

  border:
    1px solid
    rgba(22,167,199,.16);

  box-shadow:
    0 35px 90px
    rgba(30,70,80,.13);

  transform:
    rotate(3deg);

  transition: .5s;
}

.main-visual-card:hover {
  transform:
    rotate(0deg)
    scale(1.025);
}

.visual-glow {
  position: absolute;

  width: 300px;
  height: 300px;

  top: -100px;
  right: -80px;

  border-radius: 50%;

  background:
    rgba(22,167,199,.18);

  filter:
    blur(30px);
}

.visual-header {
  position: relative;
  z-index: 2;

  display: flex;
  justify-content: space-between;

  font-size: 24px;
  font-weight: 800;
}

.secure {
  width: 38px;
  height: 38px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);
}

.visual-number {
  position: relative;
  z-index: 2;

  width: 160px;
  height: 160px;

  margin:
    85px auto 50px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  font-size: 65px;
  font-weight: 800;

  background:
    linear-gradient(
      145deg,
      var(--blue-light),
      var(--blue-dark)
    );

  box-shadow:
    0 25px 55px
    rgba(22,167,199,.25);
}

.visual-lines {
  position: relative;
  z-index: 2;

  display: grid;

  gap: 14px;
}

.visual-lines span {
  display: block;

  height: 11px;

  border-radius: 20px;

  background:
    rgba(22,167,199,.12);
}

.visual-lines span:nth-child(1) {
  width: 100%;
}

.visual-lines span:nth-child(2) {
  width: 70%;
}

.visual-lines span:nth-child(3) {
  width: 85%;
}

.visual-bottom {
  position: absolute;

  bottom: 28px;
  left: 32px;
  right: 32px;

  display: flex;
  justify-content: space-between;

  color: #72848b;

  font-size: 14px;
  font-weight: 600;
}

.visual-bottom strong {
  color: var(--blue);
}

.floating-card {
  position: absolute;
  z-index: 5;

  min-width: 190px;

  padding: 17px 20px;

  display: flex;
  align-items: center;
  gap: 13px;

  background:
    rgba(255,255,255,.94);

  border:
    1px solid
    rgba(22,167,199,.14);

  border-radius: 18px;

  box-shadow:
    0 20px 45px
    rgba(30,60,70,.12);

  animation:
    floatCard 4s ease-in-out infinite;
}

.card-top {
  top: 70px;
  right: 0;
}

.card-bottom {
  bottom: 55px;
  left: 0;

  animation-delay:
    -1.5s;
}

@keyframes floatCard {
  0%, 100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-12px);
  }
}

.mini-icon,
.check-circle {
  width: 43px;
  height: 43px;

  display: grid;
  place-items: center;

  border-radius: 14px;

  color: white;

  background: var(--blue);

  font-size: 20px;
  font-weight: 800;
}

.floating-card strong {
  display: block;

  font-size: 18px;
}

.floating-card small {
  display: block;

  color: #829097;

  margin-top: 2px;
}

/* BACKGROUND */

.hero-background-circle {
  position: absolute;

  border-radius: 50%;

  filter: blur(2px);

  pointer-events: none;
}

.circle-one {
  width: 420px;
  height: 420px;

  top: 30%;
  right: -200px;

  border:
    1px solid
    rgba(22,167,199,.10);
}

.circle-two {
  width: 280px;
  height: 280px;

  bottom: -150px;
  left: -100px;

  background:
    rgba(213,191,147,.08);
}

/* FREE STRIP */

.free-strip {
  position: absolute;
  z-index: 5;

  bottom: 0;
  left: 0;

  width: 100%;

  background:
    rgba(255,255,255,.84);

  border-top:
    1px solid
    rgba(24,37,43,.07);

  backdrop-filter:
    blur(15px);
}

.free-strip-inner {
  width: min(92vw, 1200px);

  min-height: 80px;

  margin: auto;

  display: flex;
  align-items: center;
  justify-content: center;

  gap: 17px;

  text-align: center;

  color: #68787f;

  font-size: 17px;
}

.free-strip-inner strong {
  color: var(--text);
}

.strip-icon {
  width: 32px;
  height: 32px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);

  font-weight: 800;
}

/* =========================
   GENERAL SECTIONS
========================= */

section:not(.hero) {
  padding:
    115px 0;
}

.section-heading {
  width: min(90vw, 850px);

  margin:
    0 auto 55px;

  text-align: center;
}

.eyebrow {
  display: inline-block;

  margin-bottom: 12px;

  color: var(--blue);

  font-size: 15px;
  font-weight: 800;

  letter-spacing: 1.5px;

  text-transform: uppercase;
}

.section-heading h2 {
  margin: 0;

  color: var(--text);

  font-size:
    clamp(35px, 4vw, 54px);

  line-height: 1.08;

  letter-spacing: -1.8px;

  font-weight: 800;
}

.section-heading p {
  max-width: 650px;

  margin:
    20px auto 0;

  color: var(--muted);

  font-size: 19px;

  line-height: 1.6;
}

/* =========================
   COMPANIES SLIDER
========================= */

.companies-section {
  background:
    #fffdf8;
}

.company-slider {
  position: relative;

  width: min(94vw, 1450px);

  margin: auto;

  padding:
    0 45px;

  display: flex;
  align-items: center;
}

.company-track {
  width: 100%;

  display: grid;

  grid-template-columns:
    repeat(3, minmax(0, 1fr));

  gap: 32px;

  align-items: center;
}

.company-card {
  min-height: 315px;

  padding: 34px;

  position: relative;

  overflow: hidden;

  background:
    rgba(255,255,255,.92);

  border:
    1px solid
    rgba(24,37,43,.09);

  border-radius: 24px;

  box-shadow:
    0 15px 45px
    rgba(35,55,65,.07);

  transition:
    transform .5s cubic-bezier(.2,.8,.2,1),
    opacity .5s,
    box-shadow .5s,
    border-color .5s;

  cursor: pointer;
}

.company-card::before {
  content: '';

  position: absolute;

  width: 180px;
  height: 180px;

  top: -90px;
  right: -90px;

  border-radius: 50%;

  background:
    rgba(22,167,199,.07);

  transition: .5s;
}

.company-card:hover,
.company-card.active {
  transform:
    scale(1.04)
    translateY(-6px);

  border-color:
    rgba(22,167,199,.45);

  box-shadow:
    0 25px 65px
    rgba(22,167,199,.13);
}

.company-card:hover::before,
.company-card.active::before {
  transform:
    scale(1.5);

  background:
    rgba(22,167,199,.12);
}

.company-card.next,
.company-card.previous {
  opacity: .78;
}

.company-card.hidden-slide {
  display: none;
}

.company-icon {
  position: relative;

  width: 67px;
  height: 67px;

  display: grid;
  place-items: center;

  margin-bottom: 28px;

  border-radius: 20px;

  color: var(--blue);

  background:
    rgba(22,167,199,.09);

  font-size: 28px;

  transition: .4s;
}

.company-card:hover .company-icon,
.company-card.active .company-icon {
  color: white;

  background: var(--blue);

  transform:
    rotate(-5deg)
    scale(1.08);
}

.company-card h3 {
  position: relative;

  margin: 0;

  font-size: 27px;

  font-weight: 800;
}

.company-card p {
  position: relative;

  margin:
    10px 0 0;

  color: var(--muted);

  font-size: 17px;
}

.company-line {
  width: 48px;
  height: 3px;

  margin-top: 30px;

  border-radius: 20px;

  background: var(--blue);

  transition: .4s;
}

.company-card:hover .company-line,
.company-card.active .company-line {
  width: 90px;
}

.company-arrow {
  position: absolute;

  bottom: 30px;
  right: 30px;

  color: var(--blue);

  font-size: 25px;

  opacity: .55;

  transition: .4s;
}

.company-card:hover .company-arrow,
.company-card.active .company-arrow {
  opacity: 1;

  transform:
    translateX(-7px);
}

/* SLIDER ARROWS */

.slider-arrow {
  position: absolute;

  z-index: 10;

  width: 64px;
  height: 64px;

  display: grid;
  place-items: center;

  border:
    2px solid
    rgba(22,167,199,.42);

  border-radius: 50%;

  background:
    rgba(255,255,255,.96);

  color: var(--blue);

  font-size: 27px;
  font-weight: 800;

  cursor: pointer;

  box-shadow:
    0 10px 30px
    rgba(22,167,199,.14),

    0 0 25px
    rgba(22,167,199,.08);

  transition: .3s;
}

.slider-arrow:hover {
  transform:
    scale(1.18);

  color: white;

  background:
    var(--blue);

  border-color:
    var(--blue);

  box-shadow:
    0 0 25px
      rgba(22,167,199,.48),

    0 0 60px
      rgba(22,167,199,.20);
}

.slider-arrow-left {
  left: 7px;
}

.slider-arrow-right {
  right: 7px;
}

.slider-dots {
  margin-top: 35px;

  display: flex;
  justify-content: center;

  gap: 8px;
}

.slider-dots button {
  width: 9px;
  height: 9px;

  padding: 0;

  border-radius: 50%;

  background:
    #cbd7da;

  cursor: pointer;

  transition: .3s;
}

.slider-dots button.active {
  width: 30px;

  border-radius: 10px;

  background:
    var(--blue);
}

/* =========================
   SERVICES
========================= */

.services-section {
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

  min-height: 330px;

  padding: 32px;

  overflow: hidden;

  background:
    rgba(255,255,255,.88);

  border:
    1px solid
    var(--border);

  border-radius: 24px;

  box-shadow:
    0 12px 40px
    rgba(30,50,60,.05);

  transition: .4s;
}

.service-card:hover {
  transform:
    translateY(-8px);

  border-color:
    rgba(22,167,199,.35);

  box-shadow:
    0 22px 55px
    rgba(22,167,199,.11);
}

.service-number {
  position: absolute;

  top: 20px;
  left: 25px;

  color:
    rgba(22,167,199,.12);

  font-size: 54px;
  font-weight: 800;
}

.service-icon {
  width: 65px;
  height: 65px;

  display: grid;
  place-items: center;

  margin-bottom: 35px;

  border-radius: 19px;

  color: white;

  background:
    linear-gradient(
      145deg,
      var(--blue-light),
      var(--blue)
    );

  font-size: 28px;
  font-weight: 800;

  box-shadow:
    0 12px 25px
    rgba(22,167,199,.18);
}

.service-card h3 {
  margin: 0;

  font-size: 24px;
  font-weight: 800;
}

.service-card p {
  margin:
    12px 0 0;

  color: var(--muted);

  font-size: 17px;

  line-height: 1.55;
}

.service-bottom {
  position: absolute;

  bottom: 25px;
  left: 32px;
  right: 32px;

  display: flex;
  align-items: center;
  justify-content: space-between;
}

.service-bottom span {
  width: 45px;
  height: 3px;

  border-radius: 10px;

  background: var(--blue);
}

.service-bottom b {
  color: var(--blue);

  font-size: 24px;

  transition: .3s;
}

.service-card:hover .service-bottom b {
  transform:
    translateX(-7px);
}

/* =========================
   HOW
========================= */

.how-section {
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

.step-number {
  width: 90px;
  height: 90px;

  margin: 0 auto 25px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: var(--blue);

  background:
    rgba(22,167,199,.08);

  border:
    2px solid
    rgba(22,167,199,.16);

  font-size: 23px;
  font-weight: 800;

  transition: .4s;
}

.step:hover .step-number {
  color: white;

  background: var(--blue);

  transform:
    scale(1.08);
}

.step h3 {
  margin: 0;

  font-size: 22px;
  font-weight: 800;
}

.step p {
  margin-top: 8px;

  color: var(--muted);

  font-size: 16px;
}

.step-line {
  position: absolute;

  top: 45px;

  left: calc(50% + 65px);

  width: calc(100% - 130px);

  height: 2px;

  background:
    linear-gradient(
      90deg,
      rgba(22,167,199,.25),
      transparent
    );
}

/* =========================
   CTA
========================= */

.free-cta {
  position: relative;

  width: min(92vw, 1250px);

  min-height: 250px;

  margin:
    40px auto 100px;

  padding:
    45px 55px;

  overflow: hidden;

  border-radius: 30px;

  color: white;

  background:
    linear-gradient(
      135deg,
      #159fbd,
      #087d99
    );

  box-shadow:
    0 30px 70px
    rgba(22,167,199,.22);
}

.free-cta-glow {
  position: absolute;

  width: 400px;
  height: 400px;

  top: -230px;
  right: -100px;

  border-radius: 50%;

  background:
    rgba(255,255,255,.10);

  filter:
    blur(5px);
}

.free-cta-content {
  position: relative;
  z-index: 2;

  display: flex;

  align-items: center;

  gap: 30px;
}

.big-free-icon {
  flex: 0 0 auto;

  width: 80px;
  height: 80px;

  display: grid;
  place-items: center;

  border-radius: 24px;

  color: white;

  background:
    rgba(255,255,255,.14);

  border:
    1px solid
    rgba(255,255,255,.25);

  font-size: 36px;
  font-weight: 800;
}

.cta-small {
  display: block;

  margin-bottom: 5px;

  opacity: .85;

  font-size: 15px;
  font-weight: 700;
}

.free-cta h2 {
  margin: 0;

  font-size:
    clamp(28px, 4vw, 44px);

  font-weight: 800;
}

.free-cta p {
  max-width: 650px;

  margin:
    8px 0 0;

  opacity: .85;

  font-size: 17px;
}

.white-button {
  margin-right: auto;

  min-height: 58px;

  padding:
    0 25px;

  display: inline-flex;
  align-items: center;
  gap: 15px;

  white-space: nowrap;

  border-radius: 15px;

  color: var(--blue-dark);

  background: white;

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

/* =========================
   FAQ
========================= */

.faq-section {
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

  background:
    rgba(255,255,255,.80);

  border:
    1px solid
    var(--border);

  border-radius: 17px;

  transition: .3s;
}

.faq-item:hover,
.faq-item.opened {
  border-color:
    rgba(22,167,199,.30);

  box-shadow:
    0 10px 30px
    rgba(30,60,70,.05);
}

.faq-question {
  width: 100%;

  padding:
    23px 26px;

  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 25px;

  color: var(--text);

  background: transparent;

  text-align: start;

  font-size: 20px;
  font-weight: 800;

  cursor: pointer;
}

.faq-plus {
  flex: 0 0 auto;

  width: 34px;
  height: 34px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background:
    var(--blue);

  font-size: 23px;

  line-height: 1;
}

.faq-answer {
  padding:
    0 26px 25px;

  color: var(--muted);

  font-size: 18px;

  line-height: 1.65;
}

/* =========================
   CONTACT
========================= */

.contact-section {
  background:
    #fffdf8;
}

.contact-container {
  width: min(92vw, 1150px);

  margin: auto;

  padding:
    65px;

  display: grid;

  grid-template-columns:
    1fr
    .8fr;

  gap: 70px;

  border-radius: 32px;

  background:
    linear-gradient(
      145deg,
      #f1fafb,
      #ffffff
    );

  border:
    1px solid
    rgba(22,167,199,.10);

  box-shadow:
    var(--shadow);
}

.dark-badge {
  margin-bottom: 22px;
}

.contact-copy h2 {
  margin: 0;

  font-size:
    clamp(35px, 4vw, 55px);

  line-height: 1.05;

  letter-spacing: -1.5px;
}

.contact-copy > p {
  max-width: 550px;

  color: var(--muted);

  font-size: 19px;

  line-height: 1.6;
}

.contact-benefits {
  display: grid;

  gap: 12px;

  margin-top: 30px;

  color: #5e7077;

  font-weight: 600;
}

.contact-benefits div {
  display: flex;
  align-items: center;
  gap: 10px;
}

.contact-benefits span {
  width: 27px;
  height: 27px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);

  font-size: 14px;
}

.contact-form {
  min-height: 340px;

  padding: 35px;

  display: flex;
  align-items: center;

  border-radius: 25px;

  background: white;

  box-shadow:
    0 20px 50px
    rgba(30,60,70,.08);
}

.form-fields {
  width: 100%;

  display: grid;

  gap: 18px;
}

.form-fields label {
  display: grid;

  gap: 7px;

  color: #50636a;

  font-size: 15px;
  font-weight: 700;
}

.form-fields input {
  width: 100%;

  height: 56px;

  padding:
    0 17px;

  border:
    1px solid
    #d9e1e3;

  border-radius: 13px;

  outline: none;

  color: var(--text);

  background:
    #fbfcfc;

  font-size: 17px;

  transition: .25s;
}

.form-fields input:focus {
  border-color:
    var(--blue);

  box-shadow:
    0 0 0 4px
    rgba(22,167,199,.09);
}

.form-button {
  min-height: 58px;

  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;

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

  box-shadow:
    0 12px 25px
    rgba(22,167,199,.20);

  transition: .3s;
}

.form-button:hover {
  transform:
    translateY(-3px);
}

.form-fields small {
  color: #88979c;

  text-align: center;

  font-size: 13px;

  line-height: 1.5;
}

.success-message {
  width: 100%;

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

.success-message h3 {
  margin: 0;

  font-size: 26px;
}

.success-message p {
  color: var(--muted);

  font-size: 17px;
}

/* =========================
   FOOTER
========================= */

.footer {
  padding:
    45px 0;

  background:
    #17252b;

  color: white;
}

.footer-inner {
  width: min(92vw, 1200px);

  margin: auto;

  display: flex;
  align-items: center;

  gap: 30px;
}

.footer-logo {
  display: flex;
  align-items: center;

  gap: 10px;

  font-size: 23px;

  white-space: nowrap;
}

.footer-logo .logo-mark {
  width: 38px;
  height: 38px;

  border-radius: 11px;

  font-size: 19px;
}

.footer-inner p {
  max-width: 650px;

  margin: 0;

  color:
    rgba(255,255,255,.66);

  font-size: 14px;

  line-height: 1.6;
}

.footer-rights {
  margin-right: auto;

  white-space: nowrap;

  color:
    rgba(255,255,255,.55);

  font-size: 13px;
}

/* =========================
   MOBILE
========================= */

@media (max-width: 1100px) {

  .desktop-nav {
    gap: 18px;
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

  .step-line {
    display: none;
  }

  .hero-inner {
    gap: 40px;
  }
}

@media (max-width: 850px) {

  .nav-inner {
    min-height: 80px;
  }

  .desktop-nav {
    position: absolute;

    top: 82px;
    left: 3vw;
    right: 3vw;

    padding: 25px;

    display: none;

    flex-direction: column;

    align-items: stretch;

    gap: 0;

    background:
      rgba(255,255,255,.98);

    border-radius: 20px;

    box-shadow:
      0 20px 50px
      rgba(30,50,60,.14);
  }

  .desktop-nav.open {
    display: flex;
  }

  .desktop-nav a {
    padding: 16px;

    border-bottom:
      1px solid
      rgba(24,37,43,.07);
  }

  .mobile-menu-button {
    display: block;
  }

  .language-switcher {
    margin-right: auto;
  }

  .language-button {
    min-width: 43px;

    padding:
      7px 9px;
  }

  .language-label {
    display: none;
  }

  .hero {
    min-height: auto;
  }

  .hero-inner {
    min-height: auto;

    padding-top: 130px;
    padding-bottom: 120px;

    grid-template-columns: 1fr;

    text-align: center;
  }

  .hero-content {
    margin: auto;
  }

  .hero-actions {
    justify-content: center;
  }

  .hero-trust {
    justify-content: center;
  }

  .hero-visual {
    min-height: 430px;
  }

  .main-visual-card {
    height: 420px;
  }

  .visual-number {
    margin:
      55px auto 40px;
  }

  .card-top {
    right: 2%;
  }

  .card-bottom {
    left: 2%;
  }

  .free-strip-inner {
    flex-wrap: wrap;

    padding:
      14px 0;

    font-size: 15px;
  }

  .company-slider {
    width: 100%;

    padding:
      0 30px;
  }

  .company-track {
    grid-template-columns: 1fr;
  }

  .company-card {
    min-height: 290px;
  }

  .company-card.next,
  .company-card.previous {
    display: none;
  }

  .company-card.active {
    display: block;

    transform:
      scale(1.01);
  }

  .slider-arrow-left {
    left: -3px;
  }

  .slider-arrow-right {
    right: -3px;
  }

  .contact-container {
    grid-template-columns: 1fr;

    padding: 40px 25px;

    gap: 35px;
  }

  .free-cta {
    padding: 35px 28px;
  }

  .free-cta-content {
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

  .footer-rights {
    margin: 0;
  }
}

@media (max-width: 600px) {

  .nav-inner {
    width: 94vw;
  }

  .logo {
    font-size: 23px;
  }

  .logo-mark {
    width: 39px;
    height: 39px;
  }

  .language-switcher {
    gap: 3px;
  }

  .language-button {
    min-width: 39px;
    min-height: 39px;

    padding: 5px;
  }

  .language-icon {
    font-size: 18px;
  }

  .arabic-icon {
    width: 23px;
    height: 23px;
  }

  .mobile-menu-button {
    width: 43px;
    height: 43px;
  }

  .hero h1 {
    font-size:
      clamp(39px, 11vw, 57px);

    letter-spacing: -1.8px;
  }

  .hero-text {
    font-size: 18px;
  }

  .primary-button,
  .secondary-button {
    width: 100%;
  }

  .hero-actions {
    width: 100%;
  }

  .hero-visual {
    min-height: 390px;
  }

  .main-visual-card {
    width: 310px;
    height: 380px;

    padding: 25px;
  }

  .visual-number {
    width: 120px;
    height: 120px;

    margin:
      60px auto 40px;

    font-size: 48px;
  }

  .floating-card {
    min-width: 155px;

    padding: 12px;
  }

  .card-top {
    top: 25px;
    right: 0;
  }

  .card-bottom {
    bottom: 20px;
    left: 0;
  }

  section:not(.hero) {
    padding:
      80px 0;
  }

  .section-heading {
    margin-bottom: 40px;
  }

  .section-heading h2 {
    font-size: 37px;
  }

  .section-heading p {
    font-size: 17px;
  }

  .services-grid {
    width: 90vw;

    grid-template-columns: 1fr;
  }

  .steps {
    width: 90vw;

    grid-template-columns: 1fr;

    gap: 35px;
  }

  .free-cta {
    width: 90vw;

    margin-bottom: 60px;
  }

  .faq-list {
    width: 90vw;
  }

  .faq-question {
    padding:
      20px;

    font-size: 18px;
  }

  .faq-answer {
    padding:
      0 20px 22px;

    font-size: 17px;
  }

  .contact-container {
    width: 90vw;
  }

  .contact-form {
    padding: 22px;
  }

  .contact-copy h2 {
    font-size: 38px;
  }
}

/* =========================
   ACCESSIBILITY
========================= */

:focus-visible {
  outline:
    3px solid
    rgba(22,167,199,.55);

  outline-offset:
    3px;
}

@media (prefers-reduced-motion: reduce) {

  *,
  *::before,
  *::after {
    animation-duration:
      .01ms !important;

    animation-iteration-count:
      1 !important;

    scroll-behavior:
      auto !important;

    transition-duration:
      .01ms !important;
  }
}
</style>
