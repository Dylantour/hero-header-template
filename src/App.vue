<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const lang = ref(localStorage.getItem('pensara-lang') || 'he')
const textScale = ref(1)
const highContrast = ref(false)
const menuOpen = ref(false)
const faqOpen = ref(null)
const activeSlide = ref(0)
const hoveredSlide = ref(null)

let timer = null

const content = {
  he: {
    dir: 'rtl',
    name: 'פנסרה',
    subtitle: 'פתרונות פיננסיים',

    freeBadge: 'בדיקה ללא עלות וללא התחייבות',
    freeMain: 'בדיקה ללא עלות',
    freeSub: 'ללא התחייבות',

    heroEyebrow: 'פנסרה',
    heroTitle1: 'יש לכם כסף',
    heroTitle2: 'בקופות?',
    heroTitle3: 'בואו נבדוק',
    heroTitle4: 'מה אפשר לעשות איתו.',

    heroText:
      'קרנות השתלמות, קופות גמל, חיסכון פנסיוני והלוואות כנגד קופות — אנחנו עוזרים לכם להבין מה יש לכם ומה האפשרויות שניתן לבדוק.',

    start: 'התחילו את הבדיקה',
    how: 'איך זה עובד?',

    simple: '✓ תהליך פשוט',
    personal: '✓ יחס אישי',
    clear: '✓ מידע ברור',

    formTitle: 'מתחילים בבדיקה',
    formStrong: 'בדיקה ללא עלות וללא התחייבות.',
    formText:
      'הבדיקה וההדרכה על הכספים שאיתרנו — לגמרי בחינם.',

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

    submit: 'שלחו לי את הבדיקה',

    secure: '🔒 פרטים מאובטחים',
    noCommit: 'ללא התחייבות',

    servicesEyebrow: 'מה אפשר לבדוק?',
    servicesTitle: 'הכסף שלכם.',
    servicesTitle2: 'רק צריך להבין מה האפשרויות.',

    howEyebrow: 'איך זה עובד?',
    howTitle: '3 שלבים.',
    howTitle2: 'בלי כאב ראש.',
    howText:
      'מתחילים בבדיקה. מבינים את הנתונים. ורק אז בוחנים את האפשרויות.',

    companiesEyebrow: 'גופים פיננסיים',
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

    step1Title: 'משאירים פרטים',
    step1Text:
      'שם, טלפון וכמה פרטים בסיסיים. זה הכול.',

    step2Title: 'בודקים את האפשרויות',
    step2Text:
      'בוחנים את הנתונים ומבינים אילו אפשרויות ניתן לבדוק.',

    step3Title: 'מבינים ומחליטים',
    step3Text:
      'מסבירים את האפשרויות ואת המשמעות שלהן.',

    companyTypeInstitutional: 'גוף פיננסי',
    companyTypeInvestment: 'בית השקעות',

    companyDescription:
      'בחינת אפשרויות בהתאם לנתונים הרלוונטיים.',

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
        'לא. הבדיקה הראשונית אינה מחייבת אתכם להמשיך.'
      ],
      [
        'כמה מס משלמים על משיכה?',
        'המס, אם חל, משתנה בהתאם למוצר, לסוג הכספים ולנסיבות. לכן אין סכום אחיד שמתאים לכל לקוח.'
      ],
      [
        'כמה זמן זה לוקח?',
        'מילוי הטופס — פחות מדקה. נציג יחזור תוך 24 שעות (ימים א׳-ה׳, 9:00-18:00). משיכת הכסף — בדרך כלל 7-14 ימי עסקים.'
      ],
      [
        'איך זה יכול להיות בחינם?',
        'הבדיקה היא חינמית לגמרי — הבדיקה וההדרכה על הכספים שאיתרנו — לגמרי חינם. במקרים שנדרש יועץ להשלמת משיכה, נציג יציג לכם הצעה ותחליטו אם להמשיך.'
      ]
    ],

    disclaimer:
      'המידע באתר הינו כללי ואינו מהווה ייעוץ פנסיוני, ייעוץ השקעות, התחייבות לקבלת הלוואה או התחייבות לביצוע משיכה. האפשרויות והתנאים כפופים לנתונים האישיים ולתנאי הגופים הרלוונטיים.'
  },

  ru: {
    dir: 'ltr',
    name: 'Пенсера',
    subtitle: 'Финансовые решения',

    freeBadge: 'Проверка бесплатно и без обязательств',
    freeMain: 'Проверка бесплатно',
    freeSub: 'Без обязательств',

    heroEyebrow: 'Пенсера',
    heroTitle1: 'У вас есть деньги',
    heroTitle2: 'в накоплениях?',
    heroTitle3: 'Давайте посмотрим',
    heroTitle4: 'что можно сделать.',

    heroText:
      'Купат Гемель, Керен Иштальмут, пенсионные накопления и кредиты под накопления — поможем разобраться, что у вас есть и какие варианты можно проверить.',

    start: 'Начать проверку',
    how: 'Как это работает?',

    simple: '✓ Всё просто',
    personal: '✓ Личное отношение',
    clear: '✓ Понятно объясняем',

    formTitle: 'Начинаем проверку',
    formStrong: 'Проверка бесплатно и без обязательств.',
    formText:
      'Проверка и объяснение найденных денег — полностью бесплатно.',

    name: 'Полное имя *',
    namePlaceholder: 'Например: Иван Иванов',

    phone: 'Номер телефона *',
    phonePlaceholder: '050-0000000',

    interest: 'Что вас интересует?',
    choose: 'Выберите вариант',

    withdrawal: 'Вывод денег',
    training: 'Керен Иштальмут',
    provident: 'Купат Гемель',
    loan: 'Кредит под накопления',
    unsure: 'Я не уверен/а',

    consent:
      'Я разрешаю связаться со мной в соответствии с политикой конфиденциальности.',

    submit: 'Отправить данные',

    secure: '🔒 Данные защищены',
    noCommit: 'Без обязательств',

    servicesEyebrow: 'Что можно проверить?',
    servicesTitle: 'Ваши деньги.',
    servicesTitle2: 'Нужно только понять варианты.',

    howEyebrow: 'Как это работает?',
    howTitle: '3 шага.',
    howTitle2: 'Всё просто.',
    howText:
      'Сначала проверяем. Потом объясняем. И только после этого смотрим варианты.',

    companiesEyebrow: 'Финансовые организации',
    companiesTitle: 'Проверяем',
    companiesTitle2: 'варианты.',
    companiesText:
      'При необходимости можно проверить данные по накоплениям, которые находятся в разных финансовых организациях.',

    faqEyebrow: 'Частые вопросы',
    faqTitle: 'Есть вопрос?',
    faqTitle2: 'Вот ответ.',

    ctaEyebrow: 'Не знаете, с чего начать?',
    ctaTitle: 'Не нужно гадать.',
    ctaTitle2: 'Начните с проверки.',

    step1Title: 'Оставляете данные',
    step1Text:
      'Имя, телефон и несколько простых данных.',

    step2Title: 'Проверяем варианты',
    step2Text:
      'Смотрим данные и понимаем, какие варианты можно проверить.',

    step3Title: 'Понимаете и решаете',
    step3Text:
      'Объясняем всё простыми словами.',

    companyTypeInstitutional: 'Финансовая организация',
    companyTypeInvestment: 'Инвестиционный дом',

    companyDescription:
      'Проверка вариантов в соответствии с актуальными данными.',

    faqs: [
      [
        'Можно ли вывести деньги из Купат Гемель или Керен Иштальмут?',
        'Это зависит от типа продукта, даты взносов, вида денег и вашей ситуации. Сначала мы проверяем и объясняем варианты.'
      ],
      [
        'Всегда ли лучше выводить деньги?',
        'Нет. Иногда можно рассмотреть другие варианты, например кредит под накопления.'
      ],
      [
        'Обязывает ли меня первичная проверка?',
        'Нет. Первичная проверка ни к чему вас не обязывает.'
      ],
      [
        'Сколько времени занимает процесс?',
        'Заполнение формы — меньше минуты. Представитель свяжется в течение 24 часов (вс–чт, 9:00–18:00). Вывод денег — обычно 7–14 рабочих дней.'
      ],
      [
        'Почему проверка бесплатна?',
        'Проверка полностью бесплатна. Проверка и объяснение найденных денег — бесплатно. Если для завершения вывода понадобится консультант, представитель предложит условия, а вы решите, продолжать ли.'
      ]
    ],

    disclaimer:
      'Информация на сайте носит общий характер и не является пенсионной или инвестиционной консультацией, гарантией получения кредита или гарантией вывода средств. Возможности и условия зависят от личных данных и условий соответствующих организаций.'
  },

  ar: {
    dir: 'rtl',
    name: 'بنصرا',
    subtitle: 'حلول مالية',

    freeBadge: 'فحص مجاني وبدون التزام',
    freeMain: 'الفحص مجانًا',
    freeSub: 'بدون التزام',

    heroEyebrow: 'بنصرا',
    heroTitle1: 'عندكم أموال',
    heroTitle2: 'في الصناديق؟',
    heroTitle3: 'خلينا نشوف',
    heroTitle4: 'شو ممكن نعمل.',

    heroText:
      'صناديق الاستكمال، صناديق الادخار والتقاعد، وقروض مقابل المدخرات — بنساعدكم تفهموا شو عندكم وشو الخيارات الممكنة.',

    start: 'ابدأوا الفحص',
    how: 'كيف الموضوع بيشتغل؟',

    simple: '✓ الموضوع بسيط',
    personal: '✓ تعامل شخصي',
    clear: '✓ شرح واضح',

    formTitle: 'نبدأ بالفحص',
    formStrong: 'الفحص مجاني وبدون التزام.',
    formText:
      'الفحص والشرح عن الأموال اللي لقيناها — مجانًا بالكامل.',

    name: 'الاسم الكامل *',
    namePlaceholder: 'مثال: داني ليفي',

    phone: 'رقم الهاتف *',
    phonePlaceholder: '050-0000000',

    interest: 'شو اللي بهتمكم؟',
    choose: 'اختاروا خيار',

    withdrawal: 'سحب الأموال',
    training: 'صندوق استكمال',
    provident: 'صندوق ادخار',
    loan: 'قرض مقابل الصندوق',
    unsure: 'مش متأكد/ة',

    consent:
      'أوافق على التواصل معي حسب سياسة الخصوصية.',

    submit: 'إرسال التفاصيل',

    secure: '🔒 بيانات آمنة',
    noCommit: 'بدون التزام',

    servicesEyebrow: 'شو ممكن نفحص؟',
    servicesTitle: 'أموالكم.',
    servicesTitle2: 'بس لازم نفهم شو الخيارات.',

    howEyebrow: 'كيف الموضوع بيشتغل؟',
    howTitle: '3 خطوات.',
    howTitle2: 'بدون تعقيد.',
    howText:
      'بنبدأ بالفحص. بنفهم البيانات. وبعدها بنشوف الخيارات.',

    companiesEyebrow: 'جهات مالية',
    companiesTitle: 'نفحص',
    companiesTitle2: 'الخيارات.',
    companiesText:
      'عند الحاجة، ممكن نفحص بيانات مرتبطة بالصناديق والمدخرات الموجودة في جهات مالية مختلفة.',

    faqEyebrow: 'أسئلة شائعة',
    faqTitle: 'عندكم سؤال؟',
    faqTitle2: 'هاي الإجابة.',

    ctaEyebrow: 'مش عارفين من وين تبدأوا؟',
    ctaTitle: 'ما في داعي للتخمين.',
    ctaTitle2: 'ابدأوا بالفحص.',

    step1Title: 'بتتركوا التفاصيل',
    step1Text:
      'الاسم، رقم الهاتف وبعض التفاصيل البسيطة.',

    step2Title: 'بنفحص الخيارات',
    step2Text:
      'بنشوف البيانات وبنفهم شو ممكن نفحص.',

    step3Title: 'بتفهموا وبتقرروا',
    step3Text:
      'بنشرحلكم الخيارات بطريقة بسيطة.',

    companyTypeInstitutional: 'جهة مالية',
    companyTypeInvestment: 'بيت استثمار',

    companyDescription:
      'فحص الخيارات حسب البيانات ذات الصلة.',

    faqs: [
      [
        'ممكن نسحب مصاري من صندوق ادخار أو استكمال؟',
        'هذا بيعتمد على نوع الصندوق، تاريخ الإيداعات، نوع الأموال والظروف الشخصية. بنبدأ بالفحص وبنشرح شو ممكن نعمل.'
      ],
      [
        'هل دايمًا الأفضل نسحب المصاري؟',
        'مش بالضرورة. أحيانًا ممكن نفحص بدائل، مثل قرض مقابل المدخرات.'
      ],
      [
        'هل الفحص الأولي بيلزمني بشي؟',
        'لا. الفحص الأولي ما بلزمكم بأي شيء.'
      ],
      [
        'قديش بياخد الموضوع وقت؟',
        'تعبئة النموذج — أقل من دقيقة. ممثل برجعلكم خلال 24 ساعة (الأحد–الخميس، 9:00–18:00). سحب الأموال — عادةً 7–14 يوم عمل.'
      ],
      [
        'كيف الفحص ممكن يكون مجاني؟',
        'الفحص مجاني بالكامل. الفحص والشرح عن الأموال اللي لقيناها — مجانًا. إذا احتجنا مستشار لإكمال عملية السحب، الممثل بعرض عليكم السعر والشروط وأنتم بتقرروا إذا تكملوا.'
      ]
    ],

    disclaimer:
      'المعلومات في الموقع عامة ولا تشكل استشارة تقاعدية أو استثمارية أو التزامًا بالحصول على قرض أو تنفيذ سحب. الخيارات والشروط تعتمد على البيانات الشخصية وشروط الجهات ذات الصلة.'
  }
}

const current = computed(() => content[lang.value])

const services = computed(() => [
  {
    title: current.value.withdrawal,
    icon: '₪',
    text:
      lang.value === 'he'
        ? 'בדיקה של אפשרויות המשיכה בהתאם לסוג הקופה והכספים.'
        : lang.value === 'ru'
          ? 'Проверяем варианты вывода денег в зависимости от продукта.'
          : 'بنفحص خيارات سحب الأموال حسب نوع الصندوق والأموال.'
  },
  {
    title: current.value.training,
    icon: '↗',
    text:
      lang.value === 'he'
        ? 'מבינים את מצב הקרן ובוחנים אילו אפשרויות קיימות.'
        : lang.value === 'ru'
          ? 'Разбираемся в состоянии накоплений и доступных вариантах.'
          : 'بنفهم وضع الصندوق وبنشوف شو الخيارات الموجودة.'
  },
  {
    title: current.value.provident,
    icon: '◇',
    text:
      lang.value === 'he'
        ? 'בדיקה של הכספים והאפשרויות שניתן לבחון.'
        : lang.value === 'ru'
          ? 'Проверяем деньги и варианты, которые можно рассмотреть.'
          : 'بنفحص الأموال والخيارات الممكنة.'
  },
  {
    title: current.value.loan,
    icon: '+',
    text:
      lang.value === 'he'
        ? 'בחינת אפשרות לקבלת הלוואה כנגד חיסכון קיים, בכפוף לתנאים.'
        : lang.value === 'ru'
          ? 'Проверяем возможность кредита под существующие накопления.'
          : 'بنفحص إمكانية الحصول على قرض مقابل مدخرات موجودة.'
  }
])

const companies = [
  { name: 'הפניקס', type: 'institutional' },
  { name: 'הראל', type: 'institutional' },
  { name: 'מגדל', type: 'institutional' },
  { name: 'כלל', type: 'institutional' },
  { name: 'מנורה', type: 'institutional' },
  { name: 'מיטב', type: 'investment' }
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
      1.3,
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

function companyType(company) {
  return company.type === 'investment'
    ? current.value.companyTypeInvestment
    : current.value.companyTypeInstitutional
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
      'high-contrast':
        highContrast
    }"
    :style="{
      '--text-scale':
        textScale
    }"
  >

    <!-- HEADER -->

    <header class="header">

      <div class="nav">

        <a
          href="#"
          class="logo"
        >

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

          <!-- BIG LANGUAGE SWITCHER -->

          <div class="languages">

            <button
              :class="{
                selected:
                  lang === 'he'
              }"
              @click="setLang('he')"
            >
              🇮🇱
              <span>
                עברית
              </span>
            </button>


            <button
              :class="{
                selected:
                  lang === 'ru'
              }"
              @click="setLang('ru')"
            >
              🇷🇺
              <span>
                Русский
              </span>
            </button>


            <!-- NO FLAG FOR ARABIC -->

            <button
              :class="{
                selected:
                  lang === 'ar'
              }"
              @click="setLang('ar')"
            >
              <span>
                العربية
              </span>
            </button>

          </div>


          <button
            class="accessibility-top"
            @click="increaseText"
          >
            A+
          </button>

          <button
            class="accessibility-top"
            @click="decreaseText"
          >
            A−
          </button>

          <button
            class="accessibility-top"
            @click="
              highContrast =
                !highContrast
            "
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
          @click="
            menuOpen =
              !menuOpen
          "
        >
          {{
            menuOpen
              ? '×'
              : '☰'
          }}
        </button>

      </div>


      <!-- MOBILE MENU -->

      <div
        v-if="menuOpen"
        class="mobile-nav"
      >

        <div class="mobile-languages">

          <button
            :class="{
              selected:
                lang === 'he'
            }"
            @click="setLang('he')"
          >
            🇮🇱 עברית
          </button>

          <button
            :class="{
              selected:
                lang === 'ru'
            }"
            @click="setLang('ru')"
          >
            🇷🇺 Русский
          </button>

          <button
            :class="{
              selected:
                lang === 'ar'
            }"
            @click="setLang('ar')"
          >
            العربية
          </button>

        </div>


        <div class="mobile-access">

          <button
            @click="
              increaseText()
            "
          >
            A+
          </button>

          <button
            @click="
              decreaseText()
            "
          >
            A−
          </button>

          <button
            @click="
              highContrast =
                !highContrast
            "
          >
            ◐
          </button>

        </div>


        <a
          href="#services"
          @click="
            menuOpen = false
          "
        >
          {{ current.servicesEyebrow }}
        </a>

        <a
          href="#how"
          @click="
            menuOpen = false
          "
        >
          {{ current.how }}
        </a>

        <a
          href="#companies"
          @click="
            menuOpen = false
          "
        >
          {{ current.companiesEyebrow }}
        </a>

        <a
          href="#faq"
          @click="
            menuOpen = false
          "
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

          <!-- LARGE FREE MESSAGE -->

          <div class="free-banner">

            <div class="free-banner-icon">
              ✓
            </div>

            <div class="free-banner-text">

              <strong>
                {{ current.freeMain }}
              </strong>

              <span>
                {{ current.freeSub }}
              </span>

            </div>

          </div>


          <div class="form-header">

            <div class="form-step">
              01 / 03
            </div>

            <h2>
              {{ current.formTitle }}
            </h2>

            <p>
              {{ current.formStrong }}
            </p>

          </div>


          <div class="progress">
            <span></span>
          </div>


          <label>
            {{ current.name }}
          </label>

          <input
            type="text"
            :placeholder="
              current.namePlaceholder
            "
          />


          <label>
            {{ current.phone }}
          </label>

          <input
            type="tel"
            dir="ltr"
            :placeholder="
              current.phonePlaceholder
            "
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

            <span
              class="live-dot"
            ></span>

            <strong>
              {{ current.freeBadge }}
            </strong>

          </div>


          <p class="eyebrow">

            {{ current.heroEyebrow }}

            <span>
              »
            </span>

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


    <!-- HOW -->

    <section
      id="how"
      class="section dark-section"
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.howEyebrow }}

          <span>
            »
          </span>

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
            {{ current.step1Title }}
          </h3>

          <p>
            {{ current.step1Text }}
          </p>

        </div>


        <div
          class="
            step
            active-step
          "
        >

          <div class="step-number">
            02
          </div>

          <div class="step-arrow">
            «
          </div>

          <h3>
            {{ current.step2Title }}
          </h3>

          <p>
            {{ current.step2Text }}
          </p>

        </div>


        <div class="step">

          <div class="step-number">
            03
          </div>

          <h3>
            {{ current.step3Title }}
          </h3>

          <p>
            {{ current.step3Text }}
          </p>

        </div>

      </div>

    </section>


    <!-- SERVICES -->

    <section
      id="services"
      class="
        section
        dark-section
        services-section
      "
    >

      <div
        class="
          section-heading
          right-heading
        "
      >

        <p class="eyebrow">

          {{ current.servicesEyebrow }}

          <span>
            »
          </span>

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
          v-for="
            (service, index)
            in services
          "
          :key="
            service.title
          "
          class="service-card"
          :class="{
            featured:
              index === 0
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
      class="
        section
        dark-section
        company-section
      "
      @mouseenter="stopSlider"
      @mouseleave="startSlider"
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.companiesEyebrow }}

          <span>
            »
          </span>

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
          class="
            slider-arrow
            arrow-right
          "
          @click="prevSlide"
          aria-label="Previous"
        >
          «
        </button>


        <div class="company-track">

          <article
            v-for="
              company
              in visibleCompanies
            "
            :key="
              company.name
            "
            class="company-card"
            :class="{
              center:
                company.position === 0,

              dim:
                company.position !== 0,

              hovered:
                hoveredSlide ===
                company.name
            }"
            @mouseenter="
              hoveredSlide =
                company.name
            "
            @mouseleave="
              hoveredSlide = null
            "
            @click="
              activeSlide =
                companies.findIndex(
                  c =>
                    c.name ===
                    company.name
                )
            "
          >

            <div class="company-top">

              <div class="company-icon">
                ◈
              </div>

              <span class="company-type">
                {{ companyType(company) }}
              </span>

            </div>


            <h3>
              {{ company.name }}
            </h3>


            <p>
              {{ current.companyDescription }}
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
          class="
            slider-arrow
            arrow-left
          "
          @click="nextSlide"
          aria-label="Next"
        >
          »
        </button>

      </div>


      <div class="slider-dots">

        <button
          v-for="
            (_, index)
            in companies
          "
          :key="index"
          :class="{
            selected:
              index ===
              activeSlide
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

        <div class="cta-free">

          <span>
            ✓
          </span>

          {{ current.freeBadge }}

        </div>


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
          class="primary-button big-cta"
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
      class="
        section
        faq-section
      "
    >

      <div class="section-heading">

        <p class="eyebrow">

          {{ current.faqEyebrow }}

          <span>
            »
          </span>

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
          v-for="
            (faq, index)
            in current.faqs
          "
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

        {{ current.name }}

        .

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

      <span>
        ✓
      </span>

      {{ current.freeBadge }}

    </a>

  </div>

</template>


<style>

@import url(
  'https://fonts.googleapis.com/css2?family=Heebo:wght@400;500;600;700;800;900&family=Rubik:wght@400;500;600;700;800;900&display=swap'
);


:root {

  --bg:
    #0d1720;

  --bg-light:
    #12232e;

  --bg-card:
    #182b36;

  --bg-card-2:
    #1c3440;

  --cyan:
    #42d5f5;

  --cyan-light:
    #73e4fa;

  --text:
    #f7fbfd;

  --muted:
    #a8b7c1;

  --border:
    rgba(255,255,255,.13);

}


* {
  box-sizing:
    border-box;
}


html {
  scroll-behavior:
    smooth;
}


body {

  margin:
    0;

  background:
    var(--bg);

  font-family:
    'Heebo',
    'Rubik',
    Arial,
    sans-serif;

}


button,
input,
select {
  font-family:
    inherit;
}


a {

  color:
    inherit;

  text-decoration:
    none;

}


.site {

  min-height:
    100vh;

  color:
    var(--text);

  font-size:
    calc(
      16px *
      var(--text-scale)
    );

}


/* HEADER */

.header {

  position:
    fixed;

  inset:
    0 0 auto;

  z-index:
    100;

  background:
    rgba(13,23,32,.94);

  border-bottom:
    1px solid
    rgba(255,255,255,.1);

  backdrop-filter:
    blur(20px);

}


.nav {

  height:
    82px;

  max-width:
    1180px;

  margin:
    auto;

  padding:
    0 24px;

  display:
    flex;

  align-items:
    center;

  justify-content:
    space-between;

  gap:
    25px;

}


.logo {

  display:
    flex;

  align-items:
    center;

  gap:
    10px;

  flex-shrink:
    0;

}


.logo-mark {

  width:
    45px;

  height:
    45px;

  border-radius:
    13px;

  background:
    var(--cyan);

  color:
    #06141b;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  font-weight:
    900;

  font-size:
    22px;

}


.logo strong {

  display:
    block;

  font-size:
    21px;

}


.logo small {

  display:
    block;

  color:
    #8fa1ad;

  font-size:
    10px;

}


.desktop-nav {

  display:
    flex;

  align-items:
    center;

  gap:
    25px;

  color:
    #b2c0c8;

  font-size:
    14px;

  font-weight:
    700;

}


.desktop-nav a {

  transition:
    .25s;

}


.desktop-nav a:hover {

  color:
    var(--cyan);

}


/* LANGUAGES - BIGGER */

.header-actions {

  display:
    flex;

  align-items:
    center;

  gap:
    8px;

}


.languages {

  display:
    flex;

  align-items:
    center;

  padding:
    5px;

  gap:
    3px;

  border:
    1px solid
    rgba(255,255,255,.2);

  border-radius:
    14px;

  background:
    rgba(255,255,255,.07);

  box-shadow:
    0 5px 25px
    rgba(0,0,0,.18);

}


.languages button {

  min-height:
    43px;

  padding:
    8px 13px;

  border:
    0;

  background:
    transparent;

  color:
    #aebdc6;

  cursor:
    pointer;

  border-radius:
    10px;

  font-size:
    12px;

  font-weight:
    900;

  transition:
    .25s;

}


.languages button span {

  margin:
    0 3px;

}


.languages button:hover {

  background:
    rgba(66,213,245,.12);

  color:
    white;

}


.languages button.selected {

  background:
    var(--cyan);

  color:
    #06141b;

  box-shadow:
    0 5px 18px
    rgba(66,213,245,.25);

}


/* ACCESSIBILITY */

.accessibility-top {

  width:
    38px;

  height:
    38px;

  border:
    1px solid
    rgba(255,255,255,.16);

  border-radius:
    9px;

  background:
    #172832;

  color:
    #d6e1e7;

  cursor:
    pointer;

  font-weight:
    900;

}


.accessibility-top:hover {

  color:
    var(--cyan);

  border-color:
    var(--cyan);

}


/* BUTTONS */

.nav-button,
.primary-button {

  display:
    inline-flex;

  align-items:
    center;

  justify-content:
    center;

  background:
    var(--cyan);

  color:
    #06141b;

  font-weight:
    900;

  border-radius:
    999px;

  padding:
    13px 21px;

  transition:
    .3s;

  white-space:
    nowrap;

}


.nav-button:hover,
.primary-button:hover {

  background:
    var(--cyan-light);

  transform:
    translateY(-2px);

  box-shadow:
    0 12px 35px
    rgba(66,213,245,.25);

}


.nav-button span,
.primary-button span {

  margin-right:
    8px;

}


.mobile-menu {

  display:
    none;

  border:
    0;

  background:
    transparent;

  color:
    white;

  font-size:
    27px;

}


/* TICKER */

.ticker {

  position:
    relative;

  z-index:
    5;

  margin-top:
    82px;

  height:
    50px;

  overflow:
    hidden;

  background:
    #10212b;

  border-bottom:
    1px solid
    rgba(255,255,255,.08);

}


.ticker-track {

  width:
    max-content;

  height:
    100%;

  display:
    flex;

  align-items:
    center;

  gap:
    30px;

  color:
    #8fa3ae;

  font-size:
    13px;

  font-weight:
    800;

  animation:
    ticker
    28s linear infinite;

}


.ticker b {

  color:
    var(--cyan);

}


@keyframes ticker {

  from {
    transform:
      translateX(0);
  }

  to {
    transform:
      translateX(50%);
  }

}


/* HERO */

.hero {

  position:
    relative;

  min-height:
    790px;

  overflow:
    hidden;

  background:

    radial-gradient(
      circle at 75% 25%,
      rgba(49,190,226,.23),
      transparent 34%
    ),

    radial-gradient(
      circle at 10% 80%,
      rgba(70,125,155,.16),
      transparent 35%
    ),

    linear-gradient(
      135deg,
      #10212b,
      #0d1820 55%,
      #122832
    );

}


.hero-grid {

  position:
    absolute;

  inset:
    0;

  opacity:
    .14;

  background-image:

    linear-gradient(
      rgba(255,255,255,.09)
      1px,
      transparent 1px
    ),

    linear-gradient(
      90deg,
      rgba(255,255,255,.09)
      1px,
      transparent 1px
    );

  background-size:
    72px 72px;

}


.hero-glow {

  position:
    absolute;

  border-radius:
    50%;

  filter:
    blur(80px);

  pointer-events:
    none;

}


.glow-one {

  width:
    450px;

  height:
    450px;

  right:
    -120px;

  top:
    100px;

  background:
    rgba(45,205,241,.13);

}


.glow-two {

  width:
    350px;

  height:
    350px;

  left:
    10%;

  bottom:
    30px;

  background:
    rgba(84,147,177,.1);

}


.hero-content {

  position:
    relative;

  z-index:
    2;

  max-width:
    1180px;

  min-height:
    790px;

  margin:
    auto;

  padding:
    70px 24px;

  display:
    grid;

  grid-template-columns:
    420px 1fr;

  align-items:
    center;

  gap:
    90px;

}


/* FORM */

.lead-card {

  position:
    relative;

  background:
    #f8fbfc;

  color:
    #101a22;

  border:
    2px solid
    var(--cyan);

  border-radius:
    23px;

  padding:
    31px;

  box-shadow:

    0 0 0 6px
      rgba(66,213,245,.07),

    0 25px 80px
      rgba(0,0,0,.35),

    0 0 65px
      rgba(66,213,245,.14);

}


/* BIG FREE BANNER */

.free-banner {

  width:
    calc(100% + 20px);

  margin:
    -47px -10px 24px;

  min-height:
    96px;

  padding:
    17px 18px;

  display:
    flex;

  align-items:
    center;

  gap:
    14px;

  background:

    linear-gradient(
      135deg,
      #42d5f5,
      #1da9d0
    );

  color:
    #03141b;

  border-radius:
    16px;

  box-shadow:

    0 12px 30px
      rgba(55,201,245,.28),

    0 0 35px
      rgba(55,201,245,.18);

  position:
    relative;

  overflow:
    hidden;

}


.free-banner::after {

  content:
    '';

  position:
    absolute;

  width:
    140px;

  height:
    140px;

  right:
    -45px;

  top:
    -55px;

  border-radius:
    50%;

  background:
    rgba(255,255,255,.2);

}


.free-banner-icon {

  width:
    50px;

  height:
    50px;

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
    rgba(255,255,255,.35);

  font-size:
    25px;

  font-weight:
    900;

}


.free-banner-text {

  position:
    relative;

  z-index:
    2;

  display:
    flex;

  flex-direction:
    column;

}


.free-banner-text strong {

  font-size:
    25px;

  line-height:
    1;

  font-weight:
    900;

}


.free-banner-text span {

  margin-top:
    6px;

  font-size:
    13px;

  font-weight:
    800;

  opacity:
    .78;

}


/* FORM HEADER */

.form-header {

  position:
    relative;

}


.form-step {

  position:
    absolute;

  left:
    0;

  top:
    0;

  color:
    #8c98a2;

  font-size:
    10px;

  font-weight:
    800;

}


.form-header h2 {

  margin:
    0;

  font-size:
    28px;

  line-height:
    1.15;

  font-weight:
    900;

}


.form-header p {

  margin:
    7px 0 0;

  color:
    #078bad;

  font-size:
    15px;

  line-height:
    1.5;

  font-weight:
    900;

}


.progress {

  height:
    5px;

  background:
    #dce5e9;

  border-radius:
    10px;

  margin:
    18px 0;

}


.progress span {

  display:
    block;

  width:
    35%;

  height:
    100%;

  border-radius:
    inherit;

  background:
    var(--cyan);

}


/* FORM FIELDS */

.lead-card label:not(.check) {

  display:
    block;

  margin:
    12px 0 6px;

  font-size:
    13px;

  font-weight:
    900;

}


.lead-card input[type="text"],
.lead-card input[type="tel"],
.lead-card select {

  width:
    100%;

  border:
    1px solid
    #d2dde2;

  border-radius:
    10px;

  padding:
    13px;

  background:
    #f2f6f8;

  color:
    #17202c;

  outline:
    none;

  font-size:
    14px;

  transition:
    .2s;

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

  display:
    flex;

  align-items:
    flex-start;

  gap:
    8px;

  margin-top:
    13px;

  color:
    #64727d;

  font-size:
    10px;

  line-height:
    1.5;

}


.check input {

  margin-top:
    3px;

}


.submit-button {

  width:
    100%;

  margin-top:
    15px;

  padding:
    16px;

  border:
    0;

  border-radius:
    11px;

  background:
    #25afd3;

  color:
    #061018;

  font-size:
    16px;

  font-weight:
    900;

  cursor:
    pointer;

  transition:
    .3s;

}


.submit-button:hover {

  background:
    #42c9ed;

  box-shadow:
    0 10px 30px
    rgba(40,175,214,.25);

  transform:
    translateY(-2px);

}


.submit-button span {

  margin-right:
    7px;

}


.security {

  margin-top:
    15px;

  padding-top:
    13px;

  border-top:
    1px dashed
    #d5dfe4;

  display:
    flex;

  justify-content:
    center;

  gap:
    7px;

  color:
    #727f89;

  font-size:
    10px;

}


/* HERO TEXT */

.hero-text {

  text-align:
    right;

}


.live-pill {

  display:
    inline-flex;

  align-items:
    center;

  gap:
    8px;

  padding:
    10px 16px;

  border:
    1px solid
    rgba(66,213,245,.4);

  background:
    rgba(66,213,245,.1);

  border-radius:
    999px;

  color:
    #c2d8e1;

  font-size:
    13px;

}


.live-pill strong {

  color:
    var(--cyan);

}


.live-dot {

  width:
    8px;

  height:
    8px;

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

    opacity:
      .35;

    transform:
      scale(.75);

  }

}


.eyebrow {

  color:
    var(--cyan);

  font-size:
    12px;

  font-weight:
    900;

  letter-spacing:
    1.5px;

  margin:
    0 0 15px;

}


.eyebrow span {

  margin-right:
    7px;

}


.hero-text .eyebrow {

  margin-top:
    30px;

}


.hero-text h1 {

  margin:
    0;

  font-size:
    clamp(
      48px,
      5.1vw,
      78px
    );

  line-height:
    .98;

  letter-spacing:
    -2px;

  font-weight:
    900;

}


.hero-text h1 span {

  color:
    var(--cyan);

}


.hero-text h1 strong {

  color:
    white;

}


.hero-description {

  max-width:
    650px;

  margin:
    27px 0 0;

  color:
    #a6b5bf;

  font-size:
    17px;

  line-height:
    1.85;

}


.hero-actions {

  margin-top:
    31px;

  display:
    flex;

  gap:
    11px;

}


.secondary-button {

  display:
    inline-flex;

  align-items:
    center;

  padding:
    12px 22px;

  border:
    1px solid
    rgba(255,255,255,.18);

  border-radius:
    999px;

  color:
    #b9c6ce;

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

  margin-top:
    24px;

  display:
    flex;

  gap:
    23px;

  color:
    #81919c;

  font-size:
    11px;

}


/* SECTIONS */

.section {

  padding:
    120px 24px;

  position:
    relative;

}


.dark-section {

  background:
    linear-gradient(
      180deg,
      #10212b,
      #0e1c25
    );

  border-top:
    1px solid
    rgba(255,255,255,.06);

}


.section-heading {

  max-width:
    900px;

  margin:
    0 auto 58px;

  text-align:
    center;

}


.section-heading h2 {

  margin:
    0;

  font-size:
    clamp(
      42px,
      5vw,
      66px
    );

  line-height:
    1;

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

  max-width:
    620px;

  margin:
    21px auto 0;

  color:
    #91a2ad;

  line-height:
    1.8;

}


.right-heading {

  text-align:
    right;

}


/* STEPS */

.steps {

  max-width:
    1050px;

  margin:
    auto;

  display:
    grid;

  grid-template-columns:
    repeat(3, 1fr);

  gap:
    26px;

}


.step {

  position:
    relative;

  min-height:
    210px;

  padding:
    30px;

  background:
    #18303b;

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
    rgba(66,213,245,.5);

  box-shadow:
    0 20px 45px
    rgba(0,0,0,.22);

}


.active-step {

  border-color:
    var(--cyan);

  box-shadow:
    0 15px 45px
    rgba(55,201,245,.1);

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

  position:
    absolute;

  left:
    25px;

  top:
    35px;

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
    #91a2ad;

  line-height:
    1.7;

  margin:
    0;

}


/* SERVICES */

.services-section {

  background:

    radial-gradient(
      circle at 80% 20%,
      rgba(30,154,193,.12),
      transparent 30%
    ),

    #10212b;

}


.services-grid {

  max-width:
    1050px;

  margin:
    auto;

  display:
    grid;

  grid-template-columns:
    repeat(2, 1fr);

  gap:
    16px;

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
    #18303b;

  transition:
    .4s;

}


.service-card:hover {

  transform:
    translateY(-7px)
    scale(1.015);

  border-color:
    rgba(66,213,245,.65);

  box-shadow:
    0 0 40px
    rgba(66,213,245,.08);

}


.service-card.featured {

  background:
    #1d3946;

  border-color:
    rgba(66,213,245,.45);

}


.service-icon {

  width:
    50px;

  height:
    50px;

  border-radius:
    12px;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  background:
    rgba(66,213,245,.13);

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
    #92a4ae;

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


/* COMPANIES */

.company-section {

  overflow:
    hidden;

  background:

    radial-gradient(
      circle at 50% 60%,
      rgba(66,213,245,.07),
      transparent 35%
    ),

    #0f2029;

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
    #18303b;

  border:
    1px solid
    rgba(255,255,255,.1);

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
      rgba(66,213,245,.15),

    0 20px 70px
      rgba(66,213,245,.12);

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
      rgba(66,213,245,.16),

    0 25px 60px
      rgba(0,0,0,.35);

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
    #245060;

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
    #81939e;

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
    #8fa1ab;

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
    #71838e;

  font-size:
    12px;

}


.company-bottom b {

  color:
    var(--cyan);

  font-size:
    20px;

}


/* GLOWING ARROWS */

.slider-arrow {

  position:
    absolute;

  z-index:
    10;

  width:
    60px;

  height:
    60px;

  border:
    1px solid
    rgba(66,213,245,.6);

  border-radius:
    50%;

  background:
    #142933;

  color:
    var(--cyan);

  font-size:
    26px;

  cursor:
    pointer;

  box-shadow:

    0 0 20px
      rgba(66,213,245,.18),

    inset 0 0 15px
      rgba(66,213,245,.06);

  transition:
    .3s;

}


.slider-arrow:hover {

  transform:
    scale(1.18);

  background:
    var(--cyan);

  color:
    #061018;

  border-color:
    var(--cyan);

  box-shadow:

    0 0 20px
      rgba(66,213,245,.6),

    0 0 55px
      rgba(66,213,245,.35);

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
    #52646e;

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
    rgba(66,213,245,.5);

}


/* CTA */

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
      rgba(66,213,245,.17),
      transparent 45%
    ),

    linear-gradient(
      135deg,
      #10232d,
      #0c1820
    );

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
    rgba(66,213,245,.07);

  filter:
    blur(70px);

}


.cta-content {

  position:
    relative;

  z-index:
    2;

}


.cta-free {

  display:
    inline-flex;

  align-items:
    center;

  gap:
    10px;

  margin-bottom:
    22px;

  padding:
    12px 21px;

  border:
    1px solid
    rgba(66,213,245,.5);

  border-radius:
    999px;

  background:
    rgba(66,213,245,.1);

  color:
    var(--cyan);

  font-size:
    14px;

  font-weight:
    900;

}


.cta-free span {

  width:
    23px;

  height:
    23px;

  display:
    flex;

  align-items:
    center;

  justify-content:
    center;

  border-radius:
    50%;

  background:
    var(--cyan);

  color:
    #061018;

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
    #91a3ad;

  font-size:
    17px;

  margin:
    25px auto 35px;

}


.big-cta {

  font-size:
    17px;

  padding:
    16px 27px;

}


/* FAQ */

.faq-section {

  background:
    #edf4f7;

  color:
    #101a22;

}


.faq-section .eyebrow {

  color:
    #098eae;

}


.faq-section
.section-heading h2 {

  color:
    #101a22;

}


.faq-section
.section-heading h2 span {

  color:
    #0998bc;

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
    #d9e2e6;

  border-radius:
    14px;

  overflow:
    hidden;

  transition:
    .25s;

}


.faq-item:hover {

  border-color:
    #a8d8e5;

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
    34px;

  height:
    34px;

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
    #e4f6fa;

  color:
    #159cc3;

  font-size:
    20px;

}


.faq-answer {

  padding:
    0 25px 23px;

  color:
    #687680;

  line-height:
    1.8;

}


/* FOOTER */

footer {

  background:
    #09151c;

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
    #788b96;

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
    #61737e;

  font-size:
    10px;

  line-height:
    1.7;

}


.footer-copy {

  margin-top:
    15px;

}


/* FLOATING CTA */

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
    14px 22px;

  border-radius:
    999px;

  background:
    var(--cyan);

  color:
    #061018;

  font-size:
    13px;

  font-weight:
    900;

  box-shadow:
    0 8px 25px
    rgba(0,0,0,.35);

  transition:
    .3s;

}


.floating-cta span {

  margin-left:
    5px;

}


.floating-cta:hover {

  transform:
    translateY(-3px)
    scale(1.03);

  box-shadow:
    0 12px 35px
    rgba(66,213,245,.3);

}


/* HIGH CONTRAST */

.high-contrast {

  --cyan:
    #00e5ff;

  --cyan-light:
    #66efff;

}


.high-contrast
.hero,
.high-contrast
.dark-section,
.high-contrast
footer {

  background:
    #071016;

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


/* MOBILE */

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
      20px 24px 28px;

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
      #0d1b23;

  }


  .mobile-nav a {

    color:
      #c1cdd3;

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
      12px 15px;

    border:
      1px solid
      var(--border);

    border-radius:
      10px;

    background:
      #152933;

    color:
      #c3d0d6;

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
      48px;

    height:
      43px;

    border:
      1px solid
      var(--border);

    border-radius:
      8px;

    background:
      #152933;

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
      70px;

    padding:
      0 18px;

  }


  .header-actions {

    display:
      none;

  }


  .ticker {

    margin-top:
      70px;

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
      27px 19px;

  }


  .free-banner {

    width:
      calc(100% + 12px);

    margin:
      -43px -6px 22px;

    min-height:
      88px;

    padding:
      14px;

  }


  .free-banner-icon {

    width:
      43px;

    height:
      43px;

    font-size:
      22px;

  }


  .free-banner-text strong {

    font-size:
      21px;

  }


  .free-banner-text span {

    font-size:
      10px;

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
