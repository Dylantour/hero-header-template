<script setup>
import { ref } from 'vue'

const menuOpen = ref(false)
const faqOpen = ref(null)

const faqs = [
  {
    question: 'האם אפשר למשוך כסף מקופת גמל או מקרן פנסיה?',
    answer:
      'האפשרות לבצע משיכה תלויה בסוג הקופה, בסוג הכספים ובנסיבות האישיות. אנחנו בודקים את הנתונים ומסבירים אילו אפשרויות ניתן לבחון.'
  },
  {
    question: 'מה עדיף – למשוך את הכסף או לקחת הלוואה כנגד הקופה?',
    answer:
      'אין תשובה אחת שמתאימה לכולם. במקרים מסוימים ניתן לבחון הלוואה כנגד החיסכון במקום משיכה. אנחנו עוזרים להבין את האפשרויות לפני שמקבלים החלטה.'
  },
  {
    question: 'כמה מס משלמים על משיכת כספים?',
    answer:
      'גובה המס, אם חל, תלוי בסוג הקופה, בסוג הכספים ובנסיבות המשיכה. לכן חשוב לבצע בדיקה פרטנית לפני קבלת החלטה.'
  },
  {
    question: 'האם הבדיקה הראשונית מחייבת אותי?',
    answer:
      'לא. השארת הפרטים מאפשרת לנו ליצור קשר ולהבין את הצורך. כל פעולה נוספת מתבצעת רק לאחר שמבינים את האפשרויות והתנאים הרלוונטיים.'
  }
]

const toggleFaq = (index) => {
  faqOpen.value = faqOpen.value === index ? null : index
}
</script>

<template>
  <div dir="rtl" class="min-h-screen bg-white text-slate-900">

    <!-- ================= HEADER ================= -->
    <header class="sticky top-0 z-50 border-b border-indigo-100 bg-white/95 backdrop-blur">
      <div class="mx-auto flex max-w-7xl items-center justify-between px-5 py-4 lg:px-8">

        <!-- Logo -->
        <a href="#" class="flex items-center gap-3">

          <div
            class="flex h-11 w-11 items-center justify-center rounded-xl bg-indigo-500 text-xl font-bold text-white shadow-sm"
          >
            פ
          </div>

          <div>
            <div class="text-xl font-bold tracking-tight text-slate-900">
              פנסרה
            </div>

            <div class="text-[11px] font-medium text-slate-500">
              פתרונות פיננסיים
            </div>
          </div>

        </a>


        <!-- Desktop navigation -->
        <nav class="hidden items-center gap-8 md:flex">

          <a
            href="#services"
            class="text-sm font-medium text-slate-600 transition hover:text-indigo-600"
          >
            השירותים שלנו
          </a>

          <a
            href="#how"
            class="text-sm font-medium text-slate-600 transition hover:text-indigo-600"
          >
            איך זה עובד?
          </a>

          <a
            href="#faq"
            class="text-sm font-medium text-slate-600 transition hover:text-indigo-600"
          >
            שאלות נפוצות
          </a>

          <a
            href="#contact"
            class="rounded-xl bg-indigo-500 px-5 py-3 text-sm font-bold text-white shadow-sm transition hover:bg-indigo-600"
          >
            בדיקה ראשונית
          </a>

        </nav>


        <!-- Mobile menu button -->
        <button
          class="rounded-xl p-2 text-slate-700 md:hidden"
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


      <!-- Mobile navigation -->
      <div
        v-if="menuOpen"
        class="border-t border-indigo-100 bg-white px-5 py-5 md:hidden"
      >

        <div class="flex flex-col gap-4">

          <a
            href="#services"
            @click="menuOpen = false"
            class="font-medium text-slate-700"
          >
            השירותים שלנו
          </a>

          <a
            href="#how"
            @click="menuOpen = false"
            class="font-medium text-slate-700"
          >
            איך זה עובד?
          </a>

          <a
            href="#faq"
            @click="menuOpen = false"
            class="font-medium text-slate-700"
          >
            שאלות נפוצות
          </a>

          <a
            href="#contact"
            @click="menuOpen = false"
            class="rounded-xl bg-indigo-500 px-5 py-3 text-center font-bold text-white"
          >
            בדיקה ראשונית
          </a>

        </div>

      </div>

    </header>


    <!-- ================= HERO ================= -->
    <main>

      <section class="relative overflow-hidden bg-indigo-500">

        <!-- Decorative background -->
        <div
          class="absolute -left-40 -top-40 h-[500px] w-[500px] rounded-full bg-indigo-400/40 blur-3xl"
        ></div>

        <div
          class="absolute -bottom-48 -right-32 h-[550px] w-[550px] rounded-full bg-indigo-700/40 blur-3xl"
        ></div>


        <div
          class="relative mx-auto grid min-h-[650px] max-w-7xl items-center gap-14 px-5 py-16 lg:grid-cols-2 lg:px-8 lg:py-24"
        >

          <!-- Hero text -->
          <div class="text-right">

            <div
              class="mb-7 inline-flex items-center gap-2 rounded-full bg-white/10 px-4 py-2 text-sm font-medium text-white ring-1 ring-white/20"
            >
              <span class="h-2 w-2 rounded-full bg-emerald-300"></span>

              בדיקה ראשונית ללא התחייבות
            </div>


            <h1
              class="max-w-2xl text-4xl font-bold leading-tight tracking-tight text-white sm:text-5xl lg:text-6xl"
            >
              יש לכם כספים בקופות?

              <span class="mt-3 block text-indigo-100">
                בואו נבדוק מה האפשרויות שלכם.
              </span>
            </h1>


            <p
              class="mt-7 max-w-xl text-lg leading-8 text-indigo-50 sm:text-xl"
            >
              בפנסרה עוזרים לכם להבין אילו אפשרויות עומדות בפניכם
              בנוגע למשיכת כספים, קרנות השתלמות, קופות גמל
              והלוואות כנגד קופות.
            </p>


            <div class="mt-9 flex flex-col gap-3 sm:flex-row">

              <a
                href="#contact"
                class="rounded-xl bg-white px-7 py-4 text-center font-bold text-indigo-700 shadow-xl shadow-indigo-900/20 transition hover:-translate-y-1 hover:bg-indigo-50"
              >
                רוצה לבדוק את האפשרויות שלי
              </a>

              <a
                href="#services"
                class="rounded-xl border border-white/30 bg-white/10 px-7 py-4 text-center font-semibold text-white transition hover:bg-white/20"
              >
                איך זה עובד?
              </a>

            </div>


            <div
              class="mt-8 flex flex-wrap gap-x-7 gap-y-3 text-sm text-indigo-100"
            >

              <span>✓ תהליך פשוט וברור</span>

              <span>✓ יחס אישי</span>

              <span>✓ ללא התחייבות לבדיקה הראשונית</span>

            </div>

          </div>


          <!-- Hero visual -->
          <div class="relative flex items-center justify-center lg:justify-start">

            <div class="relative w-full max-w-lg">

              <!-- Glow -->
              <div
                class="absolute -inset-5 rounded-[2rem] bg-white/10 blur-2xl"
              ></div>


              <!-- Card -->
              <div
                class="relative rounded-[2rem] border border-white/20 bg-white p-7 shadow-2xl shadow-indigo-950/30 sm:p-9"
              >

                <div class="flex items-center justify-between">

                  <div>

                    <p class="text-sm font-medium text-slate-400">
                      פנסרה
                    </p>

                    <h2 class="mt-1 text-2xl font-bold text-slate-900">
                      עושים סדר בקופות
                    </h2>

                  </div>


                  <div
                    class="flex h-12 w-12 items-center justify-center rounded-2xl bg-indigo-50 text-xl font-bold text-indigo-600"
                  >
                    ₪
                  </div>

                </div>


                <div class="mt-8 space-y-3">

                  <!-- Item 1 -->
                  <div
                    class="flex items-center justify-between rounded-2xl bg-slate-50 px-5 py-4"
                  >

                    <div class="flex items-center gap-3">

                      <div
                        class="flex h-10 w-10 items-center justify-center rounded-xl bg-indigo-100 text-sm font-bold text-indigo-600"
                      >
                        01
                      </div>

                      <span class="font-semibold text-slate-800">
                        קרן השתלמות
                      </span>

                    </div>

                    <span class="text-sm font-medium text-emerald-600">
                      לבדיקה
                    </span>

                  </div>


                  <!-- Item 2 -->
                  <div
                    class="flex items-center justify-between rounded-2xl bg-slate-50 px-5 py-4"
                  >

                    <div class="flex items-center gap-3">

                      <div
                        class="flex h-10 w-10 items-center justify-center rounded-xl bg-indigo-100 text-sm font-bold text-indigo-600"
                      >
                        02
                      </div>

                      <span class="font-semibold text-slate-800">
                        קופת גמל
                      </span>

                    </div>

                    <span class="text-sm font-medium text-indigo-600">
                      לבדיקה
                    </span>

                  </div>


                  <!-- Item 3 -->
                  <div
                    class="flex items-center justify-between rounded-2xl bg-slate-50 px-5 py-4"
                  >

                    <div class="flex items-center gap-3">

                      <div
                        class="flex h-10 w-10 items-center justify-center rounded-xl bg-indigo-100 text-sm font-bold text-indigo-600"
                      >
                        03
                      </div>

                      <span class="font-semibold text-slate-800">
                        הלוואה כנגד קופה
                      </span>

                    </div>

                    <span class="text-sm font-medium text-slate-500">
                      בכפוף לתנאים
                    </span>

                  </div>

                </div>


                <div class="mt-6 rounded-2xl bg-indigo-500 p-5 text-white">

                  <div class="flex items-center gap-3">

                    <div
                      class="flex h-10 w-10 items-center justify-center rounded-full bg-white/15 font-bold"
                    >
                      ✓
                    </div>

                    <div>

                      <p class="text-sm text-indigo-100">
                        לא בטוחים מה מתאים לכם?
                      </p>

                      <p class="mt-1 font-bold">
                        מתחילים בבדיקה ומבינים את האפשרויות.
                      </p>

                    </div>

                  </div>

                </div>

              </div>


              <!-- Floating badge -->
              <div
                class="absolute -bottom-5 -right-4 hidden rounded-2xl border border-indigo-100 bg-white px-5 py-4 shadow-xl sm:block"
              >

                <p class="text-xs font-medium text-slate-400">
                  פנסרה
                </p>

                <p class="mt-1 font-bold text-slate-900">
                  עושים סדר בכסף
                </p>

              </div>

            </div>

          </div>

        </div>

      </section>


      <!-- ================= BENEFITS ================= -->
      <section class="border-b border-slate-100 bg-white">

        <div
          class="mx-auto grid max-w-7xl grid-cols-1 gap-8 px-5 py-10 sm:grid-cols-3 lg:px-8"
        >

          <div class="flex items-center gap-4">

            <div
              class="flex h-12 w-12 shrink-0 items-center justify-center rounded-full bg-indigo-50 font-bold text-indigo-600"
            >
              01
            </div>

            <div>

              <p class="font-bold text-slate-900">
                בדיקה מסודרת
              </p>

              <p class="mt-1 text-sm text-slate-500">
                מבינים קודם את המצב
              </p>

            </div>

          </div>


          <div class="flex items-center gap-4">

            <div
              class="flex h-12 w-12 shrink-0 items-center justify-center rounded-full bg-indigo-50 font-bold text-indigo-600"
            >
              02
            </div>

            <div>

              <p class="font-bold text-slate-900">
                אפשרויות ברורות
              </p>

              <p class="mt-1 text-sm text-slate-500">
                בלי מונחים מסובכים
              </p>

            </div>

          </div>


          <div class="flex items-center gap-4">

            <div
              class="flex h-12 w-12 shrink-0 items-center justify-center rounded-full bg-indigo-50 font-bold text-indigo-600"
            >
              03
            </div>

            <div>

              <p class="font-bold text-slate-900">
                ליווי אישי
              </p>

              <p class="mt-1 text-sm text-slate-500">
                עד שמבינים מה עושים
              </p>

            </div>

          </div>

        </div>

      </section>


      <!-- ================= SERVICES ================= -->
      <section
        id="services"
        class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28"
      >

        <div class="max-w-2xl">

          <p class="text-sm font-bold uppercase tracking-widest text-indigo-600">
            השירותים שלנו
          </p>

          <h2
            class="mt-3 text-3xl font-bold tracking-tight text-slate-950 sm:text-4xl"
          >
            פתרונות שמתאימים למצב שלכם
          </h2>

          <p class="mt-5 text-lg leading-8 text-slate-600">
            במקום לנסות להבין לבד מה אפשר לעשות עם הכספים שלכם,
            אנחנו עוזרים למפות את האפשרויות הרלוונטיות.
          </p>

        </div>


        <div class="mt-12 grid gap-6 md:grid-cols-2 lg:grid-cols-4">

          <!-- Service 1 -->
          <div
            class="group rounded-2xl border border-slate-200 bg-white p-7 transition duration-300 hover:-translate-y-1 hover:border-indigo-200 hover:shadow-xl hover:shadow-indigo-900/5"
          >

            <div
              class="flex h-12 w-12 items-center justify-center rounded-xl bg-indigo-50 text-xl font-bold text-indigo-600"
            >
              ₪
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              משיכת כספים
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בדיקה של אפשרויות משיכת כספים מקופות בהתאם לסוג הכספים ולנסיבות האישיות.
            </p>

          </div>


          <!-- Service 2 -->
          <div
            class="group rounded-2xl border border-slate-200 bg-white p-7 transition duration-300 hover:-translate-y-1 hover:border-indigo-200 hover:shadow-xl hover:shadow-indigo-900/5"
          >

            <div
              class="flex h-12 w-12 items-center justify-center rounded-xl bg-indigo-50 text-xl font-bold text-indigo-600"
            >
              ↗
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              קרנות השתלמות
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בחינת האפשרויות הקיימות עבור כספים בקרנות השתלמות.
            </p>

          </div>


          <!-- Service 3 -->
          <div
            class="group rounded-2xl border border-slate-200 bg-white p-7 transition duration-300 hover:-translate-y-1 hover:border-indigo-200 hover:shadow-xl hover:shadow-indigo-900/5"
          >

            <div
              class="flex h-12 w-12 items-center justify-center rounded-xl bg-indigo-50 text-xl font-bold text-indigo-600"
            >
              ◇
            </div>

            <h3 class="mt-6 text-xl font-bold text-slate-900">
              קופות גמל
            </h3>

            <p class="mt-3 leading-7 text-slate-600">
              בדיקה והכוונה בנוגע לאפשרויות שעשויות להיות רלוונטיות לקופת הגמל.
            </p>

          </div>


          <!-- Service 4 -->
          <div
            class="group rounded-2xl border border-slate-200 bg-white p-7 transition duration-300 hover:-translate-y-1 hover:border-indigo-200 hover:shadow-xl hover:shadow-indigo-900/5"
          >

            <div
              class="flex h-12 w-12 items-center justify-center rounded-xl bg-indigo-50 text-xl font-bold text-indigo-600"
            >
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


      <!-- ================= HOW IT WORKS ================= -->
      <section id="how" class="bg-indigo-50/50">

        <div class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28">

          <div class="text-center">

            <p class="text-sm font-bold uppercase tracking-widest text-indigo-600">
              איך זה עובד?
            </p>

            <h2
              class="mt-3 text-3xl font-bold tracking-tight text-slate-950 sm:text-4xl"
            >
              פשוט. ברור. בלי להסתבך.
            </h2>

          </div>


          <div class="mt-14 grid gap-6 md:grid-cols-3">

            <div class="rounded-2xl bg-white p-8 shadow-sm">

              <span class="text-5xl font-bold text-indigo-100">
                01
              </span>

              <h3 class="mt-5 text-xl font-bold text-slate-900">
                משאירים פרטים
              </h3>

              <p class="mt-3 leading-7 text-slate-600">
                מספר פרטים בסיסיים מאפשרים לנו להבין במה מדובר.
              </p>

            </div>


            <div class="rounded-2xl bg-white p-8 shadow-sm">

              <span class="text-5xl font-bold text-indigo-100">
                02
              </span>

              <h3 class="mt-5 text-xl font-bold text-slate-900">
                בודקים את האפשרויות
              </h3>

              <p class="mt-3 leading-7 text-slate-600">
                עוברים על הנתונים ומסבירים מה ניתן לבדוק ומה המשמעות של כל אפשרות.
              </p>

            </div>


            <div class="rounded-2xl bg-white p-8 shadow-sm">

              <span class="text-5xl font-bold text-indigo-100">
                03
              </span>

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


      <!-- ================= CTA / FORM ================= -->
      <section
        id="contact"
        class="mx-auto max-w-7xl px-5 py-20 lg:px-8 lg:py-28"
      >

        <div
          class="overflow-hidden rounded-[2rem] bg-indigo-500 px-6 py-12 shadow-2xl shadow-indigo-900/10 sm:px-12 lg:px-16 lg:py-16"
        >

          <div class="grid items-center gap-12 lg:grid-cols-2">

            <div>

              <p
                class="text-sm font-bold uppercase tracking-widest text-indigo-100"
              >
                מתחילים כאן
              </p>

              <h2
                class="mt-3 text-3xl font-bold leading-tight text-white sm:text-4xl"
              >
                רוצים לדעת אילו אפשרויות עומדות בפניכם?
              </h2>

              <p
                class="mt-5 max-w-xl text-lg leading-8 text-indigo-50"
              >
                השאירו פרטים ונציג יחזור אליכם כדי להבין את הצורך
                ולבחון את האפשרויות הרלוונטיות.
              </p>


              <div class="mt-8 space-y-3 text-sm text-indigo-50">

                <p>✓ תהליך פשוט וברור</p>

                <p>✓ יחס אישי</p>

                <p>✓ בדיקה בהתאם לנתונים שלכם</p>

              </div>

            </div>


            <!-- Form -->
            <form
              class="rounded-2xl bg-white p-6 text-slate-900 shadow-xl sm:p-8"
              @submit.prevent
            >

              <div class="space-y-5">

                <div>

                  <label class="mb-2 block text-sm font-bold">
                    שם מלא
                  </label>

                  <input
                    type="text"
                    placeholder="השם שלך"
                    class="w-full rounded-xl border border-slate-200 px-4 py-3 outline-none transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100"
                  />

                </div>


                <div>

                  <label class="mb-2 block text-sm font-bold">
                    מספר טלפון
                  </label>

                  <input
                    type="tel"
                    placeholder="050-0000000"
                    dir="ltr"
                    class="w-full rounded-xl border border-slate-200 px-4 py-3 text-right outline-none transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100"
                  />

                </div>


                <div>

                  <label class="mb-2 block text-sm font-bold">
                    במה תרצו לבדוק?
                  </label>

                  <select
                    class="w-full rounded-xl border border-slate-200 bg-white px-4 py-3 outline-none transition focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100"
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
                  type="submit"
                  class="w-full rounded-xl bg-indigo-500 px-5 py-4 font-bold text-white transition hover:bg-indigo-600"
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


      <!-- ================= FAQ ================= -->
      <section id="faq" class="bg-slate-50">

        <div class="mx-auto max-w-4xl px-5 py-20 lg:py-28">

          <div class="text-center">

            <p class="text-sm font-bold uppercase tracking-widest text-indigo-600">
              שאלות נפוצות
            </p>

            <h2
              class="mt-3 text-3xl font-bold text-slate-950 sm:text-4xl"
            >
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

                <span class="shrink-0 text-2xl font-light text-indigo-500">
                  {{ faqOpen === index ? '−' : '+' }}
                </span>

              </button>


              <div
                v-if="faqOpen === index"
                class="border-t border-slate-100 px-6 py-5"
              >

                <p class="leading-7 text-slate-600">
                  {{ faq.answer }}
                </p>

              </div>

            </div>

          </div>

        </div>

      </section>

    </main>


    <!-- ================= FOOTER ================= -->
    <footer class="bg-slate-950 text-slate-400">

      <div class="mx-auto max-w-7xl px-5 py-12 lg:px-8">

        <div
          class="flex flex-col gap-8 md:flex-row md:items-center md:justify-between"
        >

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


        <div
          class="mt-8 border-t border-slate-800 pt-6 text-xs leading-6 text-slate-500"
        >
          המידע באתר הינו כללי ואינו מהווה ייעוץ פנסיוני, ייעוץ השקעות,
          התחייבות לקבלת הלוואה או התחייבות לביצוע משיכה.
          האפשרויות והתנאים כפופים לנתונים האישיים ולתנאי הגופים הרלוונטיים.
        </div>

      </div>

    </footer>

  </div>
</template>
