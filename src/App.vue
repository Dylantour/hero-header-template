<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

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
      'הכספים שלכם יכולים להיות מפוזרים בין גופים שונים. אנחנו עוזרים לעשות סדר.',

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
    step2Text: 'בדרך כלל בתוך 24 שעות.',
    step3: 'בודקים את האפשרויות',
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

    heroTitle: 'Давайте проверим, есть ли у вас средства',
    heroTitle2: 'и что с ними можно сделать.',
    heroText:
      'Мы помогаем найти ваши накопления и кассы, понять что там находится и узнать, какие возможности могут быть доступны.',
    heroButton: 'Бесплатная проверка',
    heroSub: 'Просто. Понятно. Без обязательств.',

    freeTitle: 'Начинаем с бесплатной проверки',
    freeText:
      'Первичная проверка и объяснение найденных средств полностью бесплатны и ни к чему вас не обязывают.',

    sliderTitle: 'Где могут находиться ваши средства?',
    sliderText:
      'Ваши средства могут находиться в разных компаниях. Мы помогаем навести порядок.',

    servicesTitle: 'Что мы можем проверить?',
    servicesText:
      'Простой сервис, который начинается с бесплатной первичной проверки.',

    service1Title: 'Поиск средств',
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
    step1: 'Оставляете данные',
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

    formTitle: 'Хотите проверить, что вам положено?',
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
    navHow: 'كيف بشتغل؟',
    navFaq: 'أسئلة شائعة',
    navContact: 'تواصل معنا',

    badge: 'فحص ببلاش وبدون التزام',

    heroTitle: 'خلّينا نفحص إذا إلك أتعاب',
    heroTitle2: 'وممكن تكون مستحقة إلك.',
    heroText:
      'فحص بسيط وسريع، بنساعدك تعرف إذا في أتعاب مستحقة إلك وممكن تكون مسجّلة على اسمك.',
    heroButton: 'فحص ببلاش',
    heroSub: 'بسيط. سريع. بدون التزام.',

    freeTitle: 'بنبدأ بفحص ببلاش',
    freeText:
      'الفحص الأولي والشرح عن الأتعاب اللي لقيناها إلك — ببلاش وبدون أي التزام.',

    sliderTitle: 'خلّينا نفحص شو إلك',
    sliderText:
      'بنفحص إذا في أتعاب مستحقة إلك وممكن تكون مسجّلة على اسمك.',

    servicesTitle: 'شو ممكن نفحصلك؟',
    servicesText:
      'خدمة بسيطة وواضحة، بتبدأ بفحص أولي ببلاش.',

    service1Title: 'العثور على الأتعاب',
    service1Text:
      'بنفحص إذا في أتعاب مستحقة إلك وممكن تكون مسجّلة على اسمك.',

    service2Title: 'فحص إمكانية السحب',
    service2Text:
      'بنفحص إذا بتقدر تسحب الأتعاب وشو الخيارات المتاحة إلك.',

    service3Title: 'قرض مقابل الصندوق',
    service3Text:
      'بنفحص إذا في إمكانية تاخد قرض مقابل الأتعاب الموجودة بالصندوق.',

    service4Title: 'نفهمك شو إلك',
    service4Text:
      'بنرتّبلك الصورة وبنشرحلك شو موجود وشو ممكن تعمل.',

    howTitle: 'كيف بشتغل؟',
    step1: 'بتترك تفاصيلك',
    step1Text: 'أقل من دقيقة.',
    step2: 'ممثل برجعلك',
    step2Text: 'عادةً خلال 24 ساعة.',
    step3: 'بنفحص الخيارات',
    step3Text: 'وبنشرحلك كل شي بشكل واضح.',
    step4: 'إنت بتقرر',
    step4Text: 'بدون أي التزام.',

    faqTitle: 'أسئلة شائعة',

    faq1Q: 'قديش بياخد وقت؟',
    faq1A:
      'تعبئة التفاصيل بتاخد أقل من دقيقة. ممثل برجعلك خلال 24 ساعة، من الأحد للخميس، بين الساعة 9:00 و18:00. سحب الأتعاب عادةً بياخد 7–14 يوم عمل، حسب الشروط والجهة المديرة.',

    faq2Q: 'كيف الفحص ببلاش؟',
    faq2A:
      'الفحص الأولي ببلاش بالكامل. الفحص والشرح عن الأتعاب اللي لقيناها إلك — ببلاش. إذا احتجت مختص مرخّص لإكمال العملية، بنوضحلك الخيارات والتكلفة وإنت بتقرر إذا بدك تكمل.',

    faq3Q: 'هل لازم أسحب الأتعاب؟',
    faq3A:
      'لا. الفحص ما بلزمك تسحب الأتعاب. الهدف إنك تعرف شو إلك وشو الخيارات المتاحة قدامك.',

    faq4Q: 'هل ممكن آخد قرض مقابل الصندوق؟',
    faq4A:
      'بعض الصناديق ممكن تتيح إمكانية الحصول على قرض مقابل الأتعاب الموجودة فيها. الشروط بتعتمد على الصندوق وبيانات العميل.',

    faq5Q: 'في أي التزام؟',
    faq5A:
      'لا. الفحص الأولي ببلاش وبدون أي التزام.',

    formTitle: 'بدك تعرف شو إلك؟',
    formText:
      'اترك تفاصيلك وممثل برجعلك. الفحص الأولي ببلاش وبدون التزام.',
    name: 'الاسم الكامل',
    phone: 'التلفون',
    submit: 'فحص ببلاش',
    sending: 'جارٍ الإرسال...',
    sent: 'وصلتنا التفاصيل',
    sentText: 'ممثل رح يتواصل معك بأقرب وقت.',

    privacy:
      'ترك التفاصيل ما يعني إنك ملتزم تعمل أي عملية.',

    footerText:
      'بانسرا بتقدم خدمات معلومات، بحث وتوجيه. الخدمات اللي بتحتاج ترخيص بتتم عن طريق مختص مرخّص، حسب القانون.',
    rights: 'جميع الحقوق محفوظة.'
  }
}

const t = computed(() => translations[language.value])

const currentDir = computed(() => {
  return languages.find(x => x.id === language.value)?.dir || 'rtl'
})

const sliderItems = [
  {
    name: 'קרנות השתלמות',
    icon: '◇',
    text: 'בדיקה ואיתור כספים'
  },
  {
    name: 'קופות גמל',
    icon: '◉',
    text: 'קופות וכספים'
  },
  {
    name: 'קרנות פנסיה',
    icon: '✦',
    text: 'מוצרים פנסיוניים'
  },
  {
    name: 'חברות ביטוח',
    icon: '◈',
    text: 'קופות וקרנות'
  },
  {
    name: 'בתי השקעות',
    icon: '◆',
    text: 'מוצרים פיננסיים'
  },
  {
    name: 'בדיקת משיכה',
    icon: '₪',
    text: 'בדיקת אפשרויות'
  },
  {
    name: 'הלוואה כנגד קופה',
    icon: '↗',
    text: 'בדיקת אפשרויות'
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

let sliderTimer = null

function setLanguage(id) {
  language.value = id
  mobileMenu.value = false
}

function toggleFaq(index) {
  openFaq.value = openFaq.value === index ? null : index
}

function nextSlide() {
  currentSlide.value =
    (currentSlide.value + 1) % sliderItems.length
}

function previousSlide() {
  currentSlide.value =
    (currentSlide.value - 1 + sliderItems.length) %
    sliderItems.length
}

function slideClass(index) {
  const diff =
    (index - currentSlide.value + sliderItems.length) %
    sliderItems.length

  if (diff === 0) return 'active'
  if (diff === 1) return 'next'
  if (diff === sliderItems.length - 1) return 'previous'

  return 'hidden-slide'
}

function submitForm() {
  formSent.value = true

  setTimeout(() => {
    formSent.value = false
  }, 5000)
}

onMounted(() => {
  sliderTimer = setInterval(() => {
    nextSlide()
  }, 3000)
})

onUnmounted(() => {
  clearInterval(sliderTimer)
})
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
                {{ language === 'ar'
                  ? 'بدون التزام'
                  : language === 'ru'
                  ? 'Без обязательств'
                  : 'ללא התחייבות'
                }}
              </div>

            </div>

          </div>

          <!-- HERO VISUAL -->
          <div class="hero-visual">

            <div class="floating-card card-top">
              <span class="mini-icon">₪</span>

              <div>
                <strong>+</strong>

                <small>
                  {{
                    language === 'ar'
                      ? 'أتعاب'
                      : language === 'ru'
                      ? 'Средства'
                      : 'כספים'
                  }}
                </small>
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

                <span>
                  {{
                    language === 'ar'
                      ? 'الأتعاب والصناديق'
                      : language === 'ru'
                      ? 'Кассы и средства'
                      : 'קופות וכספים'
                  }}
                </span>

                <strong>
                  {{
                    language === 'ar'
                      ? 'فحص'
                      : language === 'ru'
                      ? 'Проверка'
                      : 'בדיקה'
                  }}
                </strong>

              </div>

            </div>

            <div class="floating-card card-bottom">

              <span class="check-circle">✓</span>

              <div>

                <strong>
                  {{
                    language === 'ar'
                      ? 'فحص'
                      : language === 'ru'
                      ? 'Проверка'
                      : 'בדיקה'
                  }}
                </strong>

                <small>
                  {{
                    language === 'ar'
                      ? 'ببلاش'
                      : language === 'ru'
                      ? 'Бесплатно'
                      : 'ללא עלות'
                  }}
                </small>

              </div>

            </div>

          </div>

        </div>

        <!-- FREE STRIP -->
        <div class="free-strip">

          <div class="free-strip-inner">

            <span class="strip-icon">✓</span>

            <strong>
              {{ t.freeTitle }}
            </strong>

            <span>
              {{ t.freeText }}
            </span>

          </div>

        </div>

      </section>

      <!-- MOVING SLIDER -->
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
              v-for="(item, index) in sliderItems"
              :key="item.name"
              class="company-card"
              :class="slideClass(index)"
              @mouseenter="currentSlide = index"
            >

              <div class="company-icon">
                {{ item.icon }}
              </div>

              <h3>
                {{
                  language === 'ar'
                    ? (
                        index === 0 ? 'صناديق استكمال' :
                        index === 1 ? 'صناديق توفير' :
                        index === 2 ? 'صناديق تقاعد' :
                        index === 3 ? 'شركات تأمين' :
                        index === 4 ? 'بيوت استثمار' :
                        index === 5 ? 'فحص السحب' :
                        'قرض مقابل الصندوق'
                      )
                    : language === 'ru'
                    ? (
                        index === 0 ? 'Учебные фонды' :
                        index === 1 ? 'Накопительные кассы' :
                        index === 2 ? 'Пенсионные фонды' :
                        index === 3 ? 'Страховые компании' :
                        index === 4 ? 'Инвестиционные дома' :
                        index === 5 ? 'Проверка снятия' :
                        'Кредит под кассу'
                      )
                    : item.name
                }}
              </h3>

              <p>
                {{
                  language === 'ar'
                    ? 'فحص الأتعاب والخيارات'
                    : language === 'ru'
                    ? 'Проверка средств и вариантов'
                    : item.text
                }}
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
            v-for="(_, index) in sliderItems"
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
            {{ t.navServices }}
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
            {{
              language === 'ar'
                ? 'بسيط وواضح'
                : language === 'ru'
                ? 'Просто и понятно'
                : 'פשוט וברור'
            }}
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

/* NAVBAR */

.navbar {
  position: absolute;
  z-index: 100;
  top: 0;
  left: 0;

  width: 100%;

  background:
    rgba(255, 253, 248, .78);

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

/* LANGUAGES */

.language-switcher {
  display: flex;
  align-items: center;
  gap: 7px;

  padding: 7px;

  background:
    rgba(255,255,255,.95);

  border:
    1px solid
    rgba(24,37,43,.10);

  border-radius: 20px;

  box-shadow:
    0 8px 25px
    rgba(25,50,60,.08);
}

.language-button {
  min-height: 52px;

  padding: 8px 15px;

  display: flex;
  align-items: center;
  justify-content: center;

  gap: 8px;

  border-radius: 15px;

  background: transparent;

  color: #65757d;

  font-size: 16px;
  font-weight: 800;

  cursor: pointer;

  transition: .25s;
}

.language-button:hover,
.language-button.selected {
  background:
    var(--blue);

  color: white;

  transform:
    translateY(-2px);

  box-shadow:
    0 8px 22px
    rgba(22,167,199,.30);
}

.language-icon {
  font-size: 23px;
}

.arabic-icon {
  width: 29px;
  height: 29px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  font-size: 18px;
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

/* HERO */

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

/* GENERAL SECTIONS */

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

/* MOVING SLIDER */

.companies-section {
  background:
    #fffdf8;
}

.company-slider {
  position: relative;

  width: min(94vw, 1450px);

  margin: auto;

  min-height: 320px;

  display: flex;
  align-items: center;
}

.company-track {
  position: relative;

  width: 100%;
  height: 300px;

  overflow: visible;
}

.company-card {
  position: absolute;

  top: 50%;
  left: 50%;

  width: 310px;
  min-height: 235px;

  padding: 30px;

  border-radius: 28px;

  background:
    rgba(255,255,255,.96);

  border:
    1px solid
    rgba(22,167,199,.12);

  box-shadow:
    0 25px 70px
    rgba(35,65,75,.10);

  transition:
    transform .65s cubic-bezier(.2,.8,.2,1),
    opacity .5s,
    filter .5s,
    box-shadow .5s;

  transform:
    translate(-50%, -50%)
    scale(.72);

  opacity: 0;

  pointer-events: none;

  filter: blur(3px);
}

.company-card.active {
  transform:
    translate(-50%, -50%)
    scale(1.08);

  opacity: 1;

  pointer-events: auto;

  filter: blur(0);

  z-index: 5;

  box-shadow:
    0 30px 90px
    rgba(22,167,199,.18);
}

.company-card.next {
  transform:
    translate(
      calc(-50% + 360px),
      -50%
    )
    scale(.82);

  opacity: .65;

  z-index: 3;

  filter: blur(1px);
}

.company-card.previous {
  transform:
    translate(
      calc(-50% - 360px),
      -50%
    )
    scale(.82);

  opacity: .65;

  z-index: 3;

  filter: blur(1px);
}

.company-card.hidden-slide {
  opacity: 0;
}

.company-card:hover {
  box-shadow:
    0 35px 100px
    rgba(22,167,199,.23);
}

.company-icon {
  width: 58px;
  height: 58px;

  display: grid;
  place-items: center;

  border-radius: 18px;

  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue-dark)
    );

  font-size: 27px;

  box-shadow:
    0 15px 35px
    rgba(22,167,199,.20);
}

.company-card h3 {
  margin:
    22px 0 8px;

  font-size: 23px;

  font-weight: 800;
}

.company-card p {
  margin: 0;

  color: var(--muted);

  font-size: 16px;

  line-height: 1.5;
}

.company-line {
  width: 45px;
  height: 3px;

  margin-top: 24px;

  border-radius: 10px;

  background: var(--blue);
}

.company-arrow {
  position: absolute;

  bottom: 25px;
  right: 28px;

  color: var(--blue);

  font-size: 24px;
}

.slider-arrow {
  position: absolute;

  z-index: 20;

  width: 58px;
  height: 58px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: var(--blue);

  background:
    rgba(255,255,255,.96);

  border:
    1px solid
    rgba(22,167,199,.15);

  box-shadow:
    0 15px 40px
    rgba(30,60,70,.12);

  font-size: 27px;

  cursor: pointer;

  transition: .3s;
}

.slider-arrow:hover {
  transform:
    scale(1.12);

  color: white;

  background: var(--blue);

  box-shadow:
    0 15px 40px
    rgba(22,167,199,.30);
}

.slider-arrow-left {
  left: 0;
}

.slider-arrow-right {
  right: 0;
}

.slider-dots {
  display: flex;
  justify-content: center;

  gap: 8px;

  margin-top: 20px;
}

.slider-dots button {
  width: 9px;
  height: 9px;

  padding: 0;

  border-radius: 50%;

  background:
    rgba(22,167,199,.20);

  cursor: pointer;

  transition: .3s;
}

.slider-dots button.active {
  width: 28px;

  border-radius: 10px;

  background: var(--blue);
}

/* SERVICES */

.services-section {
  background:
    var(--cream);
}

.services-grid {
  width: min(92vw, 1300px);

  margin: auto;

  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  gap: 20px;
}

.service-card {
  position: relative;

  min-height: 330px;

  padding: 30px;

  overflow: hidden;

  border-radius: 26px;

  background:
    rgba(255,255,255,.88);

  border:
    1px solid
    rgba(24,37,43,.07);

  box-shadow:
    var(--shadow);

  transition: .35s;
}

.service-card:hover {
  transform:
    translateY(-8px);

  border-color:
    rgba(22,167,199,.25);

  box-shadow:
    0 30px 80px
    rgba(30,65,75,.13);
}

.service-number {
  position: absolute;

  top: 20px;
  right: 25px;

  color:
    rgba(22,167,199,.16);

  font-size: 42px;

  font-weight: 800;
}

.service-icon {
  width: 58px;
  height: 58px;

  display: grid;
  place-items: center;

  margin-bottom: 30px;

  border-radius: 18px;

  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue)
    );

  font-size: 26px;
}

.service-card h3 {
  margin: 0 0 13px;

  font-size: 23px;

  font-weight: 800;
}

.service-card p {
  margin: 0;

  color: var(--muted);

  font-size: 16px;

  line-height: 1.65;
}

.service-bottom {
  position: absolute;

  left: 30px;
  right: 30px;
  bottom: 25px;

  display: flex;
  justify-content: space-between;
  align-items: center;
}

.service-bottom span {
  width: 45px;
  height: 3px;

  border-radius: 10px;

  background: var(--blue);
}

.service-bottom b {
  color: var(--blue);

  font-size: 23px;
}

/* HOW */

.how-section {
  background:
    #fffdf8;
}

.steps {
  width: min(90vw, 1200px);

  margin: auto;

  display: grid;

  grid-template-columns:
    repeat(4, 1fr);

  gap: 30px;
}

.step {
  position: relative;

  text-align: center;
}

.step-number {
  width: 75px;
  height: 75px;

  display: grid;
  place-items: center;

  margin: 0 auto 22px;

  border-radius: 50%;

  color: white;

  background:
    linear-gradient(
      135deg,
      var(--blue-light),
      var(--blue-dark)
    );

  font-size: 20px;
  font-weight: 800;

  box-shadow:
    0 15px 35px
    rgba(22,167,199,.20);
}

.step-content h3 {
  margin: 0 0 8px;

  font-size: 21px;

  font-weight: 800;
}

.step-content p {
  margin: 0;

  color: var(--muted);

  font-size: 16px;
}

.step-line {
  position: absolute;

  top: 37px;

  left: 75%;

  width: 50%;

  height: 1px;

  background:
    rgba(22,167,199,.20);
}

/* CTA */

.free-cta {
  position: relative;

  overflow: hidden;

  padding:
    75px 0;

  background:
    linear-gradient(
      135deg,
      #0a829d,
      #16a7c7
    );

  color: white;
}

.free-cta-glow {
  position: absolute;

  width: 500px;
  height: 500px;

  top: -250px;
  right: -150px;

  border-radius: 50%;

  background:
    rgba(255,255,255,.13);

  filter:
    blur(20px);
}

.free-cta-content {
  position: relative;
  z-index: 2;

  width: min(90vw, 1200px);

  margin: auto;

  display: flex;
  align-items: center;

  gap: 30px;
}

.big-free-icon {
  width: 80px;
  height: 80px;

  flex: 0 0 auto;

  display: grid;
  place-items: center;

  border-radius: 25px;

  color: var(--blue-dark);

  background: white;

  font-size: 35px;
  font-weight: 800;

  box-shadow:
    0 20px 45px
    rgba(0,0,0,.12);
}

.cta-small {
  font-size: 15px;

  font-weight: 800;

  opacity: .85;
}

.free-cta h2 {
  margin:
    8px 0 5px;

  font-size: 37px;

  line-height: 1.1;
}

.free-cta p {
  margin: 0;

  opacity: .88;

  font-size: 17px;

  line-height: 1.5;
}

.white-button {
  margin-right: auto;

  min-height: 60px;

  padding:
    0 27px;

  display: inline-flex;

  align-items: center;
  justify-content: center;

  gap: 15px;

  border-radius: 16px;

  color: var(--blue-dark);

  background: white;

  font-size: 17px;
  font-weight: 800;

  white-space: nowrap;

  box-shadow:
    0 15px 35px
    rgba(0,0,0,.12);

  transition: .3s;
}

.white-button:hover {
  transform:
    translateY(-4px);

  box-shadow:
    0 20px 45px
    rgba(0,0,0,.18);
}

/* FAQ */

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

  border:
    1px solid
    rgba(24,37,43,.08);

  border-radius: 18px;

  background:
    rgba(255,255,255,.88);

  transition: .3s;
}

.faq-item.opened {
  border-color:
    rgba(22,167,199,.25);

  box-shadow:
    0 15px 45px
    rgba(30,65,75,.08);
}

.faq-question {
  width: 100%;

  min-height: 70px;

  padding:
    0 25px;

  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 20px;

  background: transparent;

  color: var(--text);

  text-align: inherit;

  font-size: 18px;
  font-weight: 800;

  cursor: pointer;
}

.faq-plus {
  color: var(--blue);

  font-size: 28px;

  flex: 0 0 auto;
}

.faq-answer {
  padding:
    0 25px 25px;

  color: var(--muted);

  font-size: 17px;

  line-height: 1.7;
}

/* CONTACT */

.contact-section {
  background:
    #fffdf8;
}

.contact-container {
  width: min(90vw, 1150px);

  margin: auto;

  display: grid;

  grid-template-columns:
    1fr
    .85fr;

  gap: 70px;

  align-items: center;
}

.contact-copy h2 {
  margin:
    20px 0 12px;

  font-size: 47px;

  line-height: 1.05;
}

.contact-copy > p {
  max-width: 580px;

  color: var(--muted);

  font-size: 19px;

  line-height: 1.65;
}

.dark-badge {
  background:
    rgba(22,167,199,.10);
}

.contact-benefits {
  display: grid;

  gap: 12px;

  margin-top: 25px;

  color: #61727a;

  font-size: 16px;

  font-weight: 600;
}

.contact-benefits div {
  display: flex;

  align-items: center;

  gap: 10px;
}

.contact-benefits span {
  width: 26px;
  height: 26px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);

  font-size: 13px;
}

.contact-form {
  padding: 38px;

  border-radius: 30px;

  background: white;

  border:
    1px solid
    rgba(24,37,43,.08);

  box-shadow:
    0 30px 80px
    rgba(35,65,75,.10);
}

.form-fields {
  display: grid;

  gap: 18px;
}

.form-fields label {
  display: grid;

  gap: 8px;

  color: var(--text);

  font-size: 15px;
  font-weight: 800;
}

.form-fields input {
  width: 100%;

  height: 58px;

  padding:
    0 17px;

  border:
    1px solid
    rgba(24,37,43,.12);

  border-radius: 14px;

  outline: none;

  background:
    #fffdfa;

  color: var(--text);

  font-size: 17px;

  transition: .25s;
}

.form-fields input:focus {
  border-color:
    var(--blue);

  box-shadow:
    0 0 0 4px
    rgba(22,167,199,.10);
}

.form-button {
  min-height: 61px;

  display: flex;

  align-items: center;
  justify-content: center;

  gap: 15px;

  border-radius: 15px;

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
    0 15px 35px
    rgba(22,167,199,.22);

  transition: .3s;
}

.form-button:hover {
  transform:
    translateY(-3px);

  box-shadow:
    0 20px 45px
    rgba(22,167,199,.30);
}

.form-fields small {
  color: #87959a;

  line-height: 1.5;

  text-align: center;
}

.success-message {
  min-height: 300px;

  display: flex;

  flex-direction: column;

  align-items: center;
  justify-content: center;

  text-align: center;
}

.success-icon {
  width: 70px;
  height: 70px;

  display: grid;
  place-items: center;

  border-radius: 50%;

  color: white;

  background: var(--blue);

  font-size: 30px;

  font-weight: 800;

  margin-bottom: 20px;
}

.success-message h3 {
  margin: 0 0 8px;

  font-size: 25px;
}

.success-message p {
  margin: 0;

  color: var(--muted);
}

/* FOOTER */

.footer {
  padding:
    45px 0;

  background:
    #18252b;

  color: white;
}

.footer-inner {
  width: min(90vw, 1200px);

  margin: auto;

  display: grid;

  grid-template-columns:
    auto
    1fr
    auto;

  align-items: center;

  gap: 35px;
}

.footer-logo {
  display: flex;

  align-items: center;

  gap: 10px;

  font-size: 23px;
}

.footer-logo .logo-mark {
  width: 38px;
  height: 38px;

  font-size: 18px;
}

.footer p {
  margin: 0;

  max-width: 700px;

  color:
    rgba(255,255,255,.65);

  font-size: 14px;

  line-height: 1.6;
}

.footer-rights {
  color:
    rgba(255,255,255,.55);

  font-size: 13px;
}

/* MOBILE */

@media (max-width: 1000px) {

  .desktop-nav {
    display: none;
  }

  .mobile-menu-button {
    display: block;
  }

  .hero-inner {
    grid-template-columns: 1fr;

    gap: 30px;

    padding-top: 140px;

    text-align: center;
  }

  .hero-content {
    margin: auto;
  }

  .hero-actions,
  .hero-trust {
    justify-content: center;
  }

  .hero-text {
    margin-left: auto;
    margin-right: auto;
  }

  .hero-visual {
    min-height: 420px;
  }

  .main-visual-card {
    height: 410px;
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

  .contact-container {
    grid-template-columns: 1fr;

    gap: 40px;
  }

  .footer-inner {
    grid-template-columns: 1fr;

    text-align: center;

    justify-items: center;
  }
}

@media (max-width: 700px) {

  .nav-inner {
    min-height: 76px;

    gap: 10px;
  }

  .logo {
    font-size: 23px;
  }

  .logo-mark {
    width: 38px;
    height: 38px;
  }

  .language-switcher {
    gap: 3px;

    padding: 4px;
  }

  .language-button {
    min-height: 43px;

    padding:
      6px 8px;

    font-size: 12px;

    gap: 4px;
  }

  .language-icon {
    font-size: 17px;
  }

  .arabic-icon {
    width: 22px;
    height: 22px;

    font-size: 14px;
  }

  .hero {
    min-height: 880px;
  }

  .hero-inner {
    width: 90vw;

    padding-top: 120px;
  }

  .hero h1 {
    font-size:
      clamp(39px, 11vw, 58px);

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

  .hero-trust {
    font-size: 13px;

    gap: 12px;
  }

  .hero-visual {
    min-height: 350px;
  }

  .main-visual-card {
    width: 280px;
    height: 350px;

    padding: 23px;
  }

  .visual-number {
    width: 110px;
    height: 110px;

    margin:
      55px auto 35px;

    font-size: 45px;
  }

  .visual-bottom {
    left: 23px;
    right: 23px;
    bottom: 22px;

    font-size: 11px;
  }

  .floating-card {
    min-width: 145px;

    padding:
      11px 13px;

    gap: 8px;
  }

  .mini-icon,
  .check-circle {
    width: 35px;
    height: 35px;

    font-size: 16px;
  }

  .card-top {
    top: 30px;
    right: -5px;
  }

  .card-bottom {
    bottom: 15px;
    left: -5px;
  }

  .free-strip-inner {
    min-height: 95px;

    padding:
      15px 10px;

    flex-direction: column;

    gap: 5px;

    font-size: 14px;
  }

  section:not(.hero) {
    padding:
      80px 0;
  }

  .section-heading {
    margin-bottom: 35px;
  }

  .section-heading h2 {
    font-size: 36px;
  }

  .section-heading p {
    font-size: 17px;
  }

  .company-slider {
    min-height: 340px;
  }

  .company-track {
    height: 300px;
  }

  .company-card {
    width: 270px;

    min-height: 230px;

    padding: 25px;
  }

  .company-card.next {
    transform:
      translate(
        calc(-50% + 180px),
        -50%
      )
      scale(.72);

    opacity: .25;
  }

  .company-card.previous {
    transform:
      translate(
        calc(-50% - 180px),
        -50%
      )
      scale(.72);

    opacity: .25;
  }

  .slider-arrow {
    width: 45px;
    height: 45px;

    font-size: 21px;
  }

  .slider-arrow-left {
    left: 5px;
  }

  .slider-arrow-right {
    right: 5px;
  }

  .services-grid {
    grid-template-columns: 1fr;
  }

  .service-card {
    min-height: 300px;
  }

  .steps {
    grid-template-columns: 1fr;

    gap: 35px;
  }

  .free-cta-content {
    flex-direction: column;

    text-align: center;
  }

  .white-button {
    margin-right: 0;

    width: 100%;
  }

  .free-cta h2 {
    font-size: 31px;
  }

  .contact-copy h2 {
    font-size: 39px;
  }

  .contact-form {
    padding: 25px;
  }

  .faq-question {
    min-height: 65px;

    padding:
      0 18px;

    font-size: 16px;
  }

  .faq-answer {
    padding:
      0 18px 20px;

    font-size: 16px;
  }
}

[dir="ltr"] .hero-inner {
  direction: ltr;
}

[dir="rtl"] .company-arrow {
  transform:
    scaleX(-1);
}

[dir="ltr"] .company-arrow {
  transform:
    scaleX(1);
}

[dir="rtl"] .white-button {
  margin-right: auto;
  margin-left: 0;
}

[dir="ltr"] .white-button {
  margin-left: auto;
  margin-right: 0;
}
</style>
