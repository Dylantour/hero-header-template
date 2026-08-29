<script setup>
import { ref } from 'vue'

const menuOpen = ref(false)
const faqOpen = ref(null)

const faqs = [
  {
    question: 'האם אפשר למשוך כסף מקופת גמל או מקרן פנסיה?',
    answer:
      'האפשרות והדרך לבצע משיכה תלויות בסוג הקופה, במועד ההפקדות ובנסיבות האישיות. אנחנו בודקים את הנתונים ומציגים בפניכם את האפשרויות הרלוונטיות.'
  },
  {
    question: 'האם כדאי למשוך את הכסף או לקחת הלוואה כנגד הקופה?',
    answer:
      'לא תמיד משיכה היא האפשרות המתאימה. במקרים מסוימים ניתן לבחון גם הלוואה כנגד החיסכון. אנחנו עוזרים להבין את האפשרויות לפני שמקבלים החלטה.'
  },
  {
    question: 'כמה מס משלמים על משיכת כספים?',
    answer:
      'המס תלוי בסוג הקופה, בסוג הכספים ובנסיבות המשיכה. לכן חשוב לבצע בדיקה פרטנית לפני שמחליטים על משיכה.'
  },
  {
    question: 'כמה עולה הבדיקה?',
    answer:
      'השאירו פרטים ונציג יחזור אליכם כדי להסביר את התהליך ואת התנאים הרלוונטיים עבורכם.'
  }
]

const toggleFaq = (index) => {
  faqOpen.value = faqOpen.value === index ? null : index
}
</script>

<template>
  <div dir="rtl" class="min-h-screen bg-white text-slate-900">

    <!-- Header -->
    <header class="sticky top-0 z-50 border-b border-slate-100 bg-white/95 backdrop-blur">
      <div class="mx-auto flex max-w-7xl items-center justify-between px-5 py-4 lg:px-8">

        <!-- Logo -->
        <a href="#" class="flex items-center gap-2">
          <div class="flex h-10 w-10 items-center justify-center rounded-xl bg-slate-900 text-lg font-bold text-white">
            פ
          </div>

          <div>
            <div class="text-xl font-bold tracking-tight text-slate-900">
              פנסרה
            </div>
            <div class="text-[11px] text-slate-500">
              פתרונות פיננסיים
            </div>
          </div>
        </a>

        <!-- Desktop Navigation -->
        <nav class="hidden items-center gap-8 md:flex">
          <a href="#services" class="text-sm font-medium text-slate-600 transition hover:text-slate-900">
            השירותים שלנו
          </a>

          <a href="#how" class="text-sm font-medium text-slate-600 transition hover:text-slate-900">
            איך זה עובד?
          </a>

          <a href="#faq" class="text-sm font-medium text-slate-600 transition hover:text-slate-900">
            שאלות נפוצות
          </a>

          <a
            href="#contact"
            class="rounded-xl bg-slate-900 px-5 py-3 text-sm font-semibold text-white transition hover:bg-slate-800"
          >
            בדיקה ראשונית
          </a>
        </nav>

        <!-- Mobile button -->
        <button
          class="rounded-lg p-2 text-slate-700 md:hidden"
          @click="menuOpen = !menuOpen"
          aria-label="פתיחת תפריט"
        >
          <svg
            v-if="!menuOpen"
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 12h16M4 18h16" />
          </svg>

          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- Mobile menu -->
      <div v-if="menuOpen" class="border-t border-slate-100 bg-white px-5 py-5 md:hidden">
        <div class="flex flex-col gap-4">
          <a href="#services" @click="menuOpen = false" class="font-medium text-slate-700">
            השירותים שלנו
          </a>

          <a href="#how" @click="menuOpen = false" class="font-medium text-slate-700">
            איך זה עובד?
          </a>

          <a href="#faq" @click="menuOpen = false" class="font-medium text-slate-700">
            שאלות נפוצות
          </a>

          <a
            href="#contact"
            @click="menuOpen = false"
            class="rounded-xl bg-slate-900 px-5 py-3 text-center font-semibold text-white"
          >
            בדיקה ראשונית
          </a>
        </div>
      </div>
    </header>


    <!-- Hero -->
    <main>

      <section class="relative overflow-hidden bg-slate-50">
        <div class="absolute -left-32 -top-32 h-96 w-96 rounded-full bg-blue-100/60 blur-3xl"></div>
        <div class="absolute -bottom-40 -right-20 h-96 w-96 rounded-full bg-slate-200/70 blur-3xl"></div>

        <div class="relative mx-auto grid max-w-7xl items-center gap-14 px-5 py-20 lg:grid-cols-2 lg:px-8 lg:py-28">

          <!-- Hero text -->
          <div class="max-w-2xl">

            <div class="mb-6 inline-flex items-center gap-2 rounded-full border border-slate-200 bg-white px-4 py-2 text-sm font-medium text-slate-600 shadow-sm">
              <span class="h-2 w-2 rounded-full bg-emerald-500"></span>
              בדיקה ראשונית ללא התחייבות
            </div>

            <h1 class="text-4xl font-bold leading-tight tracking-tight text-slate-950 sm:text-5xl lg:text-6xl">
              יש לכם כסף בקופות?
              <span class="block text-blue-700">
                בואו נבדוק מה אפשר לעשות איתו.
              </span>
            </h1>

            <p class="mt-7 max-w-xl text-lg leading-8 text-slate-600">
              בפנסרה עוזרים לכם להבין אילו אפשרויות עומדות בפניכם
              בנוגע לחיסכון הפנסיוני, קופות גמל, קרנות השתלמות
              והלוואות כנגד קופות.
            </p>

            <div class="mt-9 flex flex-col gap-3 sm:flex-row">

              <a
                href="#contact"
                class="rounded-xl bg-slate-900 px-7 py-4 text-center font-semibold text-white shadow-lg shadow-slate-900/10 transition hover:-translate-y-0.5 hover:bg-slate-800"
              >
                רוצה לבדוק את האפשרויות שלי
              </a>

              <a
                href="#services"
                class="rounded-xl border border-slate-200 bg-white px-7 py-4 text-center font-semibold text-slate-700 transition hover:bg-slate-50"
              >
                איך אנחנו יכולים לעזור?
              </a>

            </div>

            <div class="mt-8 flex flex-wrap gap-x-6 gap-y-3 text-sm text-slate-500">
              <span>✓ תהליך פשוט וברור</span>
              <span>✓ ליווי אישי</span>
              <span>✓ בלי התחייבות לבדיקה הראשונית</span>
            </div>
          </div>


          <!-- Hero card -->
          <div class="relative">

            <div class="rounded-3xl border border-slate-200 bg-white p-7 shadow-2xl shadow-slate-900/10 sm:p-9">

              <div class="mb-8 flex items-center justify-between">
                <div>
                  <p class="text-sm font-medium text-slate-500">
                    פנסרה
                  </p>

                  <h2 class="mt-1 text-2xl font-bold text-slate-900">
                    עושים סדר בקופות
                  </h2>
                </div>

                <div class="flex h-12 w-12 items-center justify-center rounded-2xl bg-blue-50 text-blue-700">
                  ₪
                </div>
              </div>

              <div class="space-y-4">

                <div class="rounded-2xl bg-slate-50 p-5">
                  <div class="flex items-center justify-between">
                    <span class="font-semibold text-slate-800">
                      קרן השתלמות
                    </span>

                    <span class="text-sm text-emerald-600">
                      אפשרויות לבדיקה
                    </span>
                  </div>
                </div>

                <div class="rounded-2xl bg-slate-50 p-5">
                  <div class="flex items-center justify-between">
                    <span class="font-semibold text-slate-800">
                      קופת גמל
                    </span>

                    <span class="text-sm text-blue-600">
                      אפשרויות לבדיקה
                    </span>
                  </div>
                </div>

                <div class="rounded-2xl bg-slate-50 p-5">
                  <div class="flex items-center justify-between">
                    <span class="font-semibold text-slate-800">
                      הלוואה כנגד קופה
                    </span>

                    <span class="text-sm text-slate-500">
                      בכפוף לתנאים
                    </span>
                  </div>
                </div>

              </div>

              <div class="mt-7 rounded-2xl bg-slate-900 p-5 text-white">
                <p class="text-sm text-slate-300">
                  לא יודעים מה האפשרות שמתאימה לכם?
                </p>

                <p class="mt-1 font-semibold">
                  מתחילים בבדיקה ומבינים את האפשרויות.
                </p>
              </div>

            </div>
          </div>

        </div>
      </section>


      <!-- Trust strip -->
      <section class="border-b border-slate-100 bg-white">
        <div class="mx-auto grid max-w-7xl grid-cols-1 gap-6 px-5 py-8 sm:grid-cols-3 lg:px-8">

          <div class="flex items-center gap-4">
            <div class="flex h-11 w-11 shrink-0 items-center justify-center rounded-full bg-slate-100 font-bold text-slate-700">
              01
            </div>

            <div>
              <p class="font-semibold text-slate-900">בדיקה מסודרת</p>
              <p class="text-sm text-slate-500">מבינים קודם את המצב</p>
            </div>
          </div>

          <div class="flex items-center gap-4">
            <div class="flex h-11 w-11 shrink-0 items-center justify-center rounded-full bg-slate-100 font-bold text-slate-700">
              02
            </div>

            <div>
              <p class="font-semibold text-slate-900">אפשרויות ברורות</p>
              <p class="text-sm text-slate-500">בלי מונחים מסובכים</p>
            </div>
          </div>

          <div class="flex items-center gap-4">
            <div class="flex h-11 w-11 shrink-0 items-center justify-center rounded-full bg-slate-100 font-bold text-slate-700">
              03
            </div>

            <div>
              <p class="font-semibold text-slate-900">ליווי אישי</p>
              <p class="text-sm text-slate-500">עד שמבינים מה עושים</p>
            </div>
          </div>

        </div>
      </section>


      <!-- Services -->
      <section id="services" class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28">

        <div class="max-w-2xl">
          <p class="text-sm font-bold uppercase tracking-widest text-blue-700">
            השירותים שלנו
          </p>

          <h2 class="mt-3 text-3xl font-bold tracking-tight text-slate-950 sm:text-4xl">
            פתרונות שמתאימים למצב שלכם
          </h2>

          <p class="mt-5 text-lg leading-8 text-slate-600">
            במקום לנסות להבין לבד מה אפשר לעשות עם הכספים שלכם,
            אנחנו עוזרים למפות את האפשרויות הרלוונטיות.
          </p>
        </div>


        <div class="mt-12 grid gap-6 md:grid-cols-2 lg:grid-cols-4">

          <div class="rounded-2xl border border-slate-200 bg-white p-7 transition hover:-translate-y-1 hover:shadow-xl hover:shadow-slate-900/5">
            <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-xl font-bold text-blue-700">
              ₪
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              משיכת כספים
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בדיקה של אפשרויות משיכת כספים מקופות בהתאם לסוג הכספים ולנסיבות האישיות.
            </p>
          </div>


          <div class="rounded-2xl border border-slate-200 bg-white p-7 transition hover:-translate-y-1 hover:shadow-xl hover:shadow-slate-900/5">
            <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-xl font-bold text-blue-700">
              ↗
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              קרנות השתלמות
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בחינת אפשרויות הקיימות עבור כספים בקרנות השתלמות.
            </p>
          </div>


          <div class="rounded-2xl border border-slate-200 bg-white p-7 transition hover:-translate-y-1 hover:shadow-xl hover:shadow-slate-900/5">
            <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-xl font-bold text-blue-700">
              ◇
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              קופות גמל
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בדיקה והכוונה בנוגע לאפשרויות שעשויות להיות רלוונטיות לקופת הגמל.
            </p>
          </div>


          <div class="rounded-2xl border border-slate-200 bg-white p-7 transition hover:-translate-y-1 hover:shadow-xl hover:shadow-slate-900/5">
            <div class="flex h-12 w-12 items-center justify-center rounded-xl bg-blue-50 text-xl font-bold text-blue-700">
              +
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              הלוואה כנגד קופה
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בחינת אפשרות לקבלת הלוואה כנגד חיסכון קיים, בכפוף לתנאי הגוף המנהל.
            </p>
          </div>

        </div>
      </section>


      <!-- How it works -->
      <section id="how" class="bg-slate-50">
        <div class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28">

          <div class="text-center">
            <p class="text-sm font-bold uppercase tracking-widest text-blue-700">
              איך זה עובד?
            </p>

            <h2 class="mt-3 text-3xl font-bold tracking-tight text-slate-950 sm:text-4xl">
              פשוט. ברור. בלי להסתבך.
            </h2>
          </div>


          <div class="mt-14 grid gap-8 md:grid-cols-3">

            <div class="relative rounded-2xl bg-white p-8 shadow-sm">
              <span class="text-5xl font-bold text-slate-100">01</span>

              <h3 class="mt-5 text-xl font-bold text-slate-900">
                משאירים פרטים
              </h3>

              <p class="mt-3 leading-7 text-slate-600">
                מספר פרטים בסיסיים מאפשרים לנו להבין במה מדובר.
              </p>
            </div>


            <div class="relative rounded-2xl bg-white p-8 shadow-sm">
              <span class="text-5xl font-bold text-slate-100">02</span>

              <h3 class="mt-5 text-xl font-bold text-slate-900">
                בודקים את האפשרויות
              </h3>

              <p class="mt-3 leading-7 text-slate-600">
                עוברים על הנתונים ומסבירים מה ניתן לבדוק ומה המשמעות של כל אפשרות.
              </p>
            </div>


            <div class="relative rounded-2xl bg-white p-8 shadow-sm">
              <span class="text-5xl font-bold text-slate-100">03</span>

              <h3 class="mt-5 text-xl font-bold text-slate-900">
                מקבלים החלטה
              </h3>

              <p class="mt-3 leading-7 text-slate-600">
                רק אחרי שמבינים את האפשרויות מחליטים אם ואיך להתקדם.
              </p>
            </div>

          </div>

        </div>
      </section>


      <!-- CTA -->
      <section id="contact" class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28">

        <div class="overflow-hidden rounded-3xl bg-slate-900 px-6 py-12 text-white sm:px-12 lg:px-16 lg:py-16">

          <div class="grid items-center gap-10 lg:grid-cols-2">

            <div>
              <p class="text-sm font-bold uppercase tracking-widest text-blue-300">
                מתחילים כאן
              </p>

              <h2 class="mt-3 text-3xl font-bold leading-tight sm:text-4xl">
                רוצים לדעת אילו אפשרויות עומדות בפניכם?
              </h2>

              <p class="mt-5 max-w-xl leading-7 text-slate-300">
                השאירו פרטים ונציג יחזור אליכם כדי להבין את הצורך
                ולבחון את האפשרויות הרלוונטיות.
              </p>
            </div>


            <form class="rounded-2xl bg-white p-6 text-slate-900 shadow-xl sm:p-8">

              <div class="space-y-4">

                <div>
                  <label class="mb-2 block text-sm font-semibold">
                    שם מלא
                  </label>

                  <input
                    type="text"
                    placeholder="השם שלך"
                    class="w-full rounded-xl border border-slate-200 px-4 py-3 outline-none transition focus:border-blue-500 focus:ring-2 focus:ring-blue-100"
                  />
                </div>


                <div>
                  <label class="mb-2 block text-sm font-semibold">
                    טלפון
                  </label>

                  <input
                    type="tel"
                    placeholder="050-0000000"
                    dir="ltr"
                    class="w-full rounded-xl border border-slate-200 px-4 py-3 text-right outline-none transition focus:border-blue-500 focus:ring-2 focus:ring-blue-100"
                  />
                </div>


                <div>
                  <label class="mb-2 block text-sm font-semibold">
                    במה תרצו לבדוק?
                  </label>

                  <select
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 outline-none transition focus:border-blue-500 focus:ring-2 focus:ring-blue-100"
                  >
                    <option>בחרו אפשרות</option>
                    <option>משיכת כספים</option>
                    <option>קרן השתלמות</option>
                    <option>קופת גמל</option>
                    <option>הלוואה כנגד קופה</option>
                    <option>לא בטוח/ה</option>
                  </select>
                </div>


                <button
                  type="button"
                  class="w-full rounded-xl bg-slate-900 px-5 py-4 font-bold text-white transition hover:bg-blue-700"
                >
                  שלחו לי פרטים לבדיקה
                </button>

                <p class="text-center text-xs leading-5 text-slate-400">
                  השארת הפרטים אינה מהווה התחייבות לקבלת שירות.
                  בכפוף לתנאים ולבדיקה פרטנית.
                </p>

              </div>

            </form>

          </div>

        </div>

      </section>


      <!-- FAQ -->
      <section id="faq" class="bg-slate-50">
        <div class="mx-auto max-w-4xl px-5 py-20 lg:py-28">

          <div class="text-center">
            <p class="text-sm font-bold uppercase tracking-widest text-blue-700">
              שאלות נפוצות
            </p>

            <h2 class="mt-3 text-3xl font-bold text-slate-950 sm:text-4xl">
              לפני שמתחילים
            </h2>
          </div>


          <div class="mt-10 space-y-3">

            <div
              v-for="(faq, index) in faqs"
              :key="faq.question"
              class="overflow-hidden rounded-2xl border border-slate-200 bg-white"
            >

              <button
                class="flex w-full items-center justify-between gap-6 px-6 py-5 text-right"
                @click="toggleFaq(index)"
              >

                <span class="font-semibold text-slate-900">
                  {{ faq.question }}
                </span>

                <span class="shrink-0 text-2xl text-slate-400">
                  {{ faqOpen === index ? '−' : '+' }}
                </span>

              </button>


              <div v-if="faqOpen === index" class="border-t border-slate-100 px-6 py-5">
                <p class="leading-7 text-slate-600">
                  {{ faq.answer }}
                </p>
              </div>

            </div>

          </div>

        </div>
      </section>

    </main>


    <!-- Footer -->
    <footer class="bg-slate-950 text-slate-400">

      <div class="mx-auto max-w-7xl px-5 py-12 lg:px-8">

        <div class="flex flex-col gap-8 md:flex-row md:items-center md:justify-between">

          <div>
            <div class="text-xl font-bold text-white">
              פנסרה
            </div>

            <p class="mt-2 max-w-md text-sm leading-6">
              פתרונות ושירותים בתחום החיסכון והקופות.
            </p>
          </div>

          <div class="text-sm">
            © {{ new Date().getFullYear() }} פנסרה. כל הזכויות שמורות.
          </div>

        </div>

        <div class="mt-8 border-t border-slate-800 pt-6 text-xs leading-6 text-slate-500">
          המידע באתר הינו כללי ואינו מהווה ייעוץ פנסיוני, ייעוץ השקעות,
          התחייבות לקבלת הלוואה או התחייבות לביצוע משיכה.
          האפשרויות והתנאים כפופים לנתונים האישיים ולתנאי הגופים הרלוונטיים.
        </div>

      </div>

    </footer>

  </div>
</template>
