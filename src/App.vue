<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const lang = ref(localStorage.getItem('pensara-lang') || 'he')
const textScale = ref(1)
const highContrast = ref(false)
const menuOpen = ref(false)
const faqOpen = ref(null)
const activeSlide = ref(1)
const hoveredSlide = ref(null)

let timer = null

const content = {
  he: {
    dir: 'rtl',
    name: 'פנסרה',
    subtitle: 'פתרונות פיננסיים',

    freeBadge: 'מתחילים בחינם — ללא התחייבות',

    heroEyebrow: 'פנסרה',
    heroTitle1: 'יש לכם כסף',
    heroTitle2: 'בקופות?',
    heroTitle3: 'בואו נבדוק',
    heroTitle4: 'מה אפשר לעשות איתו.',

    heroText:
      'קרנות השתלמות, קופות גמל, חיסכון פנסיוני והלוואות כנגד קופות — אנחנו עוזרים לכם להבין את האפשרויות הקיימות ולבחון מה רלוונטי עבורכם.',

    start: 'מתחילים את הבדיקה בחינם',
    how: 'איך זה עובד?',

    simple: '✓ תהליך פשוט',
    personal: '✓ יחס אישי',
    clear: '✓ מידע ברור',

    formTitle: 'בדיקה חינמית לגמרי',
    formStrong: 'בואו נבדוק מה האפשרויות שלכם.',
    formText: 'הבדיקה וההדרכה על הכספים שאיתרנו — חינם.',

    name: 'שם מלא *',
    namePlaceholder: 'לדוגמה: דני לוי',

    phone: 'מספר טלפון *',
    phonePlaceholder: '050-0000000',

    interest: 'מה מעניין אתכם?',
    choose: 'בחרו אפשרות',

    withdrawal: 'משיכת כספים',
    training: 'קרן השתלמות',
    provident: 'קופת גמל',
    loan: 'הלוואה כנגד קופה',
    unsure: 'אני לא בטוח/ה',

    consent:
      'אני מאשר/ת יצירת קשר בהתאם למדיניות הפרטיות.',

    submit: 'מתחילים את הבדיקה בחינם',

    secure: '🔒 פרטים מאובטחים',
    noCommit: 'ללא התחייבות',

    servicesEyebrow: 'מה אפשר לעשות?',
    servicesTitle: 'הכסף שלכם.',
    servicesTitle2: 'רק צריך לדעת איפה.',

    howEyebrow: 'איך זה עובד?',
    howTitle: '3 שלבים.',
    howTitle2: 'בלי כאב ראש.',
    howText:
      'מתחילים בבדיקה. מבינים את הנתונים. ורק אז בוחנים את האפשרויות.',

    companiesEyebrow: 'גופים מוסדיים',
    companiesTitle: 'בודקים את',
    companiesTitle2: 'האפשרויות.',
    companiesText:
      'במידת הרלוונטיות, ניתן לבחון נתונים הקשורים לקופות ולחסכונות המנוהלים בגופים שונים.',

    faqEyebrow: 'שאלות נפוצות',
    faqTitle: 'יש שאלה?',
    faqTitle2: 'בדקנו גם אותה.',

    ctaEyebrow: 'לא יודעים מאיפה להתחיל?',
    ctaTitle: 'לא צריך לנחש.',
    ctaTitle2: 'מתחילים בבדיקה.',

    faqs: [
      [
        'אפשר למשוך כסף מקופת גמל או קרן השתלמות?',
        'זה תלוי בסוג המוצר, במועד ההפקדות, בסוג הכספים ובנסיבות האישיות. אנחנו מתחילים בבדיקה ומסבירים מה ניתן לבחון.'
      ],
      [
        'האם תמיד עדיף למשוך את הכסף?',
        'לא. במקרים מסוימים ניתן לבחון חלופות כמו הלוואה כנגד הקופה. המטרה היא להבין את האפשרויות לפני שמקבלים החלטה.'
      ],
      [
        'האם הבדיקה הראשונית מחייבת?',
        'לא. השארת הפרטים מאפשרת לנו להבין את הצורך ולבחון את האפשרויות הרלוונטיות.'
      ],
      [
        'כמה מס משלמים על משיכה?',
        'המס, אם חל, משתנה בהתאם למוצר, לסוג הכספים ולנסיבות. לכן אין סכום אחיד שמתאים לכל לקוח.'
      ],
      [
        'כמה זמן התהליך לוקח?',
        'מילוי הטופס — פחות מדקה. נציג יחזור תוך 24 שעות (ימים א׳-ה׳, 9:00-18:00). משיכת הכסף — בדרך כלל 7-14 ימי עסקים.'
      ],
      [
        'איך הבדיקה יכולה להיות בחינם?',
        'הבדיקה היא חינמית לגמרי. הבדיקה וההדרכה על הכספים שאיתרנו — לגמרי חינם. במקרים שנדרש יועץ להשלמת משיכה, נציג יציע לך הצעה ותוכל/י להחליט אם להמשיך.'
      ]
    ],

    disclaimer:
      'המידע באתר הינו כללי ואינו מהווה ייעוץ פנסיוני, ייעוץ השקעות, התחייבות לקבלת הלוואה או התחייבות לביצוע משיכה. האפשרויות והתנאים כפופים לנתונים האישיים ולתנאי הגופים הרלוונטיים.'
  },

  ru: {
    dir: 'ltr',
    name: 'Пенсера',
    subtitle: 'Финансовые решения',

    freeBadge: 'Начинаем бесплатно — без обязательств',

    heroEyebrow: 'Пенсера',
    heroTitle1: 'У вас есть деньги',
    heroTitle2: 'в накоплениях?',
    heroTitle3: 'Давайте проверим',
    heroTitle4: 'какие есть возможности.',

    heroText:
      'Накопительные фонды, пенсионные продукты и кредиты под залог накоплений — мы помогаем понять доступные варианты и определить, что может подойти именно вам.',

    start: 'Начать бесплатную проверку',
    how: 'Как это работает?',

    simple: '✓ Простой процесс',
    personal: '✓ Персональное отношение',
    clear: '✓ Понятная информация',

    formTitle: 'Полностью бесплатная проверка',
    formStrong: 'Давайте проверим ваши возможности.',
    formText: 'Проверка и объяснение найденных средств — бесплатно.',

    name: 'Полное имя *',
    namePlaceholder: 'Например: Иван Иванов',

    phone: 'Номер телефона *',
    phonePlaceholder: '050-0000000',

    interest: 'Что вас интересует?',
    choose: 'Выберите вариант',

    withdrawal: 'Вывод средств',
    training: 'Накопительный фонд',
    provident: 'Купат Гемель',
    loan: 'Кредит под накопления',
    unsure: 'Я не уверен/а',

    consent:
      'Я разрешаю связаться со мной в соответствии с политикой конфиденциальности.',

    submit: 'Начать бесплатную проверку',

    secure: '🔒 Защищённые данные',
    noCommit: 'Без обязательств',

    servicesEyebrow: 'Что можно проверить?',
    servicesTitle: 'Ваши деньги.',
    servicesTitle2: 'Нужно лишь понять возможности.',

    howEyebrow: 'Как это работает?',
    howTitle: '3 шага.',
    howTitle2: 'Без лишних сложностей.',
    howText:
      'Начинаем с проверки. Разбираемся в данных. Затем рассматриваем варианты.',

    companiesEyebrow: 'Институциональные организации',
    companiesTitle: 'Проверяем',
    companiesTitle2: 'возможности.',
    companiesText:
      'При необходимости можно проверить данные, связанные с накоплениями и счетами, управляемыми различными организациями.',

    faqEyebrow: 'Частые вопросы',
    faqTitle: 'Есть вопрос?',
    faqTitle2: 'Мы подготовили ответ.',

    ctaEyebrow: 'Не знаете, с чего начать?',
    ctaTitle: 'Не нужно гадать.',
    ctaTitle2: 'Начните с проверки.',

    faqs: [
      [
        'Можно ли вывести деньги из Купат Гемель или накопительного фонда?',
        'Это зависит от типа продукта, даты взносов, вида средств и личных обстоятельств. Мы начинаем с проверки и объясняем доступные варианты.'
      ],
      [
        'Всегда ли лучше выводить деньги?',
        'Нет. В некоторых случаях можно рассмотреть альтернативы, например кредит под накопления. Важно понять варианты до принятия решения.'
      ],
      [
        'Обязательна ли первичная проверка?',
        'Нет. Оставив данные, вы позволяете нам понять запрос и проверить соответствующие возможности.'
      ],
      [
        'Сколько времени занимает процесс?',
        'Заполнение формы — менее минуты. Представитель свяжется в течение 24 часов (вс–чт, 9:00–18:00). Вывод средств — обычно 7–14 рабочих дней.'
      ],
      [
        'Почему проверка бесплатна?',
        'Проверка полностью бесплатна. Проверка и объяснение найденных средств — бесплатно. Если для завершения вывода потребуется консультант, представитель предложит условия, а вы решите, продолжать ли.'
      ]
    ],

    disclaimer:
      'Информация на сайте носит общий характер и не является пенсионной или инвестиционной консультацией, гарантией получения кредита или гарантией осуществления вывода средств. Возможности и условия зависят от личных данных и условий соответствующих организаций.'
  },

  ar: {
    dir: 'rtl',
    name: 'بنصرا',
    subtitle: 'حلول مالية',

    freeBadge: 'نبدأ مجانًا — بدون التزام',

    heroEyebrow: 'بنصرا',
    heroTitle1: 'لديكم أموال',
    heroTitle2: 'في الصناديق؟',
    heroTitle3: 'دعونا نفحص',
    heroTitle4: 'ما هي الخيارات المتاحة.',

    heroText:
      'صناديق الاستكمال، صناديق الادخار، التقاعد والقروض مقابل المدخرات — نساعدكم على فهم الخيارات المتاحة وفحص ما قد يكون مناسبًا لكم.',

    start: 'ابدأوا الفحص مجانًا',
    how: 'كيف يعمل الأمر؟',

    simple: '✓ عملية بسيطة',
    personal: '✓ تعامل شخصي',
    clear: '✓ معلومات واضحة',

    formTitle: 'الفحص مجاني بالكامل',
    formStrong: 'دعونا نفحص الخيارات المتاحة لكم.',
    formText: 'الفحص والإرشاد حول الأموال التي تم العثور عليها — مجانًا.',

    name: 'الاسم الكامل *',
    namePlaceholder: 'مثال: داني ليفي',

    phone: 'رقم الهاتف *',
    phonePlaceholder: '050-0000000',

    interest: 'ما الذي يهمكم؟',
    choose: 'اختاروا خيارًا',

    withdrawal: 'سحب الأموال',
    training: 'صندوق استكمال',
    provident: 'صندوق ادخار',
    loan: 'قرض مقابل صندوق',
    unsure: 'لست متأكدًا/متأكدة',

    consent:
      'أوافق على التواصل معي وفقًا لسياسة الخصوصية.',

    submit: 'ابدأوا الفحص مجانًا',

    secure: '🔒 بيانات آمنة',
    noCommit: 'بدون التزام',

    servicesEyebrow: 'ماذا يمكننا فحصه؟',
    servicesTitle: 'أموالكم.',
    servicesTitle2: 'نحتاج فقط إلى فهم الخيارات.',

    howEyebrow: 'كيف يعمل الأمر؟',
    howTitle: '3 خطوات.',
    howTitle2: 'بدون تعقيد.',
    howText:
      'نبدأ بالفحص. نفهم البيانات. وبعد ذلك نفحص الخيارات.',

    companiesEyebrow: 'جهات مؤسسية',
    companiesTitle: 'نفحص',
    companiesTitle2: 'الخيارات.',
    companiesText:
      'عند الحاجة، يمكن فحص البيانات المتعلقة بالصناديق والمدخرات التي تديرها جهات مختلفة.',

    faqEyebrow: 'أسئلة شائعة',
    faqTitle: 'لديكم سؤال؟',
    faqTitle2: 'أعددنا الإجابة.',

    ctaEyebrow: 'لا تعرفون من أين تبدأون؟',
    ctaTitle: 'لا داعي للتخمين.',
    ctaTitle2: 'ابدأوا بالفحص.',

    faqs: [
      [
        'هل يمكن سحب الأموال من صندوق ادخار أو صندوق استكمال؟',
        'يعتمد ذلك على نوع المنتج وتاريخ الإيداعات ونوع الأموال والظروف الشخصية. نبدأ بالفحص ونوضح الخيارات التي يمكن بحثها.'
      ],
      [
        'هل من الأفضل دائمًا سحب الأموال؟',
        'ليس بالضرورة. في بعض الحالات يمكن فحص بدائل مثل القرض مقابل المدخرات. الهدف هو فهم الخيارات قبل اتخاذ القرار.'
      ],
      [
        'هل الفحص الأولي يلزمني بأي شيء؟',
        'لا. ترك التفاصيل يسمح لنا بفهم الحاجة وفحص الخيارات ذات الصلة.'
      ],
      [
        'كم يستغرق الإجراء؟',
        'تعبئة النموذج — أقل من دقيقة. سيتواصل معكم ممثل خلال 24 ساعة (الأحد–الخميس، 9:00–18:00). سحب الأموال — عادةً 7–14 يوم عمل.'
      ],
      [
        'كيف يمكن أن يكون الفحص مجانيًا؟',
        'الفحص مجاني بالكامل. الفحص والإرشاد حول الأموال التي تم العثور عليها — مجانًا. إذا تطلب إكمال السحب مستشارًا، سيقدم لكم ممثل عرضًا ويمكنكم اتخاذ قرار بشأن المتابعة.'
      ]
    ],

    disclaimer:
      'المعلومات في الموقع عامة ولا تشكل استشارة تقاعدية أو استثمارية أو التزامًا بالحصول على قرض أو تنفيذ سحب. الخيارات والشروط تخضع للبيانات الشخصية وشروط الجهات ذات الصلة.'
  }
}

const current = computed(() => content[lang.value])

const services = computed(() => [
  {
    title: current.value.withdrawal,
    text:
      lang.value === 'he'
        ? 'בדיקה של אפשרויות המשיכה בהתאם לסוג הקופה, הכספים והנסיבות.'
        : lang.value === 'ru'
          ? 'Проверка вариантов вывода средств в зависимости от продукта, средств и обстоятельств.'
          : 'فحص خيارات سحب الأموال وفقًا لنوع الصندوق والأموال والظروف.',
    icon: '₪'
  },
  {
    title: current.value.training,
    text:
      lang.value === 'he'
        ? 'הבנת מצב הקרן ובחינת האפשרויות הקיימות.'
        : lang.value === 'ru'
          ? 'Понимание состояния фонда и проверка доступных вариантов.'
          : 'فهم وضع الصندوق وفحص الخيارات المتاحة.',
    icon: '↗'
  },
  {
    title: current.value.provident,
    text:
      lang.value === 'he'
        ? 'בדיקה של הכספים והאפשרויות שניתן לבחון.'
        : lang.value === 'ru'
          ? 'Проверка средств и доступных вариантов.'
          : 'فحص الأموال والخيارات التي يمكن بحثها.',
    icon: '◇'
  },
  {
    title: current.value.loan,
    text:
      lang.value === 'he'
        ? 'בחינת אפשרות לקבלת מימון כנגד חיסכון קיים, בכפוף לתנאים.'
        : lang.value === 'ru'
          ? 'Проверка возможности кредита под существующие накопления, в соответствии с условиями.'
          : 'فحص إمكانية الحصول على تمويل مقابل مدخرات قائمة، وفقًا للشروط.',
    icon: '+'
  }
])

const companies = [
  {
    name: 'הפניקס',
    type: 'גוף מוסדי',
    icon: '◈'
  },
  {
    name: 'הראל',
    type: 'גוף מוסדי',
    icon: '◇'
  },
  {
    name: 'מגדל',
    type: 'גוף מוסדי',
    icon: '✦'
  },
  {
    name: 'כלל',
    type: 'גוף מוסדי',
    icon: '◉'
  },
  {
    name: 'מנורה',
    type: 'גוף מוסדי',
    icon: '◆'
  },
  {
    name: 'מיטב',
    type: 'בית השקעות',
    icon: '✧'
  }
]

const visibleCompanies = computed(() => {
  const result = []

  for (let i = -1; i <= 1; i++) {
    const index =
      (activeSlide.value + i + companies.length) %
      companies.length

    result.push({
      ...companies[index],
      position: i
    })
  }

  return result
})

function setLang(value) {
  lang.value = value

  localStorage.setItem(
    'pensara-lang',
    value
  )

  document.documentElement.lang = value
  document.documentElement.dir =
    content[value].dir

  menuOpen.value = false
}

function nextSlide() {
  activeSlide.value =
    (activeSlide.value + 1) %
    companies.length
}

function prevSlide() {
  activeSlide.value =
    (activeSlide.value - 1 + companies.length) %
    companies.length
}

function startSlider() {
  stopSlider()

  timer = setInterval(
    nextSlide,
    3500
  )
}

function stopSlider() {
  if (timer) {
    clearInterval(timer)
    timer = null
  }
}

function toggleFaq(index) {
  faqOpen.value =
    faqOpen.value === index
      ? null
      : index
}

function increaseText() {
  textScale.value =
    Math.min(
      1.25,
      +(textScale.value + 0.1).toFixed(2)
    )
}

function decreaseText() {
  textScale.value =
    Math.max(
      0.9,
      +(textScale.value - 0.1).toFixed(2)
    )
}

onMounted(() => {
  document.documentElement.lang =
    lang.value

  document.documentElement.dir =
    current.value.dir

  startSlider()
})

onUnmounted(() => {
  stopSlider()
})
</script>

<template>
  <div
    class="site"
    :class="{
      'high-contrast': highContrast
    }"
    :style="{
      '--text-scale': textScale
    }"
  >

    <!-- HEADER -->
    <header class="header">

      <div class="nav">

        <a href="#" class="logo">

          <span class="logo-mark">
            פ
          </span>

          <span>
            <strong>
              {{ current.name }}
            </strong>

            <small>
              {{ current.subtitle }}
            </small>
          </span>

        </a>


        <nav class="desktop-nav">

          <a href="#services">
            {{ current.servicesEyebrow }}
          </a>

          <a href="#how">
            {{ current.how }}
          </a>

          <a href="#companies">
            {{ current.companiesEyebrow }}
          </a>

          <a href="#faq">
            {{ current.faqEyebrow }}
          </a>

        </nav>


        <div class="header-actions">

          <!-- LANGUAGES -->

          <div
            class="languages"
            aria-label="בחירת שפה"
          >

            <button
              :class="{ selected: lang === 'he' }"
              @click="setLang('he')"
            >
              עברית
            </button>

            <button
              :class="{ selected: lang === 'ru' }"
              @click="setLang('ru')"
            >
              Русский
            </button>

            <button
              :class="{ selected: lang === 'ar' }"
              @click="setLang('ar')"
            >
              العربية
            </button>

          </div>


          <!-- ACCESSIBILITY -->

          <button
            class="accessibility-top"
            @click="increaseText"
            title="הגדלת טקסט"
          >
            A+
          </button>

          <button
            class="accessibility-top"
            @click="decreaseText"
            title="הקטנת טקסט"
          >
            A−
          </button>

          <button
            class="accessibility-top"
            @click="highContrast = !highContrast"
            title="ניגודיות גבוהה"
          >
            ◐
          </button>


          <a
            href="#contact"
            class="nav-button"
          >
            {{ current.start }}
            <span>«</span>
          </a>

        </div>


        <button
          class="mobile-menu"
          @click="menuOpen = !menuOpen"
          aria-label="תפריט"
        >
          {{ menuOpen ? '×' : '☰' }}
        </button>

      </div>


      <!-- MOBILE MENU -->

      <div
        v-if="menuOpen"
        class="mobile-nav"
      >

        <div class="mobile-languages">

          <button
            :class="{ selected: lang === 'he' }"
            @click="setLang('he')"
          >
            🇮🇱 עברית
          </button>

          <button
            :class="{ selected: lang === 'ru' }"
            @click="setLang('ru')"
          >
            🇷🇺 Русский
          </button>

          <button
            :class="{ selected: lang === 'ar' }"
            @click="setLang('ar')"
          >
            🇸🇦 العربية
          </button>

        </div>


        <div class="mobile-access">

          <button
            @click="increaseText"
          >
            A+
          </button>

          <button
            @click="decreaseText"
          >
            A−
          </button>

          <button
            @click="highContrast = !highContrast"
          >
            ◐
          </button>

        </div>


        <a
          href="#services"
          @click="menuOpen = false"
        >
          {{ current.servicesEyebrow }}
        </a>

        <a
          href="#how"
          @click="menuOpen = false"
        >
          {{ current.how }}
        </a>

        <a
          href="#companies"
          @click="menuOpen = false"
        >
          {{ current.companiesEyebrow }}
        </a>

        <a
          href="#faq"
          @click="menuOpen = false"
        >
          {{ current.faqEyebrow }}
        </a>

      </div>

    </header>


    <!-- TICKER -->

    <div class="ticker">

      <div class="ticker-track">

        <span>
          {{ current.training }}
        </span>

        <b>»</b>

        <span>
          {{ current.provident }}
        </span>

        <b>»</b>

        <span>
          {{ current.withdrawal }}
        </span>

        <b>»</b>

        <span>
          {{ current.loan }}
        </span>

        <b>»</b>

        <span>
          {{ current.subtitle }}
        </span>

        <b>»</b>

        <span>
          {{ current.training }}
        </span>

        <b>»</b>

        <span>
          {{ current.provident }}
        </span>

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
        >

          <div class="card-badge">

            <span></span>

            {{ current.freeBadge }}

          </div>


          <h2>

            {{ current.formTitle }}

            <strong>
              {{ current.formStrong }}
            </strong>

          </h2>


          <p class="form-subtitle">
            {{ current.formText }}
          </p>


          <div class="free-highlight">

            <span>✓</span>

            {{ current.formText }}

          </div>


          <div class="progress">
            <span></span>
          </div>


          <label>
            {{ current.name }}
          </label>

          <input
            type="text"
            :placeholder="current.namePlaceholder"
          />


          <label>
            {{ current.phone }}
          </label>

          <input
            type="tel"
            dir="ltr"
            :placeholder="current.phonePlaceholder"
          />


          <label>
            {{ current.interest }}
          </label>


          <select>

            <option>
              {{ current.choose }}
            </option>

            <option>
              {{ current.withdrawal }}
            </option>

            <option>
              {{ current.training }}
            </option>

            <option>
              {{ current.provident }}
            </option>

            <option>
              {{ current.loan }}
            </option>

            <option>
              {{ current.unsure }}
            </option>

          </select>


          <label class="check">

            <input
              type="checkbox"
            />

            <span>
              {{ current.consent }}
            </span>

          </label>


          <button
            class="submit-button"
          >

            {{ current.submit }}

            <span>
              «
            </span>

          </button>


          <div class="security">

            <span>
              {{ current.secure }}
            </span>

            <span>
              •
            </span>

            <span>
              {{ current.noCommit }}
            </span>

          </div>

        </div>


        <!-- HERO TEXT -->

        <div class="hero-text">

          <div class="live-pill">

            <span class="live-dot"></span>

            {{ current.freeBadge }}

          </div>


          <p class="eyebrow">

            {{ current.heroEyebrow }}

            <span>»</span>

          </p>


          <h1>

            {{ current.heroTitle1 }}

            <br />

            <span>
              {{ current.heroTitle2 }}
            </span>

            <br />

            <strong>
              {{ current.heroTitle3 }}
            </strong>

            <br />

            {{ current.heroTitle4 }}

          </h1>


          <p class="hero-description">
            {{ current.heroText }}
          </p>


          <div class="hero-actions">

            <a
              href="#contact"
              class="primary-button"
            >

              {{ current.start }}

              <span>
                «
              </span>

            </a>


            <a
              href="#how"
              class="secondary-button"
            >

              {{ current.how }}

              <span>
                ↓
              </span>

            </a>

          </div>


          <div class="trust-row">

            <span>
              {{ current.simple }}
            </span>

            <span>
              {{ current.personal }}
            </span>

            <span>
              {{ current.clear }}
            </span>

          </div>

        </div>

      </div>

    </section>


    <!-- HOW IT WORKS -->

    <section
      id="how"
      class="section dark-section"
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.howEyebrow }}

          <span>»</span>

        </p>


        <h2>

          {{ current.howTitle }}

          <span>
            {{ current.howTitle2 }}
          </span>

        </h2>


        <p>
          {{ current.howText }}
        </p>

      </div>


      <div class="steps">


        <div class="step">

          <div class="step-number">
            01
          </div>

          <div class="step-arrow">
            «
          </div>

          <h3>

            {{
              lang === 'he'
                ? 'משאירים פרטים'
                : lang === 'ru'
                  ? 'Оставляете данные'
                  : 'ترك التفاصيل'
            }}

          </h3>

          <p>

            {{
              lang === 'he'
                ? 'שם, טלפון וכמה פרטים בסיסיים. זה הכול.'
                : lang === 'ru'
                  ? 'Имя, телефон и несколько основных данных.'
                  : 'الاسم ورقم الهاتف وبعض التفاصيل الأساسية.'
            }}

          </p>

        </div>


        <div class="step active-step">

          <div class="step-number">
            02
          </div>

          <div class="step-arrow">
            «
          </div>

          <h3>

            {{
              lang === 'he'
                ? 'בודקים את האפשרויות'
                : lang === 'ru'
                  ? 'Проверяем варианты'
                  : 'نفحص الخيارات'
            }}

          </h3>

          <p>

            {{
              lang === 'he'
                ? 'בוחנים את הנתונים ומבינים אילו אפשרויות ניתן לבדוק.'
                : lang === 'ru'
                  ? 'Изучаем данные и определяем, какие варианты можно рассмотреть.'
                  : 'نفحص البيانات ونحدد الخيارات التي يمكن بحثها.'
            }}

          </p>

        </div>


        <div class="step">

          <div class="step-number">
            03
          </div>

          <h3>

            {{
              lang === 'he'
                ? 'מבינים ומחליטים'
                : lang === 'ru'
                  ? 'Понимаете и решаете'
                  : 'تفهمون وتقررون'
            }}

          </h3>

          <p>

            {{
              lang === 'he'
                ? 'מסבירים את האפשרויות ואת המשמעות שלהן.'
                : lang === 'ru'
                  ? 'Объясняем варианты и их значение.'
                  : 'نشرح الخيارات ومعناها.'
            }}

          </p>

        </div>

      </div>

    </section>


    <!-- SERVICES -->

    <section
      id="services"
      class="section dark-section services-section"
    >

      <div class="section-heading right-heading">

        <p class="eyebrow">

          {{ current.servicesEyebrow }}

          <span>»</span>

        </p>


        <h2>

          {{ current.servicesTitle }}

          <br />

          <span>
            {{ current.servicesTitle2 }}
          </span>

        </h2>

      </div>


      <div class="services-grid">

        <article
          v-for="(service, index) in services"
          :key="service.title"
          class="service-card"
          :class="{
            featured: index === 0
          }"
        >

          <div class="service-icon">
            {{ service.icon }}
          </div>

          <div class="service-number">
            0{{ index + 1 }}
          </div>


          <h3>
            {{ service.title }}
          </h3>


          <p>
            {{ service.text }}
          </p>


          <a href="#contact">

            {{ current.start }}

            <span>
              «
            </span>

          </a>

        </article>

      </div>

    </section>


    <!-- COMPANIES -->

    <section
      id="companies"
      class="section dark-section company-section"
      @mouseenter="stopSlider"
      @mouseleave="startSlider"
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.companiesEyebrow }}

          <span>»</span>

        </p>


        <h2>

          {{ current.companiesTitle }}

          <span>
            {{ current.companiesTitle2 }}
          </span>

        </h2>


        <p>
          {{ current.companiesText }}
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
            @click="
              activeSlide =
                companies.findIndex(
                  c => c.name === company.name
                )
            "
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

              {{
                lang === 'he'
                  ? 'בחינת אפשרויות בהתאם לנתונים הרלוונטיים.'
                  : lang === 'ru'
                    ? 'Проверка вариантов в соответствии с актуальными данными.'
                    : 'فحص الخيارات وفقًا للبيانات ذات الصلة.'
              }}

            </p>


            <div class="company-bottom">

              <span>
                {{ current.servicesEyebrow }}
              </span>

              <b>
                »
              </b>

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
          :class="{
            selected:
              index === activeSlide
          }"
          @click="
            activeSlide = index
          "
        ></button>

      </div>

    </section>


    <!-- CTA -->

    <section class="cta-section">

      <div class="cta-glow"></div>


      <div class="cta-content">

        <p class="eyebrow">
          {{ current.ctaEyebrow }}
        </p>


        <h2>

          {{ current.ctaTitle }}

          <br />

          <span>
            {{ current.ctaTitle2 }}
          </span>

        </h2>


        <p>
          {{ current.formText }}
        </p>


        <a
          href="#contact"
          class="primary-button"
        >

          {{ current.start }}

          <span>
            «
          </span>

        </a>

      </div>

    </section>


    <!-- FAQ -->

    <section
      id="faq"
      class="section faq-section"
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.faqEyebrow }}

          <span>»</span>

        </p>


        <h2>

          {{ current.faqTitle }}

          <span>
            {{ current.faqTitle2 }}
          </span>

        </h2>

      </div>


      <div class="faq-list">

        <div
          v-for="(faq, index) in current.faqs"
          :key="faq[0]"
          class="faq-item"
          :class="{
            open:
              faqOpen === index
          }"
        >

          <button
            @click="
              toggleFaq(index)
            "
          >

            <span>
              {{ faq[0] }}
            </span>


            <b>

              {{
                faqOpen === index
                  ? '−'
                  : '+'
              }}

            </b>

          </button>


          <div
            v-if="
              faqOpen === index
            "
            class="faq-answer"
          >

            {{ faq[1] }}

          </div>

        </div>

      </div>

    </section>


    <!-- FOOTER -->

    <footer>

      <div class="footer-main">

        <div class="logo footer-logo">

          <span class="logo-mark">
            פ
          </span>

          <span>

            <strong>
              {{ current.name }}
            </strong>

            <small>
              {{ current.subtitle }}
            </small>

          </span>

        </div>


        <div class="footer-links">

          <a href="#services">
            {{ current.servicesEyebrow }}
          </a>

          <a href="#how">
            {{ current.how }}
          </a>

          <a href="#companies">
            {{ current.companiesEyebrow }}
          </a>

          <a href="#faq">
            {{ current.faqEyebrow }}
          </a>

        </div>

      </div>


      <div class="footer-disclaimer">
        {{ current.disclaimer }}
      </div>


      <div class="footer-copy">

        © {{ new Date().getFullYear() }}

        {{ current.name }}.

        {{
          lang === 'he'
            ? 'כל הזכויות שמורות.'
            : lang === 'ru'
              ? 'Все права защищены.'
              : 'جميع الحقوق محفوظة.'
        }}

      </div>

    </footer>


    <!-- FLOATING CTA -->

    <a
      href="#contact"
      class="floating-cta"
    >
      {{ current.start }}
    </a>

  </div>
</template>


<style>
@import url('https://fonts.googleapis.com/css2?family=Heebo:wght@400;500;600;700;800;900&family=Rubik:wght@400;500;600;700;800;900&display=swap');

:root {
  --bg: #080b10;
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
  font-family:
    'Heebo',
    'Rubik',
    Arial,
    sans-serif;
}

button,
input,
select {
  font-family: inherit;
}

a {
  color: inherit;
  text-decoration: none;
}

.site {
  min-height: 100vh;
  color: var(--text);
  font-size:
    calc(16px * var(--text-scale));
}


/* =========================
   HEADER
========================= */

.header {
  position: fixed;
  inset: 0 0 auto;
  z-index: 100;
  background:
    rgba(7,10,15,.9);
  border-bottom:
    1px solid rgba(255,255,255,.08);
  backdrop-filter: blur(18px);
}

.nav {
  height: 76px;
  max-width: 1180px;
  margin: auto;
  padding: 0 24px;

  display: flex;
  align-items: center;
  justify-content: space-between;

  gap: 25px;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
}

.logo-mark {
  width: 42px;
  height: 42px;

  border-radius: 12px;

  background:
    var(--cyan);

  color: #061018;

  display: flex;
  align-items: center;
  justify-content: center;

  font-weight: 900;
  font-size: 21px;
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
  gap: 24px;

  color: #9ba3b0;

  font-size: 13px;
  font-weight: 700;
}

.desktop-nav a {
  transition: .25s;
}

.desktop-nav a:hover {
  color: var(--cyan);
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 7px;
}

.languages {
  display: flex;
  padding: 3px;

  border:
    1px solid rgba(255,255,255,.12);

  border-radius: 999px;

  background:
    rgba(255,255,255,.04);
}

.languages button {
  border: 0;
  background: transparent;

  color: #929aa8;

  cursor: pointer;

  padding: 7px 9px;

  border-radius: 999px;

  font-size: 11px;
  font-weight: 800;

  transition: .25s;
}

.languages button.selected {
  background:
    var(--cyan);

  color: #061018;
}

.accessibility-top {
  width: 32px;
  height: 32px;

  border:
    1px solid rgba(255,255,255,.12);

  border-radius: 8px;

  background: #11161d;

  color: #b7c0ca;

  cursor: pointer;

  font-weight: 900;
}

.accessibility-top:hover {
  color: var(--cyan);
  border-color: var(--cyan);
}

.nav-button,
.primary-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;

  background: var(--cyan);
  color: #061018;

  font-weight: 900;

  border-radius: 999px;

  padding: 12px 19px;

  transition: .3s;

  white-space: nowrap;
}

.nav-button:hover,
.primary-button:hover {
  background:
    var(--cyan-light);

  transform:
    translateY(-2px);

  box-shadow:
    0 10px 30px
    rgba(55,201,245,.2);
}

.nav-button span,
.primary-button span {
  margin-right: 8px;
}

.mobile-menu {
  display: none;

  border: 0;
  background: transparent;

  color: white;

  font-size: 25px;
}


/* MOBILE MENU */

.mobile-nav {
  display: none;
}


/* =========================
   TICKER
========================= */

.ticker {
  position: relative;
  z-index: 5;

  margin-top: 76px;

  height: 48px;

  overflow: hidden;

  background:
    #0c1016;

  border-bottom:
    1px solid var(--border);
}

.ticker-track {
  width: max-content;
  height: 100%;

  display: flex;
  align-items: center;

  gap: 28px;

  color: #777f8d;

  font-size: 13px;
  font-weight: 800;

  animation:
    ticker 28s linear infinite;
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


/* =========================
   HERO
========================= */

.hero {
  position: relative;

  min-height: 790px;

  overflow: hidden;

  background:
    radial-gradient(
      circle at 75% 30%,
      rgba(26,134,164,.28),
      transparent 32%
    ),

    radial-gradient(
      circle at 10% 80%,
      rgba(125,74,43,.16),
      transparent 30%
    ),

    #090c11;
}

.hero-grid {
  position: absolute;
  inset: 0;

  opacity: .15;

  background-image:
    linear-gradient(
      rgba(255,255,255,.08)
      1px,
      transparent 1px
    ),

    linear-gradient(
      90deg,
      rgba(255,255,255,.08)
      1px,
      transparent 1px
    );

  background-size:
    72px 72px;

  mask-image:
    linear-gradient(
      to bottom,
      black,
      transparent 90%
    );
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

  background:
    rgba(37,198,239,.12);
}

.glow-two {
  width: 300px;
  height: 300px;

  left: 10%;
  bottom: 30px;

  background:
    rgba(230,112,56,.07);
}

.hero-content {
  position: relative;
  z-index: 2;

  max-width: 1180px;

  min-height: 790px;

  margin: auto;

  padding:
    70px 24px;

  display: grid;

  grid-template-columns:
    420px 1fr;

  align-items: center;

  gap: 90px;
}


/* =========================
   FORM
========================= */

.lead-card {
  position: relative;

  background: #f8f9fb;

  color: #111722;

  border:
    2px solid var(--cyan);

  border-radius: 20px;

  padding: 31px;

  box-shadow:
    0 0 0 5px
      rgba(55,201,245,.05),

    0 25px 80px
      rgba(0,0,0,.45),

    0 0 50px
      rgba(55,201,245,.13);
}

.card-badge {
  position: absolute;

  top: -16px;
  right: 25px;

  background:
    var(--cyan);

  color: #061018;

  padding:
    7px 14px;

  border-radius:
    999px;

  font-size: 11px;
  font-weight: 900;
}

.card-badge span {
  display: inline-block;

  width: 7px;
  height: 7px;

  margin-left: 6px;

  background:
    #063d4c;

  border-radius: 50%;
}

.lead-card h2 {
  margin:
    10px 0 8px;

  font-size: 27px;

  line-height: 1.15;

  font-weight: 900;
}

.lead-card h2 strong {
  display: block;

  color:
    #1599bf;

  margin-top: 5px;
}

.form-subtitle {
  color: #687181;

  font-size: 13px;

  line-height: 1.7;

  margin:
    0 0 12px;
}

.free-highlight {
  padding:
    11px 13px;

  border:
    1px solid #b9ebf7;

  background:
    #ecfbff;

  color:
    #117e9f;

  border-radius:
    10px;

  font-size: 12px;

  font-weight: 800;

  line-height: 1.5;
}

.free-highlight span {
  margin-left: 5px;
}

.progress {
  height: 5px;

  background:
    #e1e5ea;

  border-radius:
    10px;

  margin:
    15px 0;
}

.progress span {
  display: block;

  width: 35%;
  height: 100%;

  border-radius:
    inherit;

  background:
    var(--cyan);
}

.lead-card label:not(.check) {
  display: block;

  margin:
    11px 0 6px;

  font-size: 12px;

  font-weight: 900;
}

.lead-card input[type="text"],
.lead-card input[type="tel"],
.lead-card select {
  width: 100%;

  border:
    1px solid #dbe0e7;

  border-radius:
    10px;

  padding:
    12px 13px;

  background:
    #f3f5f8;

  color:
    #17202c;

  outline: none;

  font-size: 14px;

  transition: .2s;
}

.lead-card input:focus,
.lead-card select:focus {
  border-color:
    var(--cyan);

  background:
    white;

  box-shadow:
    0 0 0 3px
    rgba(55,201,245,.12);
}

.check {
  display: flex;

  align-items: flex-start;

  gap: 8px;

  margin-top: 13px;

  color:
    #697382;

  font-size: 10px;

  line-height: 1.5;
}

.check input {
  margin-top: 3px;
}

.submit-button {
  width: 100%;

  margin-top: 15px;

  padding: 14px;

  border: 0;

  border-radius: 10px;

  background:
    #28afd6;

  color:
    #061018;

  font-size: 14px;

  font-weight: 900;

  cursor: pointer;

  transition: .3s;
}

.submit-button:hover {
  background:
    #42c9ed;

  box-shadow:
    0 10px 30px
    rgba(40,175,214,.25);
}

.submit-button span {
  margin-right: 7px;
}

.security {
  margin-top: 15px;

  padding-top: 13px;

  border-top:
    1px dashed #d5dae0;

  display: flex;

  justify-content: center;

  gap: 7px;

  color:
    #727b88;

  font-size: 10px;
}


/* =========================
   HERO TEXT
========================= */

.hero-text {
  text-align: right;
}

.live-pill {
  display: inline-flex;

  align-items: center;

  gap: 8px;

  padding:
    7px 13px;

  border:
    1px solid
    rgba(55,201,245,.25);

  background:
    rgba(55,201,245,.06);

  border-radius:
    999px;

  color:
    #8da5b1;

  font-size: 11px;
}

.live-dot {
  width: 7px;
  height: 7px;

  background:
    var(--cyan);

  border-radius:
    50%;

  box-shadow:
    0 0 12px
    var(--cyan);

  animation:
    pulse 1.8s infinite;
}

@keyframes pulse {
  50% {
    opacity: .35;
    transform: scale(.75);
  }
}

.eyebrow {
  color:
    var(--cyan);

  font-size: 12px;

  font-weight: 900;

  letter-spacing:
    1.5px;

  margin:
    0 0 15px;
}

.eyebrow span {
  margin-right: 7px;
}

.hero-text .eyebrow {
  margin-top: 30px;
}

.hero-text h1 {
  margin: 0;

  font-size:
    clamp(
      48px,
      5.1vw,
      78px
    );

  line-height: .98;

  letter-spacing: -2px;

  font-weight: 900;
}

.hero-text h1 span {
  color:
    var(--cyan);
}

.hero-text h1 strong {
  color: white;
}

.hero-description {
  max-width: 650px;

  margin:
    27px 0 0;

  color:
    #89919e;

  font-size: 17px;

  line-height: 1.85;
}

.hero-actions {
  margin-top: 31px;

  display: flex;

  gap: 11px;

  justify-content:
    flex-start;
}

.secondary-button {
  display: inline-flex;

  align-items: center;

  padding:
    12px 22px;

  border:
    1px solid
    rgba(255,255,255,.15);

  border-radius:
    999px;

  color:
    #b2bac5;

  transition:
    .3s;
}

.secondary-button:hover {
  border-color:
    var(--cyan);

  color:
    var(--cyan);
}

.trust-row {
  margin-top: 24px;

  display: flex;

  gap: 23px;

  color:
    #69727e;

  font-size: 11px;
}


/* =========================
   SECTIONS
========================= */

.section {
  padding:
    120px 24px;

  position: relative;
}

.dark-section {
  background:
    #0b0e14;

  border-top:
    1px solid
    rgba(255,255,255,.05);
}

.section-heading {
  max-width: 900px;

  margin:
    0 auto 58px;

  text-align:
    center;
}

.section-heading h2 {
  margin: 0;

  font-size:
    clamp(
      42px,
      5vw,
      66px
    );

  line-height: 1;

  font-weight:
    900;

  letter-spacing:
    -2px;
}

.section-heading h2 span {
  color:
    var(--cyan);
}

.section-heading p:last-child {
  max-width: 620px;

  margin:
    21px auto 0;

  color:
    #7f8793;

  line-height:
    1.8;
}

.right-heading {
  text-align:
    right;
}


/* =========================
   STEPS
========================= */

.steps {
  max-width: 1050px;

  margin:
    auto;

  display:
    grid;

  grid-template-columns:
    repeat(3, 1fr);

  gap: 26px;
}

.step {
  position: relative;

  min-height: 210px;

  padding:
    30px;

  background:
    #171a20;

  border:
    1px solid
    var(--border);

  border-radius:
    17px;

  transition:
    .35s;
}

.step:hover {
  transform:
    translateY(-7px);

  border-color:
    rgba(55,201,245,.5);

  box-shadow:
    0 20px 45px
    rgba(0,0,0,.3);
}

.active-step {
  border-color:
    var(--cyan);

  box-shadow:
    0 15px 45px
    rgba(55,201,245,.09);
}

.step-number {
  color:
    var(--cyan);

  font-size:
    37px;

  font-weight:
    900;
}

.step-arrow {
  position: absolute;

  left: 25px;
  top: 35px;

  color:
    var(--cyan);

  font-size:
    27px;

  opacity:
    .65;
}

.step h3 {
  font-size:
    20px;

  margin:
    25px 0 8px;
}

.step p {
  color:
    #777f8c;

  line-height:
    1.7;

  margin: 0;
}


/* =========================
   SERVICES
========================= */

.services-section {
  background:
    radial-gradient(
      circle at 80% 20%,
      rgba(30,154,193,.08),
      transparent 30%
    ),

    #0b0e14;
}

.services-grid {
  max-width: 1050px;

  margin:
    auto;

  display:
    grid;

  grid-template-columns:
    repeat(2, 1fr);

  gap: 16px;
}

.service-card {
  min-height:
    270px;

  position:
    relative;

  overflow:
    hidden;

  padding:
    30px;

  border:
    1px solid
    var(--border);

  border-radius:
    18px;

  background:
    #171a20;

  transition:
    .4s;
}

.service-card:hover {
  transform:
    translateY(-7px)
    scale(1.015);

  border-color:
    rgba(55,201,245,.65);

  box-shadow:
    0 0 40px
    rgba(55,201,245,.08);
}

.service-card.featured {
  background:
    #1d2229;

  border-color:
    rgba(55,201,245,.45);
}

.service-icon {
  width: 48px;
  height: 48px;

  border-radius:
    12px;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  background:
    rgba(55,201,245,.13);

  color:
    var(--cyan);

  font-size:
    22px;

  font-weight:
    900;
}

.service-number {
  position:
    absolute;

  left:
    25px;

  top:
    20px;

  color:
    rgba(255,255,255,.05);

  font-size:
    65px;

  font-weight:
    900;
}

.service-card h3 {
  margin:
    32px 0 7px;

  font-size:
    23px;
}

.service-card p {
  max-width:
    500px;

  color:
    #858d99;

  line-height:
    1.8;
}

.service-card a {
  display:
    inline-block;

  margin-top:
    15px;

  color:
    var(--cyan);

  font-weight:
    800;
}

.service-card a span {
  margin-right:
    8px;
}


/* =========================
   COMPANY SLIDER
========================= */

.company-section {
  overflow:
    hidden;

  background:
    radial-gradient(
      circle at 50% 60%,
      rgba(55,201,245,.05),
      transparent 35%
    ),

    #0b0e14;
}

.company-slider {
  position:
    relative;

  max-width:
    1100px;

  margin:
    auto;

  display:
    flex;

  align-items:
    center;
}

.company-track {
  width:
    100%;

  display:
    grid;

  grid-template-columns:
    repeat(3, 1fr);

  gap:
    25px;
}

.company-card {
  min-height:
    280px;

  padding:
    28px;

  background:
    #171a20;

  border:
    1px solid
    rgba(255,255,255,.09);

  border-radius:
    18px;

  transition:
    transform .5s
      cubic-bezier(.2,.8,.2,1),

    opacity .5s,

    box-shadow .5s,

    border-color .5s;

  cursor:
    pointer;
}

.company-card.dim {
  opacity:
    .55;

  transform:
    scale(.91);
}

.company-card.center {
  opacity:
    1;

  transform:
    scale(1.03);

  border-color:
    var(--cyan);

  box-shadow:
    0 0 0 1px
      rgba(55,201,245,.15),

    0 20px 70px
      rgba(55,201,245,.12);
}

.company-card.hovered {
  transform:
    scale(1.075)
    translateY(-8px);

  z-index:
    5;

  opacity:
    1;

  border-color:
    var(--cyan);

  box-shadow:
    0 0 35px
      rgba(55,201,245,.16),

    0 25px 60px
      rgba(0,0,0,.45);
}

.company-card.dim.hovered {
  transform:
    scale(1.03)
    translateY(-6px);
}

.company-top {
  display:
    flex;

  align-items:
    center;

  justify-content:
    space-between;
}

.company-icon {
  width:
    48px;

  height:
    48px;

  border-radius:
    12px;

  background:
    #203e48;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  color:
    var(--cyan);

  font-size:
    22px;
}

.company-type {
  color:
    #68727f;

  font-size:
    11px;
}

.company-card h3 {
  margin:
    50px 0 8px;

  font-size:
    27px;

  font-weight:
    900;
}

.company-card p {
  color:
    #858e9a;

  line-height:
    1.7;

  min-height:
    55px;
}

.company-bottom {
  margin-top:
    20px;

  padding-top:
    15px;

  border-top:
    1px dashed
    rgba(255,255,255,.1);

  display:
    flex;

  justify-content:
    space-between;

  color:
    #67717e;

  font-size:
    12px;
}

.company-bottom b {
  color:
    var(--cyan);

  font-size:
    20px;
}

.slider-arrow {
  position:
    absolute;

  z-index:
    10;

  width:
    56px;

  height:
    56px;

  border:
    1px solid
    rgba(55,201,245,.35);

  border-radius:
    50%;

  background:
    #111820;

  color:
    var(--cyan);

  font-size:
    25px;

  cursor:
    pointer;

  box-shadow:
    0 0 25px
    rgba(55,201,245,.1);

  transition:
    .3s;
}

.slider-arrow:hover {
  transform:
    scale(1.15);

  background:
    var(--cyan);

  color:
    #061018;

  box-shadow:
    0 0 35px
    rgba(55,201,245,.4);
}

.arrow-right {
  right:
    -28px;
}

.arrow-left {
  left:
    -28px;
}

.slider-dots {
  margin-top:
    35px;

  display:
    flex;

  justify-content:
    center;

  gap:
    8px;
}

.slider-dots button {
  width:
    7px;

  height:
    7px;

  padding:
    0;

  border:
    0;

  border-radius:
    50%;

  background:
    #3c444e;

  cursor:
    pointer;

  transition:
    .3s;
}

.slider-dots button.selected {
  width:
    24px;

  border-radius:
    20px;

  background:
    var(--cyan);

  box-shadow:
    0 0 12px
    rgba(55,201,245,.5);
}


/* =========================
   CTA
========================= */

.cta-section {
  position:
    relative;

  overflow:
    hidden;

  padding:
    130px 24px;

  text-align:
    center;

  background:
    radial-gradient(
      circle at 50% 100%,
      rgba(55,201,245,.15),
      transparent 45%
    ),

    #080b10;
}

.cta-glow {
  position:
    absolute;

  width:
    400px;

  height:
    400px;

  left:
    50%;

  top:
    40%;

  transform:
    translate(-50%,-50%);

  border-radius:
    50%;

  background:
    rgba(55,201,245,.06);

  filter:
    blur(70px);
}

.cta-content {
  position:
    relative;

  z-index:
    2;
}

.cta-content h2 {
  margin:
    0;

  font-size:
    clamp(
      45px,
      6vw,
      76px
    );

  line-height:
    1;
}

.cta-content h2 span {
  color:
    var(--cyan);
}

.cta-content > p:not(.eyebrow) {
  color:
    #858e9a;

  font-size:
    17px;

  margin:
    25px auto 35px;
}


/* =========================
   FAQ
========================= */

.faq-section {
  background:
    #f4f6f8;

  color:
    #10151d;
}

.faq-section .eyebrow {
  color:
    #1195ba;
}

.faq-section
.section-heading h2 {
  color:
    #10151d;
}

.faq-section
.section-heading h2 span {
  color:
    #119bc1;
}

.faq-list {
  max-width:
    850px;

  margin:
    auto;
}

.faq-item {
  margin-bottom:
    10px;

  background:
    white;

  border:
    1px solid
    #e0e4e8;

  border-radius:
    14px;

  overflow:
    hidden;
}

.faq-item button {
  width:
    100%;

  padding:
    23px 25px;

  display:
    flex;

  justify-content:
    space-between;

  align-items:
    center;

  border:
    0;

  background:
    white;

  text-align:
    inherit;

  cursor:
    pointer;

  color:
    #151b24;

  font-size:
    16px;

  font-weight:
    800;
}

.faq-item button b {
  width:
    32px;

  height:
    32px;

  flex-shrink:
    0;

  border-radius:
    50%;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  background:
    #eaf8fc;

  color:
    #159cc3;

  font-size:
    20px;
}

.faq-answer {
  padding:
    0 25px 23px;

  color:
    #68727e;

  line-height:
    1.8;
}


/* =========================
   FOOTER
========================= */

footer {
  background:
    #070a0f;

  padding:
    45px 24px 30px;

  border-top:
    1px solid
    var(--border);
}

.footer-main {
  max-width:
    1100px;

  margin:
    auto;

  display:
    flex;

  justify-content:
    space-between;

  align-items:
    center;
}

.footer-links {
  display:
    flex;

  gap:
    25px;

  color:
    #69727e;

  font-size:
    12px;
}

.footer-links a:hover {
  color:
    var(--cyan);
}

.footer-disclaimer,
.footer-copy {
  max-width:
    1100px;

  margin:
    30px auto 0;

  color:
    #4f5864;

  font-size:
    10px;

  line-height:
    1.7;
}

.footer-copy {
  margin-top:
    15px;
}


/* =========================
   FLOATING CTA
========================= */

.floating-cta {
  position:
    fixed;

  z-index:
    90;

  bottom:
    22px;

  left:
    18px;

  padding:
    11px 19px;

  border-radius:
    999px;

  background:
    var(--cyan);

  color:
    #061018;

  font-size:
    12px;

  font-weight:
    900;

  box-shadow:
    0 8px 25px
    rgba(0,0,0,.35);
}


/* =========================
   HIGH CONTRAST
========================= */

.high-contrast {
  --cyan:
    #00e5ff;

  --cyan-light:
    #66efff;
}

.high-contrast .hero,
.high-contrast .dark-section,
.high-contrast footer {
  background:
    #000;
}

.high-contrast
.company-card,
.high-contrast
.service-card,
.high-contrast
.step {
  border-color:
    rgba(255,255,255,.4);
}


/* =========================
   MOBILE
========================= */

@media (max-width: 1050px) {

  .desktop-nav {
    display:
      none;
  }

  .header-actions
  .nav-button {
    display:
      none;
  }

  .mobile-menu {
    display:
      block;
  }

  .header-actions {
    margin-right:
      auto;
  }

  .mobile-nav {
    padding:
      18px 24px 25px;

    display:
      flex;

    flex-direction:
      column;

    gap:
      17px;

    border-top:
      1px solid
      var(--border);

    background:
      #080b10;
  }

  .mobile-nav a {
    color:
      #b8bec8;

    font-weight:
      700;
  }

  .mobile-languages {
    display:
      flex;

    gap:
      7px;

    flex-wrap:
      wrap;
  }

  .mobile-languages button {
    padding:
      10px 13px;

    border:
      1px solid
      var(--border);

    border-radius:
      9px;

    background:
      #11161d;

    color:
      #b8bec8;

    cursor:
      pointer;

    font-weight:
      800;
  }

  .mobile-languages
  button.selected {
    background:
      var(--cyan);

    color:
      #061018;
  }

  .mobile-access {
    display:
      flex;

    gap:
      7px;
  }

  .mobile-access button {
    width:
      45px;

    height:
      40px;

    border:
      1px solid
      var(--border);

    border-radius:
      8px;

    background:
      #11161d;

    color:
      white;

    font-weight:
      900;

    cursor:
      pointer;
  }

  .hero-content {
    grid-template-columns:
      420px 1fr;

    gap:
      45px;
  }
}


@media (max-width: 850px) {

  .nav {
    height:
      68px;

    padding:
      0 18px;
  }

  .header-actions {
    display:
      none;
  }

  .ticker {
    margin-top:
      68px;
  }

  .hero {
    min-height:
      auto;
  }

  .hero-content {
    min-height:
      auto;

    grid-template-columns:
      1fr;

    gap:
      55px;

    padding:
      75px 18px 80px;
  }

  .hero-text {
    order:
      -1;
  }

  .hero-text h1 {
    font-size:
      48px;
  }

  .lead-card {
    width:
      100%;

    max-width:
      520px;

    margin:
      auto;
  }

  .steps {
    grid-template-columns:
      1fr;
  }

  .services-grid {
    grid-template-columns:
      1fr;
  }

  .company-track {
    grid-template-columns:
      1fr;
  }

  .company-card.dim {
    display:
      none;
  }

  .company-card.center {
    transform:
      scale(1);
  }

  .arrow-right {
    right:
      -10px;
  }

  .arrow-left {
    left:
      -10px;
  }

  .footer-main {
    flex-direction:
      column;

    align-items:
      flex-start;

    gap:
      25px;
  }

  .footer-links {
    flex-wrap:
      wrap;
  }
}


@media (max-width: 520px) {

  .section {
    padding:
      90px 18px;
  }

  .hero-text h1 {
    font-size:
      42px;

    letter-spacing:
      -1px;
  }

  .hero-description {
    font-size:
      15px;
  }

  .hero-actions {
    flex-direction:
      column;
  }

  .primary-button,
  .secondary-button {
    width:
      100%;

    text-align:
      center;
  }

  .trust-row {
    flex-wrap:
      wrap;

    gap:
      10px 18px;
  }

  .lead-card {
    padding:
      26px 19px;
  }

  .section-heading h2 {
    font-size:
      42px;
  }

  .company-card {
    min-height:
      300px;
  }

  .floating-cta {
    bottom:
      14px;

    left:
      14px;

    font-size:
      11px;
  }
}


@media (prefers-reduced-motion: reduce) {

  *,
  *::before,
  *::after {
    scroll-behavior:
      auto !important;

    animation-duration:
      .01ms !important;

    animation-iteration-count:
      1 !important;

    transition-duration:
      .01ms !important;
  }
}
</style>
