-- ============================================================================
--  Delay & Reverb Calculator Pro v1.0  --  ReaScript (Lua) version
--  Standalone window for REAPER (no track / FX needed).
--  Port of the JSFX plugin: same layout, themes, calculators and EN/UA manual.
--
--  Install:  Actions > Show action list > New action > Load ReaScript...
--            pick this file, then run it (or put it on a toolbar / menu).
--  Window is resizable and dockable. Settings are remembered between runs.
-- ============================================================================

local r = reaper
local floor = math.floor
local EXT = "VitaliiSound_DelayReverbCalcPro"

HELP_PAGES = {
  {
    en = {
      {"h", 105, "1. OVERVIEW & BPM"},
      {"h", 145, "OVERVIEW"},
      {"t", 167, "Delay & Reverb Calculator is a tempo-based utility for calculating musical timing values for delay, reverb, and other time-based effects."},
      {"t", 187, "The calculator uses BPM and time signature to convert musical timing into exact time values."},
      {"t", 207, "It can also follow the host application tempo and time signature automatically."},
      {"t", 227, "Main sections: Reverb Suggestions, Note Durations, and Delay Tail."},
      {"t", 247, "Additional tools: Tap BPM, Custom Converter, display units, feedback scales, themes, and EN/UA Help."},
      {"h", 285, "BPM"},
      {"t", 307, "BPM means Beats Per Minute. It is the tempo used for all musical-time calculations."},
      {"t", 327, "60 BPM = 1000 ms per quarter note.  120 BPM = 500 ms.  140 BPM = approximately 428.57 ms."},
      {"t", 347, "Changing BPM changes note durations, reverb timing suggestions, and beat/bar conversions."},
      {"t", 367, "Manual BPM range: 20–300 BPM. Values outside this range are limited to the supported range."},
      {"t", 387, "To edit BPM: click the BPM value, type the number, and press Enter."},
    },
    ua = {
      {"h", 105, "1. ОГЛЯД І BPM"},
      {"h", 145, "ОГЛЯД"},
      {"t", 167, "Delay & Reverb Calculator — утиліта на основі темпу для розрахунку музичних значень часу для delay, reverb та інших ефектів."},
      {"t", 187, "Калькулятор використовує BPM і розмір такту, щоб перетворювати музичний таймінг у точні значення часу."},
      {"t", 207, "Також він може автоматично слідувати за темпом і розміром такту хоста."},
      {"t", 227, "Основні розділи: Reverb Suggestions, Note Durations і Delay Tail."},
      {"t", 247, "Додатково: Tap BPM, Custom Converter, одиниці відображення, шкали feedback, теми та EN/UA Help."},
      {"h", 285, "BPM"},
      {"t", 307, "BPM означає Beats Per Minute — удари за хвилину. Це основа всіх музичних розрахунків часу."},
      {"t", 327, "60 BPM = 1000 мс на чверть. 120 BPM = 500 мс. 140 BPM = приблизно 428.57 мс."},
      {"t", 347, "Зміна BPM змінює тривалості нот, таймінг ревербу та перерахунок у долі й такти."},
      {"t", 367, "Діапазон Manual BPM: 20–300 BPM. Значення за межами діапазону обмежуються підтримуваним діапазоном."},
      {"t", 387, "Для редагування BPM натисніть на значення, введіть число та натисніть Enter."},
    },
  },
  {
    en = {
      {"h", 105, "2. TAP BPM & SYNC"},
      {"h", 145, "TAP BPM"},
      {"t", 167, "Click TAP BPM repeatedly in time with the music."},
      {"t", 187, "The calculator averages the recent tap intervals and converts them to BPM."},
      {"t", 207, "Up to six recent intervals are used for the average."},
      {"t", 227, "If more than two seconds pass between taps, the current tap sequence is restarted."},
      {"t", 247, "Intervals outside the accepted range are ignored."},
      {"t", 267, "Middle- or right-click the TAP BPM area to reset the tap counter."},
      {"h", 305, "MANUAL / SYNC"},
      {"t", 327, "Manual uses the BPM and time signature entered by the user."},
      {"t", 347, "Use Manual for independent calculations, experimentation, or work outside a DAW project."},
      {"t", 367, "Sync follows the host application tempo. The BPM control is locked while Sync is active."},
      {"t", 387, "When available, the host time signature is also used. Changing project tempo updates the calculator automatically."},
      {"t", 407, "If the host does not provide a valid tempo, the calculator keeps a valid BPM within its supported range."},
    },
    ua = {
      {"h", 105, "2. TAP BPM ТА SYNC"},
      {"h", 145, "TAP BPM"},
      {"t", 167, "Натискайте TAP BPM кілька разів у ритм музики."},
      {"t", 187, "Плагін усереднює останні інтервали між натисканнями та переводить їх у BPM."},
      {"t", 207, "Для середнього використовується до шести останніх інтервалів."},
      {"t", 227, "Якщо між натисканнями минає більше двох секунд, поточна серія починається заново."},
      {"t", 247, "Інтервали поза допустимим діапазоном ігноруються."},
      {"t", 267, "Середня або права кнопка миші на TAP BPM скидає лічильник."},
      {"h", 305, "MANUAL / SYNC"},
      {"t", 327, "Manual використовує BPM і розмір такту, введені користувачем."},
      {"t", 347, "Manual зручний для незалежних розрахунків, експериментів або роботи без DAW."},
      {"t", 367, "Sync слідує за темпом хоста. У Sync керування BPM блокується."},
      {"t", 387, "Якщо хост надає коректний розмір такту, він також використовується. Зміна темпу проєкту автоматично оновлює калькулятор."},
      {"t", 407, "Якщо хост не дає коректного темпу, калькулятор зберігає допустимий BPM."},
    },
  },
  {
    en = {
      {"h", 105, "3. TIME SIGNATURE & UNITS"},
      {"h", 145, "TIME SIGNATURE"},
      {"t", 167, "The time signature determines how many quarter-note equivalents are contained in one bar."},
      {"t", 187, "4/4 = 4 quarter-note equivalents. 3/4 = 3. 6/8 = 3. 12/8 = 6."},
      {"t", 207, "Presets: March 2/4, Waltz 3/4, Common 4/4, Five Four 5/4, Six Eight 6/8, Seven Eight 7/8, Nine Eight 9/8, Twelve Eight 12/8."},
      {"t", 227, "In Manual mode, numerator and denominator can be entered separately. Supported range: 1–16."},
      {"t", 247, "In Sync mode, the host time signature takes priority."},
      {"h", 285, "DISPLAY UNITS"},
      {"t", 307, "ms = milliseconds. This is the direct unit used by most delay and timing controls."},
      {"t", 327, "s = seconds. Useful for longer reverb tails and decay times."},
      {"t", 347, "Hz = reciprocal frequency representation of the interval. Frequency = 1000 / milliseconds."},
      {"t", 367, "Example: 500 ms = 2 Hz."},
      {"t", 387, "Changing the display unit changes how calculated time is shown; it does not change the calculation itself."},
    },
    ua = {
      {"h", 105, "3. РОЗМІР ТАКТУ ТА ОДИНИЦІ"},
      {"h", 145, "РОЗМІР ТАКТУ"},
      {"t", 167, "Розмір такту визначає кількість еквівалентів чверті в одному такті."},
      {"t", 187, "4/4 = 4 чверті. 3/4 = 3. 6/8 = 3. 12/8 = 6."},
      {"t", 207, "Пресети: March 2/4, Waltz 3/4, Common 4/4, Five Four 5/4, Six Eight 6/8, Seven Eight 7/8, Nine Eight 9/8, Twelve Eight 12/8."},
      {"t", 227, "У Manual чисельник і знаменник можна вводити окремо. Діапазон: 1–16."},
      {"t", 247, "У Sync пріоритет має розмір такту хоста."},
      {"h", 285, "ОДИНИЦІ ВІДОБРАЖЕННЯ"},
      {"t", 307, "ms = мілісекунди. Це пряме значення для більшості delay і timing-контролів."},
      {"t", 327, "s = секунди. Зручно для довгих reverb tails і decay."},
      {"t", 347, "Hz = обернене представлення частоти інтервалу. Частота = 1000 / мілісекунди."},
      {"t", 367, "Приклад: 500 мс = 2 Hz."},
      {"t", 387, "Зміна одиниці змінює лише спосіб відображення часу, а не сам розрахунок."},
    },
  },
  {
    en = {
      {"h", 105, "4. REVERB"},
      {"h", 145, "REVERB SUGGESTIONS"},
      {"t", 167, "The Reverb Suggestions tab provides tempo-based starting points for reverb design."},
      {"t", 187, "Each room type is calculated from the current BPM and time signature."},
      {"t", 207, "The result is separated into Pre-delay, Decay Time, and Total Reverb Time."},
      {"t", 227, "Pre-delay is the time between the dry signal and the beginning of the main reverb response."},
      {"t", 247, "Decay Time is the remaining calculated reverb duration after pre-delay."},
      {"t", 267, "Total Reverb Time is the complete calculated duration, including pre-delay."},
      {"h", 305, "REVERB SIZE SUGGESTIONS"},
      {"t", 327, "These are timing suggestions, not acoustic simulations of real buildings."},
      {"t", 347, "Stadium: pre-delay 1/16, total length 4 bars."},
      {"t", 367, "Cathedral: 1/32, 3 bars.  Hall: 1/32, 2 bars."},
      {"t", 387, "Large Room: 1/64, 1 bar.  Medium Room: 1/128, 0.75 bar."},
      {"t", 407, "Small Room: 1/256, 0.5 bar.  Bathroom: 1/512, 0.375 bar."},
      {"t", 427, "Tight Echo: 1/1024, 0.25 bar.  Shoebox: 1/2048, 0.125 bar."},
      {"t", 447, "These values do not model room dimensions, materials, diffusion, absorption, or early reflections."},
    },
    ua = {
      {"h", 105, "4. REVERB"},
      {"h", 145, "REVERB SUGGESTIONS"},
      {"t", 167, "Reverb Suggestions дає стартові значення для дизайну ревербу на основі темпу."},
      {"t", 187, "Кожен тип приміщення розраховується з поточного BPM і розміру такту."},
      {"t", 207, "Результат розділено на Pre-delay, Decay Time і Total Reverb Time."},
      {"t", 227, "Pre-delay — час між сухим сигналом і початком основної реверберації."},
      {"t", 247, "Decay Time — залишок розрахованої тривалості ревербу після pre-delay."},
      {"t", 267, "Total Reverb Time — повна розрахована тривалість разом із pre-delay."},
      {"h", 305, "РОЗМІРИ РЕВЕРБУ"},
      {"t", 327, "Це таймінгові рекомендації, а не акустичні симуляції реальних будівель."},
      {"t", 347, "Stadium: pre-delay 1/16, загальна довжина 4 такти."},
      {"t", 367, "Cathedral: 1/32, 3 такти. Hall: 1/32, 2 такти."},
      {"t", 387, "Large Room: 1/64, 1 такт. Medium Room: 1/128, 0.75 такту."},
      {"t", 407, "Small Room: 1/256, 0.5 такту. Bathroom: 1/512, 0.375 такту."},
      {"t", 427, "Tight Echo: 1/1024, 0.25 такту. Shoebox: 1/2048, 0.125 такту."},
      {"t", 447, "Ці значення не моделюють розміри кімнати, матеріали, дифузію, поглинання чи ранні відбиття."},
    },
  },
  {
    en = {
      {"h", 105, "5. NOTE DURATIONS"},
      {"h", 145, "NOTE VALUES"},
      {"t", 167, "Available values: 1, 1/2, 1/4, 1/8, 1/16, 1/32, 1/64, 1/128, 1/256, 1/512."},
      {"t", 187, "Normal uses the standard musical duration."},
      {"t", 207, "Dotted multiplies the normal duration by 1.5."},
      {"t", 227, "Triplet uses two thirds of the corresponding normal division."},
      {"h", 265, "EXAMPLES"},
      {"t", 287, "At 120 BPM: 1/4 = 500 ms, 1/8 = 250 ms, 1/16 = 125 ms."},
      {"t", 307, "At 120 BPM, 1/4 dotted = 750 ms."},
      {"t", 327, "1/4 triplet = two thirds of a quarter-note duration."},
      {"t", 347, "1/8 triplet = one third of a quarter-note duration."},
      {"t", 367, "Use these values for tempo-synced delay, modulation, and other time-based effects."},
    },
    ua = {
      {"h", 105, "5. ТРИВАЛОСТІ НОТ"},
      {"h", 145, "ЗНАЧЕННЯ НОТ"},
      {"t", 167, "Доступні значення: 1, 1/2, 1/4, 1/8, 1/16, 1/32, 1/64, 1/128, 1/256, 1/512."},
      {"t", 187, "Normal використовує стандартну музичну тривалість."},
      {"t", 207, "Dotted множить нормальну тривалість на 1.5."},
      {"t", 227, "Triplet використовує дві третини відповідного звичайного поділу."},
      {"h", 265, "ПРИКЛАДИ"},
      {"t", 287, "При 120 BPM: 1/4 = 500 мс, 1/8 = 250 мс, 1/16 = 125 мс."},
      {"t", 307, "При 120 BPM: 1/4 dotted = 750 мс."},
      {"t", 327, "1/4 triplet = дві третини тривалості чверті."},
      {"t", 347, "1/8 triplet = одна третина тривалості чверті."},
      {"t", 367, "Використовуйте ці значення для tempo-sync delay, modulation та інших time-based ефектів."},
    },
  },
  {
    en = {
      {"h", 105, "6. CUSTOM CONVERTER"},
      {"h", 145, "CUSTOM CONVERTER"},
      {"t", 167, "Enter an exact time in milliseconds and the calculator searches its supported rhythmic values."},
      {"t", 187, "The result shows the closest note value, calculated duration, current BPM, percentage error, exact BPM for that note, and related effect times."},
      {"t", 207, "Example: enter 375 ms to find the closest musical timing value."},
      {"h", 245, "ERROR & EXACT BPM"},
      {"t", 267, "Error shows the difference between the entered value and the closest supported note duration."},
      {"t", 287, "A positive error means the entered value is longer than the matched value; a negative error means it is shorter."},
      {"t", 307, "If the difference is essentially zero, the calculator reports an exact match."},
      {"t", 327, "Exact BPM is the tempo at which the selected note value would equal the entered millisecond value exactly."},
      {"t", 347, "Useful effect suggestions: Reverb Tail / Decay = 4× matched value; Delay Time = matched value; Ducking Release = 2× matched value."},
      {"t", 367, "These are convenience suggestions, not mandatory settings."},
    },
    ua = {
      {"h", 105, "6. CUSTOM CONVERTER"},
      {"h", 145, "CUSTOM CONVERTER"},
      {"t", 167, "Введіть точний час у мілісекундах, і калькулятор знайде найближчі підтримувані ритмічні значення."},
      {"t", 187, "Результат показує найближчу ноту, тривалість, BPM, помилку, точний BPM для ноти та пов'язані часи ефектів."},
      {"t", 207, "Приклад: введіть 375 мс, щоб знайти найближче музичне значення."},
      {"h", 245, "ПОМИЛКА ТА ТОЧНИЙ BPM"},
      {"t", 267, "Error показує різницю між введеним значенням і найближчою підтримуваною тривалістю ноти."},
      {"t", 287, "Додатна помилка означає, що введене значення довше за знайдене; від’ємна — коротше."},
      {"t", 307, "Якщо різниця практично нульова, калькулятор показує точний збіг."},
      {"t", 327, "Exact BPM — темп, при якому вибране значення ноти точно дорівнюватиме введеним мілісекундам."},
      {"t", 347, "Підказки: Reverb Tail/Decay = 4×; Delay Time = саме значення; Ducking Release = 2×."},
      {"t", 367, "Це зручні рекомендації, а не обов’язкові налаштування."},
    },
  },
  {
    en = {
      {"h", 105, "7. DELAY TAIL"},
      {"h", 145, "DELAY TAIL CALCULATOR"},
      {"t", 167, "The Delay Tail tab estimates how long a feedback delay remains audible before repeats fall below a selected threshold."},
      {"t", 187, "Inputs include delay time, feedback or target tail, threshold, and damping profile."},
      {"t", 207, "The calculation can report tail duration, repeat count, last repeat time, beats, bars, and feedback representations."},
      {"h", 245, "FEEDBACK → TAIL"},
      {"t", 267, "Use this mode when you already know delay time and feedback percentage."},
      {"t", 287, "Select the threshold that defines when the remaining repeats are considered finished."},
      {"t", 307, "The calculator estimates tail duration and repeat count."},
      {"t", 327, "Workflow: enter delay time → set feedback → choose threshold → optionally choose damping → read the estimated tail."},
    },
    ua = {
      {"h", 105, "7. DELAY TAIL"},
      {"h", 145, "КАЛЬКУЛЯТОР DELAY TAIL"},
      {"t", 167, "Вкладка Delay Tail оцінює, як довго feedback delay залишається чутним, перш ніж повтори опустяться нижче заданого threshold."},
      {"t", 187, "Входи: час delay, feedback або цільовий tail, threshold і damping profile."},
      {"t", 207, "Результат може містити тривалість хвоста, кількість повторів, час останнього повтору, долі, такти та різні представлення feedback."},
      {"h", 245, "FEEDBACK → TAIL"},
      {"t", 267, "Використовуйте цей режим, коли вже знаєте delay time і відсоток feedback."},
      {"t", 287, "Оберіть threshold, який визначає момент завершення чутного хвоста."},
      {"t", 307, "Калькулятор оцінює тривалість хвоста та кількість повторів."},
      {"t", 327, "Workflow: delay time → feedback → threshold → за потреби damping → готовий tail."},
    },
  },
  {
    en = {
      {"h", 105, "8. TAIL → FEEDBACK"},
      {"h", 145, "TAIL → FEEDBACK"},
      {"t", 167, "Use this mode when you know how long you want the delay tail to last."},
      {"t", 187, "Enter the desired target tail in seconds."},
      {"t", 207, "The calculator estimates the feedback percentage required to reach approximately that duration."},
      {"t", 227, "The result also includes estimated repeats and musical timing."},
      {"t", 247, "Conceptually: desired tail → calculated feedback, instead of feedback → resulting tail."},
      {"h", 285, "THRESHOLD"},
      {"t", 307, "Threshold defines the level at which the calculator considers the delay tail to have effectively ended."},
      {"t", 327, "Available values: -40 dB, -50 dB, -60 dB, -70 dB."},
      {"t", 347, "A lower threshold allows the tail to continue farther into the decay."},
      {"t", 367, "A higher threshold considers the tail finished earlier."},
      {"t", 387, "-60 dB is the default threshold and is described by the interface as approximately 0.1% amplitude."},
      {"t", 407, "Threshold is a practical cutoff, not absolute silence."},
    },
    ua = {
      {"h", 105, "8. TAIL → FEEDBACK"},
      {"h", 145, "TAIL → FEEDBACK"},
      {"t", 167, "Використовуйте цей режим, коли знаєте бажану тривалість хвоста."},
      {"t", 187, "Введіть цільовий tail у секундах."},
      {"t", 207, "Калькулятор оцінить feedback, необхідний приблизно для такої тривалості."},
      {"t", 227, "Результат також містить оцінку повторів і музичного таймінгу."},
      {"t", 247, "Логіка: бажаний tail → розрахований feedback, а не feedback → отриманий tail."},
      {"h", 285, "THRESHOLD"},
      {"t", 307, "Threshold визначає рівень, на якому калькулятор вважає хвіст практично завершеним."},
      {"t", 327, "Доступні значення: -40 dB, -50 dB, -60 dB, -70 dB."},
      {"t", 347, "Нижчий threshold дозволяє хвосту тривати далі."},
      {"t", 367, "Вищий threshold вважає хвіст завершеним раніше."},
      {"t", 387, "-60 dB — значення за замовчуванням; інтерфейс описує його як приблизно 0.1% амплітуди."},
      {"t", 407, "Threshold — практичний cutoff, а не абсолютна тиша."},
    },
  },
  {
    en = {
      {"h", 105, "9. DAMPING PROFILE"},
      {"h", 145, "DAMPING"},
      {"t", 167, "Ideal — ×1.00. No additional damping correction is applied."},
      {"t", 187, "Mild — ×0.85. Applies a moderate correction; the interface describes this as approximately a 15% tail reduction."},
      {"t", 207, "Strong — ×0.70. Applies a stronger correction; the interface describes this as approximately a 30% tail reduction."},
      {"h", 245, "IMPORTANT LIMITATION"},
      {"t", 267, "Damping is explicitly a heuristic correction, not a physical model."},
      {"t", 287, "It does not simulate frequency-dependent damping, filtering, diffusion, absorption or internal plugin behavior."},
      {"t", 307, "Use the profiles as practical correction factors when you want a shorter estimated tail."},
    },
    ua = {
      {"h", 105, "9. DAMPING PROFILE"},
      {"h", 145, "DAMPING"},
      {"t", 167, "Ideal — ×1.00. Додаткова корекція damping не застосовується."},
      {"t", 187, "Mild — ×0.85. Помірна корекція; інтерфейс описує її як приблизно 15% скорочення хвоста."},
      {"t", 207, "Strong — ×0.70. Сильніша корекція; інтерфейс описує її як приблизно 30% скорочення хвоста."},
      {"h", 245, "ВАЖЛИВЕ ОБМЕЖЕННЯ"},
      {"t", 267, "Damping — це евристична поправка, а не фізична модель."},
      {"t", 287, "Вона не симулює частотно-залежне damping, фільтрацію, дифузію, поглинання або поведінку конкретного процесора."},
      {"t", 307, "Використовуйте профілі як практичні коефіцієнти, коли потрібен коротший розрахунковий хвіст."},
    },
  },
  {
    en = {
      {"h", 105, "10. UNIVERSAL FEEDBACK SCALES"},
      {"h", 145, "FEEDBACK REPRESENTATIONS"},
      {"t", 167, "Standard %: conventional feedback percentage, 0–100%. Example: 50% = feedback factor 0.50."},
      {"t", 187, "Normalized: the same factor on a 0.00–1.00 scale. Example: 50% = 0.500."},
      {"t", 207, "Analog Dial: the same factor on a 0–10 scale. Example: 50% = 5.00."},
      {"t", 227, "Loop Gain (dB): the corresponding feedback gain in decibels."},
      {"h", 265, "IMPORTANT NOTE"},
      {"t", 287, "These conversions describe the same theoretical feedback factor using different scales."},
      {"t", 307, "They do not account for the individual knob taper of a specific analog emulation."},
      {"t", 327, "For nonlinear or analog-style controls, treat the value as a theoretical reference, not the exact physical knob position."},
    },
    ua = {
      {"h", 105, "10. УНІВЕРСАЛЬНІ ШКАЛИ FEEDBACK"},
      {"h", 145, "ПРЕДСТАВЛЕННЯ FEEDBACK"},
      {"t", 167, "Standard %: звичайний відсоток feedback, 0–100%. Наприклад 50% = коефіцієнт 0.50."},
      {"t", 187, "Normalized: той самий коефіцієнт на шкалі 0.00–1.00. 50% = 0.500."},
      {"t", 207, "Analog Dial: той самий коефіцієнт на шкалі 0–10. 50% = 5.00."},
      {"t", 227, "Loop Gain (dB): відповідний feedback gain у децибелах."},
      {"h", 265, "ВАЖЛИВА ПРИМІТКА"},
      {"t", 287, "Ці перетворення описують той самий теоретичний feedback-рівень різними шкалами."},
      {"t", 307, "Вони автоматично не враховують індивідуальну taper-криву ручки конкретної analog emulation."},
      {"t", 327, "Для нелінійних або analog-style контролів сприймайте значення як теоретичний орієнтир, а не як точну позицію фізичної ручки."},
    },
  },
  {
    en = {
      {"h", 105, "11. EXACT INPUT & PRACTICAL USE"},
      {"h", 145, "ENTERING EXACT VALUES"},
      {"t", 167, "BPM: click the BPM field, type a number, and press Enter."},
      {"t", 187, "Delay Tail: click the relevant value field, type the number, and press Enter."},
      {"t", 207, "Time Signature: click numerator or denominator and enter the desired value."},
      {"t", 227, "Custom Converter: open Custom and enter the millisecond value."},
      {"t", 247, "Press ESC to cancel an active edit."},
      {"h", 285, "TEMPO-SYNCED DELAY"},
      {"t", 307, "1. Enable Sync."},
      {"t", 327, "2. Make sure the DAW is running at the desired project tempo."},
      {"t", 347, "3. Open Note Durations."},
      {"t", 367, "4. Choose the desired rhythmic value."},
      {"t", 387, "5. Enter that value into the delay plugin."},
    },
    ua = {
      {"h", 105, "11. ТОЧНЕ ВВЕДЕННЯ ТА ПРАКТИЧНЕ ВИКОРИСТАННЯ"},
      {"h", 145, "ВВЕДЕННЯ ТОЧНИХ ЗНАЧЕНЬ"},
      {"t", 167, "BPM: натисніть поле BPM, введіть число та натисніть Enter."},
      {"t", 187, "Delay Tail: натисніть потрібне поле, введіть число та натисніть Enter."},
      {"t", 207, "Time Signature: натисніть чисельник або знаменник і введіть значення."},
      {"t", 227, "Custom Converter: відкрийте Custom і введіть значення в мс."},
      {"t", 247, "ESC скасовує активне редагування."},
      {"h", 285, "DELAY СИНХРОНІЗОВАНИЙ З ТЕМПОМ"},
      {"t", 307, "1. Увімкніть Sync."},
      {"t", 327, "2. Переконайтеся, що DAW працює з потрібним темпом."},
      {"t", 347, "3. Відкрийте Note Durations."},
      {"t", 367, "4. Виберіть потрібне ритмічне значення."},
      {"t", 387, "5. Введіть це значення у delay-плагін."},
    },
  },
  {
    en = {
      {"h", 105, "12. PRACTICAL WORKFLOWS"},
      {"h", 145, "MANUAL DELAY"},
      {"t", 167, "1. Select Manual."},
      {"t", 187, "2. Enter the BPM."},
      {"t", 207, "3. Choose the required note value."},
      {"t", 227, "4. Read the result in milliseconds."},
      {"t", 247, "5. Enter the value into your delay plugin."},
      {"h", 285, "TEMPO-BASED REVERB"},
      {"t", 307, "1. Set the correct BPM."},
      {"t", 327, "2. Set the correct time signature."},
      {"t", 347, "3. Open Reverb Suggestions."},
      {"t", 367, "4. Choose a room-size starting point."},
      {"t", 387, "5. Use the calculated pre-delay and decay as starting values."},
      {"t", 407, "6. Adjust the final reverb by ear."},
    },
    ua = {
      {"h", 105, "12. ПРАКТИЧНІ WORKFLOW"},
      {"h", 145, "MANUAL DELAY"},
      {"t", 167, "1. Виберіть Manual."},
      {"t", 187, "2. Введіть BPM."},
      {"t", 207, "3. Виберіть потрібне значення ноти."},
      {"t", 227, "4. Прочитайте результат у мілісекундах."},
      {"t", 247, "5. Введіть значення у delay-плагін."},
      {"h", 285, "РЕВЕРБ НА ОСНОВІ ТЕМПУ"},
      {"t", 307, "1. Встановіть BPM."},
      {"t", 327, "2. Встановіть розмір такту."},
      {"t", 347, "3. Відкрийте Reverb Suggestions."},
      {"t", 367, "4. Виберіть стартовий розмір приміщення."},
      {"t", 387, "5. Використайте pre-delay і decay як стартові значення."},
      {"t", 407, "6. Підкоригуйте фінальний reverb на слух."},
    },
  },
  {
    en = {
      {"h", 105, "13. DESIGNING A DELAY TAIL"},
      {"h", 145, "WORKFLOW"},
      {"t", 167, "1. Open Delay Tail."},
      {"t", 187, "2. Enter your delay time."},
      {"t", 207, "3. Choose Feedback → Tail if feedback is already known."},
      {"t", 227, "4. Choose Tail → Feedback if the desired tail length is already known."},
      {"t", 247, "5. Select the threshold that represents the desired end point."},
      {"t", 267, "6. Use damping only as a practical correction."},
      {"h", 305, "WHAT TO EXPECT"},
      {"t", 327, "The result is an estimate based on exponential decay and the selected threshold."},
      {"t", 347, "Actual audio can differ because real processors may have filters, modulation, saturation or other processing in the loop."},
    },
    ua = {
      {"h", 105, "13. НАЛАШТУВАННЯ DELAY TAIL"},
      {"h", 145, "WORKFLOW"},
      {"t", 167, "1. Відкрийте Delay Tail."},
      {"t", 187, "2. Введіть delay time."},
      {"t", 207, "3. Feedback → Tail, якщо feedback вже відомий."},
      {"t", 227, "4. Tail → Feedback, якщо відома бажана довжина хвоста."},
      {"t", 247, "5. Виберіть threshold, який відповідає бажаній точці завершення."},
      {"t", 267, "6. Використовуйте damping лише як практичну поправку."},
      {"h", 305, "ЧОГО ОЧІКУВАТИ"},
      {"t", 327, "Результат є оцінкою на основі експоненційного затухання та вибраного threshold."},
      {"t", 347, "Реальний звук може відрізнятися, бо процесор може мати фільтри, modulation, saturation, nonlinearities або іншу обробку у feedback loop."},
    },
  },
  {
    en = {
      {"h", 105, "14. WHAT THE CALCULATOR DOES NOT SIMULATE"},
      {"h", 145, "LIMITATIONS"},
      {"t", 167, "The calculator is a timing and estimation utility. It does not reproduce the complete internal behavior of a specific delay or reverb plugin."},
      {"t", 187, "Delay Tail does not model frequency-dependent feedback, filters in the loop, modulation, saturation or nonlinear processing."},
      {"t", 207, "Reverb Suggestions do not simulate room acoustics, absorption, diffusion, dimensions, materials or early reflections."},
      {"h", 245, "INTERPRETATION"},
      {"t", 267, "Calculated values are musical starting points and estimates, not guaranteed measurements of a specific processor."},
      {"t", 287, "Always compare the calculated result with the actual effect and adjust for musical context."},
    },
    ua = {
      {"h", 105, "14. ЧОГО КАЛЬКУЛЯТОР НЕ СИМУЛЮЄ"},
      {"h", 145, "ОБМЕЖЕННЯ"},
      {"t", 167, "Калькулятор є утилітою для таймінгу та оцінки. Він не відтворює всю внутрішню поведінку конкретного delay або reverb плагіна."},
      {"t", 187, "Delay Tail не моделює частотно-залежний feedback, фільтри в петлі, modulation, saturation або nonlinear processing."},
      {"t", 207, "Reverb Suggestions не симулює акустику кімнати, поглинання, дифузію, розміри, матеріали чи ранні відбиття."},
      {"h", 245, "ІНТЕРПРЕТАЦІЯ"},
      {"t", 267, "Розраховані значення — це музичні стартові точки та оцінки, а не гарантовані вимірювання конкретного процесора."},
      {"t", 287, "Завжди порівнюйте результат із реальним ефектом і коригуйте під музичний контекст."},
    },
  },
  {
    en = {
      {"h", 105, "15. THEMES & HELP"},
      {"h", 145, "COLOR THEMES"},
      {"t", 167, "The interface provides four visual themes: Charcoal, Emerald, Graphite, and Ocean."},
      {"t", 187, "Click the theme button in the top-right area to cycle through the available themes."},
      {"t", 207, "Changing the theme changes the visual appearance without changing calculations."},
      {"h", 245, "HELP WINDOW"},
      {"t", 267, "Press ? Help to open the built-in manual."},
      {"t", 287, "The Help window contains English and Ukrainian versions of the manual."},
      {"t", 307, "Use Prev / Next or the mouse wheel to change pages."},
      {"t", 327, "Press Close, click outside the Help window, or press ESC to exit."},
    },
    ua = {
      {"h", 105, "15. ТЕМИ ТА HELP"},
      {"h", 145, "КОЛЬОРОВІ ТЕМИ"},
      {"t", 167, "Інтерфейс має чотири теми: Charcoal, Emerald, Graphite та Ocean."},
      {"t", 187, "Натискайте кнопку теми у верхньому правому куті для перемикання."},
      {"t", 207, "Зміна теми змінює лише вигляд і не впливає на розрахунки."},
      {"h", 245, "ВІКНО HELP"},
      {"t", 267, "Натисніть ? Help, щоб відкрити вбудований мануал."},
      {"t", 287, "У Help є повні версії українською та англійською."},
      {"t", 307, "Prev / Next або коліщатко миші перемикають сторінки."},
      {"t", 327, "Close, клік поза вікном або ESC закривають Help."},
    },
  },
  {
    en = {
      {"h", 105, "16. QUICK REFERENCE"},
      {"h", 145, "TEMPO & UNITS"},
      {"t", 167, "Manual — use your own BPM."},
      {"t", 187, "Sync — follow the host tempo and valid host time signature."},
      {"t", 207, "ms — milliseconds.  s — seconds.  Hz — reciprocal frequency representation."},
      {"h", 245, "TABS & TOOLS"},
      {"t", 267, "Reverb Suggestions — pre-delay, decay, and total reverb timing."},
      {"t", 287, "Note Durations — normal, dotted, and triplet note values."},
      {"t", 307, "Delay Tail — feedback, tail length, threshold, damping, and repeat estimates."},
      {"t", 327, "TAP BPM — calculate BPM from repeated clicks."},
      {"t", 347, "Custom — find the closest musical value for an exact millisecond time."},
      {"t", 367, "Time Signature — set the meter used for bar calculations."},
    },
    ua = {
      {"h", 105, "16. ШВИДКА ДОВІДКА"},
      {"h", 145, "ТЕМП І ОДИНИЦІ"},
      {"t", 167, "Manual — використовуйте власний BPM."},
      {"t", 187, "Sync — слідуйте за темпом хоста та коректним host time signature."},
      {"t", 207, "ms — мілісекунди. s — секунди. Hz — обернене представлення частоти."},
      {"h", 245, "ВКЛАДКИ ТА ІНСТРУМЕНТИ"},
      {"t", 267, "Reverb Suggestions — pre-delay, decay та total reverb timing."},
      {"t", 287, "Note Durations — normal, dotted і triplet значення нот."},
      {"t", 307, "Delay Tail — feedback, довжина хвоста, threshold, damping та оцінка повторів."},
      {"t", 327, "TAP BPM — визначення BPM за повторними натисканнями."},
      {"t", 347, "Custom — пошук найближчого музичного значення для точного часу в мс."},
      {"t", 367, "Time Signature — розмір такту для розрахунків у тактах."},
    },
  },
  {
    en = {
      {"h", 105, "17. RECOMMENDED WORKFLOW"},
      {"h", 145, "START HERE"},
      {"t", 167, "1. Set Tempo."},
      {"t", 187, "2. Set Time Signature."},
      {"t", 207, "3. Set Display Unit."},
      {"t", 227, "4. Choose the calculation section you need."},
      {"h", 265, "DAW vs STANDALONE"},
      {"t", 287, "For DAW work, Sync mode follows the project tempo and time signature from the host."},
      {"t", 307, "For standalone calculations or experiments, use Manual mode."},
      {"t", 327, "Calculated times are starting points. Adjust the final setting for musical context and the specific effect processor."},
    },
    ua = {
      {"h", 105, "17. РЕКОМЕНДОВАНИЙ WORKFLOW"},
      {"h", 145, "ПОЧАТОК"},
      {"t", 167, "1. Встановіть Tempo."},
      {"t", 187, "2. Встановіть Time Signature."},
      {"t", 207, "3. Встановіть Display Unit."},
      {"t", 227, "4. Виберіть потрібний розділ калькулятора."},
      {"h", 265, "DAW vs STANDALONE"},
      {"t", 287, "Для DAW роботи Sync слідує за темпом і розміром такту проєкту, якщо хост їх надає."},
      {"t", 307, "Для standalone-розрахунків або експериментів використовуйте Manual."},
      {"t", 327, "Розраховані часи — стартові значення. Фінальне налаштування залежить від музичного контексту та конкретного процесора."},
    },
  },
  {
    en = {
      {"h", 105, "18. FINAL NOTES"},
      {"h", 145, "USE THE RESULTS AS STARTING POINTS"},
      {"t", 167, "The calculator is designed to turn musical timing into practical numbers quickly."},
      {"t", 187, "A mathematically synchronized value is not automatically the best-sounding value for every mix."},
      {"t", 207, "Use the calculated result, then listen to the actual processor and make the final adjustment."},
      {"h", 245, "MANUAL SUMMARY"},
      {"t", 267, "Set BPM → set time signature → choose the required calculator → read the result → apply it to the effect → adjust by ear."},
      {"t", 287, "For host-synced projects, enable Sync so the calculator follows the DAW when valid host information is available."},
    },
    ua = {
      {"h", 105, "18. ФІНАЛЬНІ ПРИМІТКИ"},
      {"h", 145, "ВИКОРИСТОВУЙТЕ ЯК СТАРТОВІ ЗНАЧЕННЯ"},
      {"t", 167, "Калькулятор швидко перетворює музичний таймінг у практичні числа."},
      {"t", 187, "Математично синхронізоване значення не обов’язково є найкращим за звучанням для кожного міксу."},
      {"t", 207, "Використайте результат, послухайте реальний процесор і зробіть фінальну корекцію."},
      {"h", 245, "КОРОТКИЙ ПІДСУМОК"},
      {"t", 267, "Встановіть BPM → розмір такту → виберіть калькулятор → прочитайте результат → застосуйте до ефекту → підкоригуйте на слух."},
      {"t", 287, "Для host-synced проєктів увімкніть Sync, щоб калькулятор слідував за DAW, коли хост надає коректну інформацію."},
    },
  },
  {
    en = {
      {"h", 105, "19. VERSION / SCOPE"},
      {"h", 145, "CURRENT HELP SCOPE"},
      {"t", 167, "This built-in manual documents the controls and calculation concepts exposed by the current plugin interface."},
      {"t", 187, "The calculations are intended for timing, musical synchronization, and practical estimation."},
      {"h", 225, "REFERENCE"},
      {"t", 247, "The exact behavior of a third-party delay, reverb, modulation, or analog emulation may differ from the theoretical values shown here."},
      {"t", 267, "When in doubt, verify the result against the actual processor and the project context."},
    },
    ua = {
      {"h", 105, "19. ОБСЯГ І ПРИЗНАЧЕННЯ"},
      {"h", 145, "ОБСЯГ ЦЬОГО HELP"},
      {"t", 167, "Цей вбудований мануал описує контролі та концепції розрахунків, доступні у поточному інтерфейсі плагіна."},
      {"t", 187, "Розрахунки призначені для таймінгу, музичної синхронізації та практичної оцінки."},
      {"h", 225, "ПРИМІТКА"},
      {"t", 247, "Точна поведінка стороннього delay, reverb, modulation або analog emulation може відрізнятися від теоретичних значень."},
      {"t", 267, "Якщо є сумнів, перевірте результат на реальному процесорі та в контексті проєкту."},
    },
  },
  {
    en = {
      {"h", 105, "20. QUICK START"},
      {"h", 145, "FASTEST WORKFLOW"},
      {"t", 167, "1. Enable Sync if working in a DAW."},
      {"t", 187, "2. Confirm BPM and time signature."},
      {"t", 207, "3. Open Note Durations for a tempo-synced delay."},
      {"t", 227, "4. Open Reverb Suggestions for a starting reverb timing."},
      {"t", 247, "5. Open Delay Tail when you need a feedback/tail estimate."},
      {"t", 267, "6. Use Custom when you already know an exact millisecond value."},
      {"h", 305, "DONE"},
      {"t", 327, "The calculator is ready to provide the timing reference."},
      {"t", 347, "Use the navigation below to return to any section of the manual."},
    },
    ua = {
      {"h", 105, "20. ШВИДКИЙ СТАРТ"},
      {"h", 145, "НАЙШВИДШИЙ WORKFLOW"},
      {"t", 167, "1. Увімкніть Sync, якщо працюєте в DAW."},
      {"t", 187, "2. Перевірте BPM і розмір такту."},
      {"t", 207, "3. Відкрийте Note Durations для tempo-synced delay."},
      {"t", 227, "4. Відкрийте Reverb Suggestions для стартового таймінгу ревербу."},
      {"t", 247, "5. Відкрийте Delay Tail для оцінки feedback/tail."},
      {"t", 267, "6. Використовуйте Custom, якщо вже знаєте точний час у мс."},
      {"h", 305, "ГОТОВО"},
      {"t", 327, "Калькулятор готовий надати таймінговий орієнтир."},
      {"t", 347, "Використовуйте навігацію нижче, щоб повернутися до будь-якого розділу мануалу."},
    },
  },
  {
    en = {
      {"h", 105, "21. KEYBOARD & NAVIGATION"},
      {"h", 145, "KEYBOARD"},
      {"t", 167, "Enter confirms a numeric value."},
      {"t", 187, "Backspace deletes the last digit while editing."},
      {"t", 207, "ESC cancels an active edit or closes the Help window."},
      {"h", 245, "HELP NAVIGATION"},
      {"t", 267, "Prev and Next move between manual pages."},
      {"t", 287, "The mouse wheel also changes Help pages."},
      {"t", 307, "The EN | UA button switches the language without changing calculations."},
      {"t", 327, "Close, outside click, or ESC exits Help."},
    },
    ua = {
      {"h", 105, "21. КЛАВІАТУРА ТА НАВІГАЦІЯ"},
      {"h", 145, "КЛАВІАТУРА"},
      {"t", 167, "Enter підтверджує числове значення."},
      {"t", 187, "Backspace видаляє останню цифру під час редагування."},
      {"t", 207, "ESC скасовує активне редагування або закриває Help."},
      {"h", 245, "НАВІГАЦІЯ HELP"},
      {"t", 267, "Prev і Next перемикають сторінки мануалу."},
      {"t", 287, "Коліщатко миші також перемикає сторінки Help."},
      {"t", 307, "Кнопка EN | UA перемикає мову без зміни розрахунків."},
      {"t", 327, "Close, клік поза вікном або ESC закривають Help."},
    },
  },
  {
    en = {
      {"h", 105, "22. COMPLETE SUMMARY"},
      {"h", 145, "WHAT THIS PLUGIN PROVIDES"},
      {"t", 167, "Tempo-based delay and reverb timing calculations."},
      {"t", 187, "Manual or host-synchronized BPM and time signature."},
      {"t", 207, "Normal, dotted, and triplet note values."},
      {"t", 227, "Custom millisecond-to-note conversion."},
      {"t", 247, "Reverb timing suggestions for multiple room-size categories."},
      {"t", 267, "Delay feedback/tail estimation with threshold and heuristic damping."},
      {"t", 287, "Multiple feedback representations and visual themes."},
      {"h", 325, "FINAL REMINDER"},
      {"t", 347, "Use the calculator as a musical timing reference and estimation tool."},
      {"t", 367, "The final audio result depends on the actual effect processor, its settings, and the musical context."},
    },
    ua = {
      {"h", 105, "22. ПОВНИЙ ПІДСУМОК"},
      {"h", 145, "ЩО ДАЄ ЦЕЙ ПЛАГІН"},
      {"t", 167, "Розрахунок delay/reverb таймінгу на основі темпу."},
      {"t", 187, "Manual або host-synced BPM і time signature."},
      {"t", 207, "Normal, dotted та triplet значення нот."},
      {"t", 227, "Конвертація точних мілісекунд у найближче музичне значення."},
      {"t", 247, "Таймінгові рекомендації для ревербу з кількома категоріями розміру."},
      {"t", 267, "Оцінка feedback/tail із threshold та евристичним damping."},
      {"t", 287, "Кілька представлень feedback і візуальні теми."},
      {"h", 325, "ФІНАЛЬНЕ НАГАДУВАННЯ"},
      {"t", 347, "Використовуйте калькулятор як музичний таймінговий орієнтир та інструмент оцінки."},
      {"t", 367, "Фінальний результат залежить від реального процесора, його налаштувань і музичного контексту."},
    },
  },
}


-- ----------------------------------------------------------------------------
--  State
-- ----------------------------------------------------------------------------
base_w, base_h = 785, 670
scale, scale_x, scale_y, offset_x, offset_y = 1, 1, 1, 0, 0
mx, my = 0, 0

bpm, manual_bpm = 120, 120
unit, theme, active_tab = 0, 0, 0
sync_mode = false
edit_bpm = false
scroll_y = 0
show_ts_menu, show_custom_menu, show_help = false, false, false
help_lang, help_scroll = 0, 0
cur_ts_num, cur_ts_den = 4, 4
manual_ts_num, manual_ts_den = 4, 4
edit_ts = 0

fb_mode, fb_damping_mode = 0, 0
fb_delay_ms, fb_feedback_pct, fb_tail_target = 375, 50, 3.0
fb_threshold, fb_threshold_idx = -60, 2
fb_edit_field = 0
fb_calc_tail, fb_calc_repeats, fb_calc_feedback, fb_db_val = 0, 0, 0, 0
tail_beats, tail_bars, last_rep = 0, 0, 0

bpm_string, fb_buf, custom_string = "", "", ""
ts_num_str, ts_den_str = "", ""
unit_text = "ms"

tap_i, tap_n, tap_last = {}, 0, 0
last_cap = 0
fbn_best_row, fbn_best_col, fbn_best_ms, fbn_best_diff, fbn_best_mult = 0, 0, 0, 0, 0

hover = {}

local THEMES = {
  { name = "Charcoal", bg = {0.180,0.190,0.200}, panel = {0.120,0.130,0.140}, line = {0.280,0.300,0.320},
    text = {0.900,0.920,0.930}, muted = {0.560,0.590,0.610}, accent = {0.380,0.820,0.680}, hi = {0.560,0.960,0.830} },
  { name = "Emerald", bg = {0.025,0.040,0.033}, panel = {0.078,0.105,0.092}, line = {0.160,0.220,0.190},
    text = {0.840,0.880,0.850}, muted = {0.460,0.520,0.490}, accent = {0.450,0.780,0.570}, hi = {0.620,0.920,0.700} },
  { name = "Graphite", bg = {0.030,0.033,0.045}, panel = {0.090,0.100,0.120}, line = {0.170,0.190,0.230},
    text = {0.850,0.880,0.920}, muted = {0.480,0.510,0.560}, accent = {0.380,0.650,0.980}, hi = {0.580,0.800,1.000} },
  { name = "Ocean",    bg = {0.015,0.035,0.065}, panel = {0.035,0.075,0.130}, line = {0.090,0.170,0.280},
    text = {0.820,0.900,0.950}, muted = {0.460,0.580,0.660}, accent = {0.280,0.680,0.950}, hi = {0.480,0.830,1.000} },
}
local T = THEMES[1]
local function apply_theme() T = THEMES[theme + 1] or THEMES[1] end

-- Ko-fi QR (25x25)
local qr_size = 25
local qr_rows = {
  0x1FD267F,0x1055141,0x175D65D,0x175CD5D,0x174AB5D,0x105C141,0x1FD557F,0x0007000,0x19C422F,0x082271A,
  0x1B6F27C,0x1099556,0x1D6306F,0x1096B12,0x03E777C,0x06A8036,0x1B443FC,0x0016D10,0x1FC3150,0x105271F,
  0x17531FE,0x174E6E7,0x174F6CA,0x105223E,0x1FDF207,
}

-- ----------------------------------------------------------------------------
--  Persistence
-- ----------------------------------------------------------------------------
local function clamp(v, lo, hi) if v < lo then return lo elseif v > hi then return hi end return v end

local function load_state()
  local function get(k, def)
    local v = tonumber(r.GetExtState(EXT, k))
    if v == nil then return def end
    return v
  end
  manual_bpm      = clamp(get("bpm", 120), 20, 300)
  unit            = clamp(floor(get("unit", 0)), 0, 2)
  sync_mode       = get("sync", 0) == 1
  theme           = clamp(floor(get("theme", 0)), 0, 3)
  active_tab      = clamp(floor(get("tab", 0)), 0, 2)
  fb_threshold_idx= clamp(floor(get("thr", 2)), 0, 3)
  fb_damping_mode = clamp(floor(get("damp", 0)), 0, 2)
  manual_ts_num   = clamp(floor(get("tsn", 4)), 1, 16)
  manual_ts_den   = clamp(floor(get("tsd", 4)), 1, 16)
  fb_delay_ms     = clamp(get("fbd", 375), 1, 10000)
  fb_feedback_pct = clamp(get("fbp", 50), 0.1, 99.9)
  fb_tail_target  = clamp(get("fbt", 3.0), 0.01, 600)
  fb_mode         = clamp(floor(get("fbm", 0)), 0, 1)
  local thr = { -40, -50, -60, -70 }
  fb_threshold = thr[fb_threshold_idx + 1]
  bpm = manual_bpm
  cur_ts_num, cur_ts_den = manual_ts_num, manual_ts_den
  apply_theme()
end

local function save_state()
  local function set(k, v) r.SetExtState(EXT, k, tostring(v), true) end
  set("bpm", manual_bpm); set("unit", unit); set("sync", sync_mode and 1 or 0)
  set("theme", theme); set("tab", active_tab); set("thr", fb_threshold_idx)
  set("damp", fb_damping_mode); set("tsn", manual_ts_num); set("tsd", manual_ts_den)
  set("fbd", fb_delay_ms); set("fbp", fb_feedback_pct); set("fbt", fb_tail_target); set("fbm", fb_mode)
  local dock, x, y, w, h = gfx.dock(-1)
  if dock then
    set("dock", dock); set("wx", x); set("wy", y); set("ww", w); set("wh", h)
  end
end

-- ----------------------------------------------------------------------------
--  Drawing helpers (all coordinates in the 785x670 design space)
-- ----------------------------------------------------------------------------
-- The layout is stretched independently on both axes (scale_x / scale_y), so the
-- interface always fills the whole window. `scale` (the smaller of the two) is used
-- for fonts, corner radii and other things that must keep their proportions.
local function Xp(x) return floor(x * scale_x) end
local function Yp(y) return floor(y * scale_y) end
local function Sp(v) return floor(v * scale) end
-- rectangle in design coordinates. Edges are rounded as edges (not as sizes), so
-- neighbouring cells never leave hairline gaps. wpx / hpx = fixed size in pixels.
local function grect(x, y, w, h, fill, wpx, hpx)
  local x1, y1 = floor(x * scale_x), floor(y * scale_y)
  local pw = wpx or (floor((x + w) * scale_x) - x1)
  local ph = hpx or (floor((y + h) * scale_y) - y1)
  gfx.rect(x1, y1, pw, ph, fill)
end

local function setc(c, m, a)
  m = m or 1
  gfx.r = math.min(1, c[1] * m); gfx.g = math.min(1, c[2] * m); gfx.b = math.min(1, c[3] * m)
  gfx.a = a or 1
end
local function setrgb(rr, gg, bb, a) gfx.r, gfx.g, gfx.b, gfx.a = rr, gg, bb, a or 1 end
local function frect(x, y, w, h) grect(x, y, w, h, 1) end

local function set_font(size)
  local sz = floor(size * scale)
  if sz < 8 then sz = 8 end
  gfx.setfont(1, "Arial", sz)
end

local function draw_rrect(x, y, w, h, rad)
  local x2, y2 = Xp(x + w), Yp(y + h)
  x = Xp(x); y = Yp(y); w = x2 - x; h = y2 - y; rad = Sp(rad)
  local mr = floor(math.min(w, h) / 2)
  if rad > mr then rad = mr end
  if rad < 1 then
    gfx.rect(x, y, w, h, 1)
  else
    gfx.rect(x + rad, y, w - 2 * rad, h, 1)
    gfx.rect(x, y + rad, rad, h - 2 * rad, 1)
    gfx.rect(x + w - rad, y + rad, rad, h - 2 * rad, 1)
    gfx.circle(x + rad, y + rad, rad, 1)
    gfx.circle(x + w - rad - 1, y + rad, rad, 1)
    gfx.circle(x + rad, y + h - rad - 1, rad, 1)
    gfx.circle(x + w - rad - 1, y + h - rad - 1, rad, 1)
  end
end

local function draw_bevel(x, y, w, h, inset)
  local ix, iw = x + inset, w - 2 * inset
  setrgb(1, 1, 1, 0.12)
  grect(ix, y + 1, iw, 0, 1, nil, 1)
  setrgb(0, 0, 0, 0.30)
  grect(ix, y + h - 2, iw, 0, 1, nil, 1)
  gfx.a = 1
end

local function panel(x, y, w, h)
  setrgb(0, 0, 0, 0.10); draw_rrect(x, y + 3, w, h, 10)
  gfx.a = 0.05;          draw_rrect(x - 1, y + 5, w + 2, h + 2, 12)
  setc(T.panel); draw_rrect(x, y, w, h, 10)
  setrgb(1, 1, 1, 0.10); grect(x + 12, y + 1, w - 24, 0, 1, nil, 1)
  setrgb(0, 0, 0, 0.18); grect(x + 12, y + h - 1, w - 24, 0, 1, nil, 1)
  gfx.a = 1
end

local function text_left(x, y, txt, size)
  setc(T.text); set_font(size)
  gfx.x = Xp(x); gfx.y = Yp(y)
  gfx.drawstr(txt)
end

local function text_muted(x, y, txt, size)
  setc(T.muted); set_font(size)
  gfx.x = Xp(x); gfx.y = Yp(y)
  gfx.drawstr(txt)
end

local function text_center(x, y, w, h, txt, size)
  gfx.a = 1; set_font(size)
  local tw, th = gfx.measurestr(txt)
  setc(T.text)
  gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
  gfx.drawstr(txt)
end

local function text_center_c(x, y, w, h, txt, size, cr, cg, cb)
  gfx.a = 1; set_font(size)
  local tw, th = gfx.measurestr(txt)
  setrgb(cr, cg, cb, 1)
  gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
  gfx.drawstr(txt)
end
local function text_center_t(x, y, w, h, txt, size, c) text_center_c(x, y, w, h, txt, size, c[1], c[2], c[3]) end
local DARK = {0.05, 0.05, 0.05}

local function format_time(value)
  if unit == 0 then return string.format("%.2f", value)
  elseif unit == 1 then return string.format("%.3f", value / 1000)
  else
    if value > 0 then return string.format("%.3f", 1000 / value) end
    return "0.000"
  end
end

local function unit_name()
  if unit == 0 then unit_text = "ms" elseif unit == 1 then unit_text = "s" else unit_text = "Hz" end
end

local function draw_kofi_qr(start_x, start_y, module_size)
  local quiet = 4
  local total = qr_size + quiet * 2
  local px = module_size * scale          -- modules stay square at any window shape
  local ox, oy = Xp(start_x), Yp(start_y)
  setrgb(1, 1, 1, 1)
  local q = floor(quiet * px)
  gfx.rect(ox - q, oy - q, floor(total * px) + 2, floor(total * px) + 2, 1)
  setrgb(0, 0, 0, 1)
  for row = 0, qr_size - 1 do
    local mask = qr_rows[row + 1]
    for c = 0, qr_size - 1 do
      if ((mask >> (qr_size - 1 - c)) & 1) == 1 then
        local x0, y0 = ox + floor(c * px), oy + floor(row * px)
        gfx.rect(x0, y0, ox + floor((c + 1) * px) - x0 + 1, oy + floor((row + 1) * px) - y0 + 1, 1)
      end
    end
  end
end

local function cell_box(x, y, w, h, hl, mul)
  setc(T.panel, mul or 0.7)
  frect(x, y, w, h)
  if hl then setc(T.line); frect(x, y, w, h) end
  setc(T.line)
  grect(x, y, w, h, 0)
end

local function cell_label(x, y, w, h, txt, hl)
  cell_box(x, y, w, h, hl)
  set_font(17); setc(T.text)
  local tw, th = gfx.measurestr(txt)
  gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
  gfx.drawstr(txt)
end

local function cell_value(x, y, w, h, val_str, unit_str, hl)
  cell_box(x, y, w, h, hl)
  if #unit_str > 0 then
    set_font(17); local tw, th = gfx.measurestr(val_str)
    set_font(12); local tw_u, th_u = gfx.measurestr(unit_str)
    local gap = 5
    local total_w = tw + gap * scale + tw_u
    local sx = floor(offset_x + (x + w / 2) * scale_x - total_w / 2)
    local val_y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
    set_font(17); setc(T.text)
    gfx.x = sx; gfx.y = val_y; gfx.drawstr(val_str)
    set_font(12); setc(T.muted)
    gfx.x = floor(sx + tw + gap * scale); gfx.y = val_y + (th - th_u)
    gfx.drawstr(unit_str)
  else
    set_font(17); setc(T.text)
    local tw, th = gfx.measurestr(val_str)
    gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
    gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
    gfx.drawstr(val_str)
  end
end

local function header_cell(x, y, w, h, txt)
  setc(T.panel, 1.3); frect(x, y, w, h)
  setc(T.line); grect(x, y, w, h, 0)
  set_font(14); setc(T.text)
  local tw, th = gfx.measurestr(txt)
  gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
  gfx.drawstr(txt)
end

local function note_row(y, label, normal, dotted, triplet, zebra)
  local n1, n2, n3 = format_time(normal), format_time(dotted), format_time(triplet)
  unit_name()
  cell_label(15, y, 125, 28, label, zebra)
  cell_value(140, y, 210, 28, n1, unit_text, zebra)
  cell_value(350, y, 210, 28, n2, unit_text, zebra)
  cell_value(560, y, 210, 28, n3, unit_text, zebra)
end

local function reverb_row(y, name, pre_factor, bars, zebra)
  local quarter = 60000 / bpm
  local bar_q = cur_ts_num * 4 / cur_ts_den
  local bar_ms = quarter * bar_q
  local total = bar_ms * bars
  local pre = quarter * (pre_factor * 4)
  local decay = total - pre
  local r1, r2, r3 = format_time(pre), format_time(decay), format_time(total)
  unit_name()

  local bars_label
  if bars == 4 then bars_label = "4 bars" elseif bars == 3 then bars_label = "3 bars"
  elseif bars == 2 then bars_label = "2 bars" elseif bars == 1 then bars_label = "1 bar"
  elseif bars == 0.75 then bars_label = "3/4 bar" elseif bars == 0.5 then bars_label = "1/2 bar"
  elseif bars == 0.375 then bars_label = "3/8 bar" elseif bars == 0.25 then bars_label = "1/4 bar"
  else bars_label = "1/8 bar" end

  local cell_x, cell_w, cell_h = 15, 200, 30
  cell_box(cell_x, y, cell_w, cell_h, zebra)

  set_font(17); setc(T.text)
  local tw_name, th_name = gfx.measurestr(name)
  gfx.x = floor(offset_x + (cell_x + 10) * scale_x)
  gfx.y = floor(offset_y + (y + cell_h / 2) * scale_y - th_name / 2)
  gfx.drawstr(name)

  set_font(12); setc(T.muted)
  local tw_bars, th_bars = gfx.measurestr(bars_label)
  gfx.x = floor(offset_x + (cell_x + cell_w - 10) * scale_x - tw_bars)
  gfx.y = floor(offset_y + (y + cell_h / 2) * scale_y - th_bars / 2)
  gfx.drawstr(bars_label)

  cell_value(215, y, 175, cell_h, r1, unit_text, zebra)
  cell_value(390, y, 190, cell_h, r2, unit_text, zebra)
  cell_value(580, y, 190, cell_h, r3, unit_text, zebra)
end

local function unit_button(x, y, w, h, is_active, is_hover, label)
  gfx.a = 1
  if is_active then
    setc(T.accent); draw_rrect(x, y, w, h, 8)
    setc(T.hi, 1, 0.35)
    grect(x + 6, y + 2, w - 12, 0, 1, nil, 1)
    gfx.a = 1
  else
    if is_hover then setc(T.line) else setc(T.panel, 1.5) end
    draw_rrect(x, y, w, h, 8)
    draw_bevel(x, y, w, h, 6)
  end
  set_font(17)
  local tw, th = gfx.measurestr(label)
  if is_active then setrgb(0.05, 0.05, 0.05, 1) else setc(T.text) end
  gfx.x = floor(offset_x + (x + w / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (y + h / 2) * scale_y - th / 2)
  gfx.drawstr(label)
end

local function ts_preset_btn(x, y, w, h, pnum, pden, pname, is_active)
  gfx.a = 1
  local info = string.format("%d/%d", pnum, pden)
  if is_active then
    setc(T.accent); draw_rrect(x, y, w, h, 6)
    text_center_t(x, y, w, h / 2, info, 18, DARK)
    text_center_t(x, y + h / 2, w, h / 2, pname, 11, DARK)
  else
    setc(T.panel, 1.5); draw_rrect(x, y, w, h, 6)
    draw_bevel(x, y, w, h, 6)
    text_center(x, y, w, h / 2, info, 18)
    text_center(x, y + h / 2, w, h / 2, pname, 11)
  end
end

-- segmented button used on the Delay Tail tab (modes / thresholds / damping)
local function seg_button(x, y, w, h, rad, active, is_hover, label, inset)
  if active then setc(T.accent) elseif is_hover then setc(T.line) else setc(T.panel, 1.3) end
  draw_rrect(x, y, w, h, rad)
  if active then
    setc(T.hi, 1, 0.35)
    grect(x + 6, y + 2, w - 12, 0, 1, nil, 1)
    gfx.a = 1
  else
    draw_bevel(x, y, w, h, inset)
  end
  if active then text_center_t(x, y, w, h, label, 12, DARK) else text_center(x, y, w, h, label, 12) end
end

-- ----------------------------------------------------------------------------
--  Calculators
-- ----------------------------------------------------------------------------
local NOTE_BASE = { 4, 2, 1, 0.5, 0.25, 0.125, 0.0625, 0.03125, 0.015625, 0.0078125 }
local NOTE_MULT = { 1, 1.5, 0.6666667 }
local NOTE_NAMES = { "1", "1/2", "1/4", "1/8", "1/16", "1/32", "1/64", "1/128", "1/256", "1/512" }
local NOTE_SUFFIX = { "", " dotted", " triplet" }

local function find_best_note(target)
  local q = 60000 / bpm
  fbn_best_diff = 9999999
  fbn_best_row, fbn_best_col, fbn_best_ms, fbn_best_mult = 0, 0, 0, 0
  for idx = 0, 29 do
    local row = floor(idx / 3)
    local col = idx % 3
    local full_mult = NOTE_BASE[row + 1] * NOTE_MULT[col + 1]
    local ms = q * full_mult
    local d = math.abs(ms - target)
    if d < fbn_best_diff then
      fbn_best_diff, fbn_best_row, fbn_best_col = d, row, col
      fbn_best_ms, fbn_best_mult = ms, full_mult
    end
  end
end

local function build_note_name(row, col)
  return NOTE_NAMES[row + 1] .. NOTE_SUFFIX[col + 1]
end

local function calc_feedback_tail()
  if fb_delay_ms < 1 then fb_delay_ms = 1 end
  if fb_feedback_pct < 0.1 then fb_feedback_pct = 0.1 end
  if fb_feedback_pct > 99.9 then fb_feedback_pct = 99.9 end
  if fb_tail_target < 0.01 then fb_tail_target = 0.01 end

  local damping = (fb_damping_mode == 0 and 1.0) or (fb_damping_mode == 1 and 0.85) or 0.70

  if fb_mode == 0 then
    local fb_factor = fb_feedback_pct / 100
    local denom = 20 * (math.log(fb_factor) / math.log(10))
    if denom ~= 0 then
      local reps_pure = fb_threshold / denom
      if reps_pure < 1 then reps_pure = 1 end
      fb_calc_repeats = reps_pure * damping
      fb_calc_tail = (fb_calc_repeats * fb_delay_ms) / 1000
      fb_calc_feedback = fb_feedback_pct
    end
  else
    local reps_target = (fb_tail_target * 1000) / fb_delay_ms
    if reps_target < 1 then reps_target = 1 end
    fb_calc_repeats = reps_target
    local reps_pure = reps_target / damping
    if reps_pure < 1 then reps_pure = 1 end
    local fb_factor_calc = 10 ^ (fb_threshold / (20 * reps_pure))
    fb_calc_feedback = fb_factor_calc * 100
    if fb_calc_feedback > 99.9 then fb_calc_feedback = 99.9 end
    fb_calc_tail = fb_tail_target
  end

  if fb_calc_feedback > 0.1 then
    fb_db_val = 20 * (math.log(fb_calc_feedback / 100) / math.log(10))
  else
    fb_db_val = -99.9
  end

  local bar_q = cur_ts_num * 4 / cur_ts_den
  tail_beats = (fb_calc_tail * bpm) / 60
  if bar_q > 0 then tail_bars = tail_beats / bar_q else tail_bars = 0 end

  last_rep = floor(fb_calc_repeats)
  if last_rep < 1 then last_rep = 1 end
end

-- host tempo / time signature (project cursor, or play position while playing)
local function read_host()
  local pos
  if (r.GetPlayState() & 1) == 1 then pos = r.GetPlayPosition() else pos = r.GetCursorPosition() end
  local a, b, c, d = r.TimeMap_GetTimeSigAtTime(0, pos)
  local num, den, tempo
  if d ~= nil then num, den, tempo = b, c, d else num, den, tempo = a, b, c end
  if not tempo or tempo <= 0 then tempo = r.Master_GetTempo() end
  return tempo, num, den
end

-- ----------------------------------------------------------------------------
--  Text input helpers
-- ----------------------------------------------------------------------------
local function blink() return (floor(r.time_precise() * 2) % 2) == 0 end

local function draw_edit_text(x, y, buf, size, cur_h)
  setc(T.accent)
  set_font(size)
  gfx.x = Xp(x); gfx.y = Yp(y)
  gfx.drawstr(buf)
  local tw = gfx.measurestr(buf)
  if blink() then
    setc(T.hi)
    gfx.rect(floor(x * scale_x + tw + 2 * scale), Yp(y), 2, Sp(cur_h), 1)
  end
end

local function append_number(buf, key, maxlen, allow_dot)
  if key >= 48 and key <= 57 then
    if #buf < maxlen then buf = buf .. string.char(key) end
  elseif key == 46 and allow_dot then
    if #buf > 0 and #buf < maxlen and not buf:find(".", 1, true) then buf = buf .. "." end
  elseif key == 8 then
    if #buf > 0 then buf = buf:sub(1, #buf - 1) end
  end
  return buf
end

local function handle_key(key)
  if show_help then
    if key == 27 then show_help = false end

  elseif show_custom_menu then
    if key == 13 then show_custom_menu = false
    elseif key == 27 then show_custom_menu = false; custom_string = ""
    else custom_string = append_number(custom_string, key, 5, true) end

  elseif show_ts_menu then
    if edit_ts > 0 and not sync_mode then
      if (key >= 48 and key <= 57) or key == 8 then
        if edit_ts == 1 then ts_num_str = append_number(ts_num_str, key, 2, false)
        else ts_den_str = append_number(ts_den_str, key, 2, false) end
      elseif key == 13 then
        if edit_ts == 1 then
          if #ts_num_str > 0 then
            local p = clamp(floor(tonumber(ts_num_str) or 1), 1, 16)
            manual_ts_num = p; cur_ts_num = p; ts_num_str = ""
          end
        elseif edit_ts == 2 then
          if #ts_den_str > 0 then
            local p = clamp(floor(tonumber(ts_den_str) or 1), 1, 16)
            manual_ts_den = p; cur_ts_den = p; ts_den_str = ""
          end
        end
        edit_ts = 0; show_ts_menu = false
      elseif key == 27 then
        edit_ts = 0; ts_num_str = ""; ts_den_str = ""; show_ts_menu = false
      end
    else
      if key == 13 or key == 27 then show_ts_menu = false end
    end

  elseif edit_bpm and not sync_mode then
    if key == 13 then
      if #bpm_string > 0 then
        local p = clamp(tonumber(bpm_string) or 120, 20, 300)
        manual_bpm = p; bpm = p
      end
      edit_bpm = false; bpm_string = ""
    elseif key == 27 then edit_bpm = false; bpm_string = ""
    else bpm_string = append_number(bpm_string, key, 6, true) end

  elseif fb_edit_field > 0 and active_tab == 2 then
    if key == 13 then
      if #fb_buf > 0 then
        local p = tonumber(fb_buf) or 0
        if fb_edit_field == 1 then fb_delay_ms = clamp(p, 1, 10000)
        elseif fb_edit_field == 2 then fb_feedback_pct = clamp(p, 0.1, 99.9)
        elseif fb_edit_field == 3 then fb_tail_target = clamp(p, 0.01, 600) end
      end
      fb_edit_field = 0; fb_buf = ""
    elseif key == 27 then fb_edit_field = 0; fb_buf = ""
    else fb_buf = append_number(fb_buf, key, 8, true) end
  end
end

-- ----------------------------------------------------------------------------
--  Tab contents
-- ----------------------------------------------------------------------------
local function draw_tab_reverb(cy)
  header_cell(15,  cy, 200, 34, "Reverb Size")
  header_cell(215, cy, 175, 34, "Pre-Delay")
  header_cell(390, cy, 190, 34, "Decay Time")
  header_cell(580, cy, 190, 34, "Reverb Time")
  reverb_row(cy + 34,  "Stadium",     1/16,   4,     false)
  reverb_row(cy + 64,  "Cathedral",   1/32,   3,     true)
  reverb_row(cy + 94,  "Hall",        1/32,   2,     false)
  reverb_row(cy + 124, "Large Room",  1/64,   1,     true)
  reverb_row(cy + 154, "Medium Room", 1/128,  0.75,  false)
  reverb_row(cy + 184, "Small Room",  1/256,  0.5,   true)
  reverb_row(cy + 214, "Bathroom",    1/512,  0.375, false)
  reverb_row(cy + 244, "Tight Echo",  1/1024, 0.25,  true)
  reverb_row(cy + 274, "Shoebox",     1/2048, 0.125, false)
end

local function draw_tab_notes(cy)
  header_cell(15,  cy, 125, 32, "Note")
  header_cell(140, cy, 210, 32, "Normal")
  header_cell(350, cy, 210, 32, "Dotted")
  header_cell(560, cy, 210, 32, "Triplet")
  unit_name()
  text_muted(230, cy + 21, unit_text, 9)
  text_muted(440, cy + 21, unit_text, 9)
  text_muted(650, cy + 21, unit_text, 9)

  local q = 60000 / bpm
  note_row(cy + 32,  "1",     q*4,        q*6,        q*4*2/3,  false)
  note_row(cy + 60,  "1/2",   q*2,        q*3,        q*2*2/3,  true)
  note_row(cy + 88,  "1/4",   q,          q*1.5,      q*2/3,    false)
  note_row(cy + 116, "1/8",   q*0.5,      q*0.75,     q/3,      true)
  note_row(cy + 144, "1/16",  q*0.25,     q*0.375,    q/6,      false)
  note_row(cy + 172, "1/32",  q*0.125,    q*0.1875,   q/12,     true)
  note_row(cy + 200, "1/64",  q*0.0625,   q*0.09375,  q/24,     false)
  note_row(cy + 228, "1/128", q*0.03125,  q*0.046875, q/48,     true)
  note_row(cy + 256, "1/256", q*0.015625, q*0.0234375,q/96,     false)
  note_row(cy + 284, "1/512", q*0.0078125,q*0.01171875,q/192,   true)
end

local function draw_tab_tail(cy)
  local function inbox(x1, x2, y1, y2) return my >= 354 and mx >= x1 and mx <= x2 and my >= y1 and my <= y2 end

  panel(15, cy + 10, 755, 55)
  text_muted(35, cy + 18, "DELAY TIME", 11)

  hover.fb_delay = inbox(35, 365, cy + 30, cy + 58)
  if fb_edit_field == 1 or hover.fb_delay then setc(T.line) else setc(T.panel, 1.3) end
  draw_rrect(35, cy + 30, 330, 26, 6)
  if fb_edit_field == 1 then
    draw_edit_text(45, cy + 34, fb_buf, 18, 20)
  else
    text_left(45, cy + 34, string.format("%.2f ms", fb_delay_ms), 18)
  end
  text_muted(400, cy + 34, "Tail = time until repeats fall below threshold", 11)

  panel(15, cy + 75, 755, 178)
  text_muted(35, cy + 85, "MODE", 11)
  hover.fb_mode0 = inbox(35, 200, cy + 98, cy + 124)
  hover.fb_mode1 = inbox(210, 375, cy + 98, cy + 124)
  seg_button(35,  cy + 98, 165, 26, 6, fb_mode == 0, hover.fb_mode0, "Feedback -> Tail", 4)
  seg_button(210, cy + 98, 165, 26, 6, fb_mode == 1, hover.fb_mode1, "Tail -> Feedback", 4)

  text_muted(35, cy + 132, "THRESHOLD", 11)
  local th_y, thr_w, thr_h = cy + 145, 90, 22
  hover.thr40 = inbox(35,  125, th_y, th_y + thr_h)
  hover.thr50 = inbox(135, 225, th_y, th_y + thr_h)
  hover.thr60 = inbox(235, 325, th_y, th_y + thr_h)
  hover.thr70 = inbox(335, 425, th_y, th_y + thr_h)
  seg_button(35,  th_y, thr_w, thr_h, 5, fb_threshold == -40, hover.thr40, "-40 dB", 4)
  seg_button(135, th_y, thr_w, thr_h, 5, fb_threshold == -50, hover.thr50, "-50 dB", 4)
  seg_button(235, th_y, thr_w, thr_h, 5, fb_threshold == -60, hover.thr60, "-60 dB", 4)
  seg_button(335, th_y, thr_w, thr_h, 5, fb_threshold == -70, hover.thr70, "-70 dB", 4)
  text_muted(450, th_y + 6, "-60 dB approx 0.1% amplitude", 11)

  text_muted(35, cy + 175, "DAMPING PROFILE", 11)
  local dp_y, dp_w, dp_h = cy + 188, 180, 22
  hover.dmp0 = inbox(35,  215, dp_y, dp_y + dp_h)
  hover.dmp1 = inbox(225, 405, dp_y, dp_y + dp_h)
  hover.dmp2 = inbox(415, 595, dp_y, dp_y + dp_h)
  seg_button(35,  dp_y, dp_w, dp_h, 5, fb_damping_mode == 0, hover.dmp0, "Ideal (x1.00)",  4)
  seg_button(225, dp_y, dp_w, dp_h, 5, fb_damping_mode == 1, hover.dmp1, "Mild (x0.85)",   4)
  seg_button(415, dp_y, dp_w, dp_h, 5, fb_damping_mode == 2, hover.dmp2, "Strong (x0.70)", 4)
  text_muted(35, cy + 218, "Mild approx -15% tail. Strong approx -30% tail.", 10)
  text_muted(35, cy + 233, "Heuristic correction, not a physical model.", 10)

  panel(15, cy + 262, 755, 140)

  local edit_id = (fb_mode == 0) and 2 or 3
  text_muted(35, cy + 272, (fb_mode == 0) and "FEEDBACK" or "TARGET TAIL (sec)", 11)
  hover.fb_input = inbox(35, 285, cy + 285, cy + 315)
  if fb_edit_field == edit_id or hover.fb_input then setc(T.line) else setc(T.panel, 1.3) end
  draw_rrect(35, cy + 285, 250, 30, 6)
  if fb_edit_field == edit_id then
    draw_edit_text(50, cy + 289, fb_buf, 20, 22)
  else
    if fb_mode == 0 then text_left(50, cy + 289, string.format("%.2f %%", fb_feedback_pct), 20)
    else text_left(50, cy + 289, string.format("%.3f sec", fb_tail_target), 20) end
  end

  if fb_mode == 0 then text_muted(320, cy + 272, string.format("TAIL TO %d dB", fb_threshold), 11)
  else text_muted(320, cy + 272, string.format("FEEDBACK TO %d dB", fb_threshold), 11) end
  setc(T.panel, 0.8); draw_rrect(320, cy + 285, 425, 30, 6)
  setc(T.hi); set_font(20); gfx.x = Xp(335); gfx.y = Yp(cy + 289)
  if fb_mode == 0 then gfx.drawstr(string.format("%.3f sec", fb_calc_tail))
  else gfx.drawstr(string.format("%.2f %%", fb_calc_feedback)) end

  text_muted(320, cy + 322, string.format("Continuous estimate: %.2f repeats", fb_calc_repeats), 12)
  text_muted(320, cy + 337, string.format("Last repeat: ~#%d at %.2f sec", last_rep, last_rep * fb_delay_ms / 1000), 12)
  text_muted(320, cy + 352, string.format("approx %.1f beats @ %.0f BPM", tail_beats, bpm), 12)
  text_muted(320, cy + 367, string.format("approx %.2f bars (%d/%d)", tail_bars, cur_ts_num, cur_ts_den), 12)

  panel(15, cy + 410, 755, 130)
  text_muted(35, cy + 420, "UNIVERSAL FEEDBACK SCALES", 11)
  text_muted(35, cy + 442, "Standard % (0-100%):", 12)
  setc(T.hi);     set_font(16); gfx.x = Xp(250); gfx.y = Yp(cy + 438); gfx.drawstr(string.format("%.2f %%", fb_calc_feedback))
  text_muted(35, cy + 462, "Normalized (0.00 - 1.00):", 12)
  setc(T.text);   set_font(16); gfx.x = Xp(250); gfx.y = Yp(cy + 458); gfx.drawstr(string.format("%.3f", fb_calc_feedback / 100))
  text_muted(35, cy + 482, "Analog Dial (0 - 10):", 12)
  setc(T.text);   set_font(16); gfx.x = Xp(250); gfx.y = Yp(cy + 478); gfx.drawstr(string.format("%.2f", fb_calc_feedback / 10))
  text_muted(35, cy + 502, "Loop Gain (dB):", 12)
  setc(T.accent); set_font(16); gfx.x = Xp(250); gfx.y = Yp(cy + 498)
  if fb_calc_feedback > 0.1 then gfx.drawstr(string.format("%.2f dB", fb_db_val)) else gfx.drawstr("-inf dB") end

  text_muted(440, cy + 438, "For analog emulations with non-", 11)
  text_muted(440, cy + 455, "linear knob tapers, use Loop Gain", 11)
  text_muted(440, cy + 472, "(dB) or Standard % as a theoretical", 11)
  text_muted(440, cy + 489, "baseline. Values above 100% =", 11)
  text_muted(440, cy + 506, "self-oscillation (not calculated).", 11)
end

-- ----------------------------------------------------------------------------
--  Overlays
-- ----------------------------------------------------------------------------
local function draw_help()
  setrgb(0, 0, 0, 0.78); gfx.rect(0, 0, gfx.w, gfx.h, 1); gfx.a = 1
  panel(15, 20, 755, 615)
  local title = (help_lang == 0) and "DELAY & REVERB CALCULATOR — USER MANUAL"
                                  or "DELAY & REVERB CALCULATOR — ПОСІБНИК КОРИСТУВАЧА"
  text_center_t(25, 27, 550, 32, title, 15, T.hi)

  if hover.help_lang then setc(T.line) else setc(T.panel, 1.5) end
  draw_rrect(585, 35, 85, 32, 6)
  text_center(585, 35, 85, 32, (help_lang == 0) and "EN | UA" or "UA | EN", 11)

  if hover.help_close then setc(T.line) else setc(T.panel, 1.5) end
  draw_rrect(675, 35, 90, 32, 6)
  text_center(675, 35, 90, 32, (help_lang == 0) and "Close" or "Закрити", 11)

  setc(T.line, 1, 0.4); grect(30, 78, 725, 0, 1, nil, 1); gfx.a = 1

  local page = HELP_PAGES[help_scroll + 1]
  if page then
    for _, it in ipairs(page[(help_lang == 0) and "en" or "ua"]) do
      if it[1] == "h" then text_left(40, it[2], it[3], 16) else text_muted(40, it[2], it[3], 13) end
    end
  end

  if help_scroll == 0 then
    draw_kofi_qr(620, 395, 5)
    setc(T.hi); set_font(11)
    local s = (help_lang == 0) and "Support development" or "Підтримати розробку"
    local tw = gfx.measurestr(s)
    gfx.x = floor(offset_x + (620 + 62) * scale_x - tw / 2); gfx.y = Yp(545); gfx.drawstr(s)
    setc(T.muted); set_font(10)
    s = "ko-fi.com/vitaliisound"
    tw = gfx.measurestr(s)
    gfx.x = floor(offset_x + (620 + 62) * scale_x - tw / 2); gfx.y = Yp(560); gfx.drawstr(s)
  end

  if help_scroll > 0 then setc(T.panel, 1.5) else setc(T.panel) end
  draw_rrect(35, 585, 90, 35, 6); text_center(35, 585, 90, 35, "< Prev", 11)
  text_center_t(125, 585, 535, 35, string.format("PAGE %d / 22", help_scroll + 1), 10, T.muted)
  if help_scroll < 21 then setc(T.panel, 1.5) else setc(T.panel) end
  draw_rrect(655, 585, 95, 35, 6); text_center(655, 585, 95, 35, "Next >", 11)
  text_center_t(125, 620, 535, 12,
    (help_lang == 0) and "Mouse wheel = page  |  ESC = close" or "Коліщатко = сторінка  |  ESC = закрити", 9, T.muted)
end

local function draw_box_outline(x, y, w, h)
  grect(x, y, w, 0, 1, nil, 1)
  grect(x, y + h - 1, w, 0, 1, nil, 1)
  grect(x, y, 0, h, 1, 1, nil)
  grect(x + w - 1, y, 0, h, 1, 1, nil)
end

local function draw_ts_field(x, editing, str, value, cx)
  if editing then setc(T.panel, 1.1) else setc(T.panel, 1.5) end
  draw_rrect(x, 228, 95, 65, 8)
  setrgb(1, 1, 1, 0.06); grect(x + 6, 229, 83, 0, 1, nil, 1); gfx.a = 1
  if editing then setc(T.accent); draw_box_outline(x, 228, 95, 65) end

  set_font(32); gfx.a = 1
  if editing and #str > 0 then
    setc(T.accent)
    local tw, th = gfx.measurestr(str)
    gfx.x = floor(offset_x + cx * scale_x - tw / 2); gfx.y = floor(offset_y + 260 * scale_y - th / 2)
    gfx.drawstr(str)
  elseif editing then
    if blink() then setc(T.hi); gfx.rect(floor((cx - 1) * scale_x), Yp(243), 2, Sp(35), 1) end
  else
    setc(T.text)
    local s = string.format("%d", value)
    local tw, th = gfx.measurestr(s)
    gfx.x = floor(offset_x + cx * scale_x - tw / 2); gfx.y = floor(offset_y + 260 * scale_y - th / 2)
    gfx.drawstr(s)
  end
end

local function draw_ts_menu()
  setrgb(0, 0, 0, 0.7); gfx.rect(0, 0, gfx.w, gfx.h, 1); gfx.a = 1
  panel(195, 160, 395, 400)
  text_center(195, 168, 395, 26, "TIME SIGNATURE", 16)
  setc(T.line, 1, 0.4); grect(215, 212, 355, 0, 1, nil, 1); gfx.a = 1

  draw_ts_field(275, edit_ts == 1, ts_num_str, cur_ts_num, 322)
  text_center_c(370, 228, 45, 65, "/", 32, T.text[1], T.text[2], T.text[3])
  draw_ts_field(415, edit_ts == 2, ts_den_str, cur_ts_den, 462)

  if sync_mode then
    text_center_t(195, 305, 395, 20, "SYNC: size follows host", 12, T.hi)
  else
    text_center(195, 305, 395, 20, string.format("= %.2f quarter notes per bar", cur_ts_num * 4 / cur_ts_den), 12)
  end

  setc(T.line, 1, 0.4); grect(215, 332, 355, 0, 1, nil, 1); gfx.a = 1
  text_center(195, 340, 395, 18, "QUICK PRESETS", 11)
  local P = {
    {200,368,2,4,"March"},{295,368,3,4,"Waltz"},{390,368,4,4,"Common"},{485,368,5,4,"Five Four"},
    {200,432,6,8,"Six Eight"},{295,432,7,8,"Seven Eight"},{390,432,9,8,"Nine Eight"},{485,432,12,8,"Twelve Eight"},
  }
  for _, p in ipairs(P) do
    ts_preset_btn(p[1], p[2], 90, 54, p[3], p[4], p[5], cur_ts_num == p[3] and cur_ts_den == p[4])
  end

  setc(T.muted); set_font(10)
  local s = "ENTER to close  -  ESC to cancel"
  local tw = gfx.measurestr(s)
  gfx.x = floor(offset_x + (195 + 395 / 2) * scale_x - tw / 2); gfx.y = Yp(538); gfx.drawstr(s)
end

local function put(x, y, s, size, c)
  setc(c); set_font(size)
  gfx.x = Xp(x); gfx.y = Yp(y); gfx.drawstr(s)
end
local function put_right(xr, y, s)
  local tw = gfx.measurestr(s)
  gfx.x = floor(offset_x + xr * scale_x - tw); gfx.y = Yp(y); gfx.drawstr(s)
end

local function draw_custom_menu()
  setrgb(0, 0, 0, 0.7); gfx.rect(0, 0, gfx.w, gfx.h, 1); gfx.a = 1
  panel(182, 160, 420, 470)
  text_center(182, 175, 420, 30, "CUSTOM CONVERTER", 14)
  setc(T.line, 1, 0.4); grect(212, 208, 360, 0, 1, nil, 1); gfx.a = 1

  setrgb(0, 0, 0, 0.3); draw_rrect(212, 220, 360, 55, 8)
  setc(T.panel, 1.2);   draw_rrect(212, 222, 360, 51, 8)

  if #custom_string > 0 then
    draw_edit_text(232, 234, custom_string, 24, 28)
  else
    put(232, 234, "_ _ _", 24, T.muted)
  end
  put(532, 240, "ms", 16, T.muted)

  local custom_ms = 0
  if #custom_string > 0 then custom_ms = tonumber(custom_string) or 0 end

  if custom_ms > 0.5 and custom_ms < 10000 then
    find_best_note(custom_ms)
    local note_name = build_note_name(fbn_best_row, fbn_best_col)

    put(212, 290, "CLOSEST MATCH", 11, T.muted)
    put(212, 312, note_name, 22, T.accent)
    put(212, 348, string.format("%.2f ms @ %.2f BPM", fbn_best_ms, bpm), 13, T.text)

    local err_pct = (custom_ms - fbn_best_ms) / fbn_best_ms * 100
    local out
    if math.abs(err_pct) < 0.05 then out = "Error: 0.0%  exact match"
    elseif err_pct > 0 then out = string.format("Error: +%.1f%% (your value is longer)", err_pct)
    else out = string.format("Error: %.1f%% (your value is shorter)", err_pct) end
    put(212, 370, out, 13, T.text)

    put(212, 398, "EXACT BPM FOR THIS NOTE VALUE", 11, T.muted)
    put(212, 416, string.format("%.2f", 60000 * fbn_best_mult / custom_ms), 22, T.hi)

    setc(T.line, 1, 0.4); grect(212, 450, 360, 0, 1, nil, 1); gfx.a = 1

    put(212, 460, "EFFECT TIME", 11, T.muted)
    put(212, 476, note_name, 13, T.text)
    local tw1 = gfx.measurestr(note_name)
    gfx.x = floor(offset_x + 212 * scale_x + tw1); gfx.drawstr(" @ ")
    local tw2 = gfx.measurestr(" @ ")
    gfx.x = floor(offset_x + 212 * scale_x + tw1 + tw2); gfx.drawstr(string.format("%.2f BPM", bpm))

    put(212, 494, string.format("%.2f ms", fbn_best_ms), 22, T.accent)
    put(212, 526, "Useful for:", 11, T.muted)

    put(212, 545, "Reverb Tail / Decay (suggested)", 12, T.text)
    put_right(560, 545, string.format("%.2f ms", fbn_best_ms * 4))
    put(212, 562, "Delay Time", 12, T.text)
    put_right(560, 562, string.format("%.2f ms", fbn_best_ms))
    put(212, 579, "Ducking Release (suggested)", 12, T.text)
    put_right(560, 579, string.format("%.2f ms", fbn_best_ms * 2))
  else
    put(212, 310, "Type a value in milliseconds...", 14, T.muted)
  end

  setc(T.muted); set_font(10)
  local s = "ENTER to close  -  ESC to cancel"
  local tw = gfx.measurestr(s)
  gfx.x = floor(offset_x + (182 + 420 / 2) * scale_x - tw / 2); gfx.y = Yp(608); gfx.drawstr(s)
end

-- ----------------------------------------------------------------------------
--  Main frame
-- ----------------------------------------------------------------------------
local function reset_taps()
  tap_n = 0; tap_i = {}; tap_last = 0
end

local function in_rect(x1, x2, y1, y2) return mx >= x1 and mx <= x2 and my >= y1 and my <= y2 end

local function frame()
  -- scaling / mouse -----------------------------------------------------------
  local sx, sy = gfx.w / base_w, gfx.h / base_h
  scale = (sx < sy) and sx or sy
  if scale < 0.2 then scale = 0.2 end
  scale_x = math.max(sx, 0.2); scale_y = math.max(sy, 0.2)
  offset_x, offset_y = 0, 0
  mx = gfx.mouse_x / scale_x
  my = gfx.mouse_y / scale_y

  -- keyboard (once per frame, dispatched by the active mode) --------------------
  local key = gfx.getchar()
  while key > 0 do
    handle_key(key)
    key = gfx.getchar()
  end
  if key < 0 then return false end

  -- host / manual tempo --------------------------------------------------------
  if sync_mode then
    local tempo, n, d = read_host()
    if tempo and tempo > 0 then bpm = tempo end
    if n and d and n > 0 and d > 0 then cur_ts_num, cur_ts_den = n, d end
  else
    bpm = manual_bpm
    cur_ts_num, cur_ts_den = manual_ts_num, manual_ts_den
  end
  cur_ts_num = clamp(cur_ts_num, 1, 16); cur_ts_den = clamp(cur_ts_den, 1, 16)
  bpm = clamp(bpm, 20, 300)
  calc_feedback_tail()

  -- mouse state ----------------------------------------------------------------
  local cap = floor(gfx.mouse_cap)
  local mouse_clicked       = (cap & 1)  ~= 0 and (last_cap & 1)  == 0
  local mouse_mid_clicked   = (cap & 64) ~= 0 and (last_cap & 64) == 0
  local mouse_right_clicked = (cap & 2)  ~= 0 and (last_cap & 2)  == 0
  last_cap = cap
  local wheel = gfx.mouse_wheel
  gfx.mouse_wheel = 0

  -- background -----------------------------------------------------------------
  setc(T.bg); gfx.rect(0, 0, gfx.w, gfx.h, 1)

  hover.sync   = in_rect(455, 575, 18, 48)
  hover.theme  = in_rect(585, 690, 18, 48)
  hover.help   = in_rect(700, 770, 18, 48)
  hover.tab1   = in_rect(15, 260, 325, 357)
  hover.tab2   = in_rect(270, 515, 325, 357)
  hover.tab3   = in_rect(525, 770, 325, 357)
  hover.tap    = in_rect(400, 770, 75, 190)
  hover.ms     = mx >= 20 and mx < 143 and my >= 255 and my <= 307
  hover.s      = mx >= 143 and mx < 266 and my >= 255 and my <= 307
  hover.hz     = in_rect(266, 389, 255, 307)
  hover.ts     = in_rect(400, 770, 255, 307)
  hover.custom = in_rect(15, 770, 200, 248)
  local tap_press = (cap & 1) ~= 0 and hover.tap and not sync_mode
  local overlay = show_help or show_custom_menu or show_ts_menu

  -- content scrolling ----------------------------------------------------------
  local content_top = 354
  local content_bottom = (active_tab == 2) and 900 or 670
  local scroll_max = content_bottom - base_h
  if scroll_max < 0 then scroll_max = 0 end
  if not overlay and wheel ~= 0 then scroll_y = scroll_y - (wheel / 120) * 30 end
  scroll_y = clamp(scroll_y, 0, scroll_max)
  local content_y = content_top - scroll_y

  if active_tab == 0 then draw_tab_reverb(content_y)
  elseif active_tab == 1 then draw_tab_notes(content_y)
  else draw_tab_tail(content_y) end

  -- header --------------------------------------------------------------------
  setc(T.bg); gfx.rect(0, 0, gfx.w, floor(offset_y + 360 * scale_y), 1)

  text_left(25, 18, "Delay & Reverb Calculator v1.0", 21)
  text_muted(25, 47, "Tempo based calculator for delay, reverb and FX timing", 11)

  if sync_mode then setc(T.accent) elseif hover.sync then setc(T.line) else setc(T.panel) end
  draw_rrect(455, 18, 120, 30, 6)
  if sync_mode then text_center_t(455, 18, 120, 30, "SYNC: HOST", 12, DARK)
  else text_center_t(455, 18, 120, 30, "SYNC: MANUAL", 12, T.text) end

  if hover.theme then setc(T.line) else setc(T.panel) end
  draw_rrect(585, 18, 105, 30, 6)
  text_center(585, 18, 105, 30, T.name, 12)

  if hover.help then setc(T.line) else setc(T.panel, 1.3) end
  draw_rrect(700, 18, 70, 30, 6)
  text_center(700, 18, 70, 30, "? Help", 13)

  -- BPM panel
  panel(15, 75, 370, 115)
  local bpm_caption
  if sync_mode then bpm_caption = "HOST BPM (LOCKED)"
  elseif edit_bpm then bpm_caption = "TYPE BPM & PRESS ENTER"
  else bpm_caption = "ENTER YOUR BPM" end
  text_muted(35, 88, bpm_caption, 12)

  setrgb(0, 0, 0, 0.3); draw_rrect(35, 113, 330, 49, 8)
  setc(T.panel, 1.2);   draw_rrect(35, 115, 330, 45, 8)

  if edit_bpm and not sync_mode then
    setc(T.accent, 1, 0.5)
    grect(35, 115, 330, 0, 1, nil, 1)
    grect(35, 159, 330, 0, 1, nil, 1)
    gfx.a = 1
    draw_edit_text(45, 124, bpm_string, 24, 26)
  else
    if sync_mode then setc(T.hi) else setc(T.accent) end
    set_font(24); gfx.x = Xp(45); gfx.y = Yp(124)
    gfx.drawstr(string.format("%.2f", bpm))
  end

  local bpm_ratio = clamp((bpm - 20) / 280, 0, 1)
  setc(T.line);   grect(35, 170, 330, 0, 1, nil, 3)
  setc(T.accent); grect(35, 170, 0, 0, 1, floor(330 * bpm_ratio * scale_x), 3)
  setc(T.hi);     gfx.rect(floor(offset_x + (35 + 330 * bpm_ratio - 2) * scale_x), Yp(169), 2, 5, 1)
  setc(T.muted)
  gfx.rect(floor(offset_x + (35 + 330 * 0.25) * scale_x), Yp(169), 1, 5, 1)
  gfx.rect(floor(offset_x + (35 + 330 * 0.50) * scale_x), Yp(169), 1, 5, 1)
  gfx.rect(floor(offset_x + (35 + 330 * 0.75) * scale_x), Yp(169), 1, 5, 1)

  -- TAP panel
  panel(400, 75, 370, 115)
  if sync_mode then
    setc(T.panel, 0.8); draw_rrect(420, 93, 330, 77, 10)
  elseif tap_press then
    setc(T.accent); draw_rrect(420, 95, 330, 75, 10)
  else
    if hover.tap then setc(T.line) else setc(T.panel, 1.3) end
    draw_rrect(420, 93, 330, 77, 10)
    setrgb(1, 1, 1, 0.06); grect(430, 94, 310, 0, 1, nil, 1); gfx.a = 1
  end
  local tap_label = (tap_n == 0) and "TAP BPM" or "TAP AGAIN"
  if sync_mode then setc(T.muted)
  elseif tap_press then setrgb(0.05, 0.05, 0.05, 1)
  else setc(T.accent) end
  set_font(24)
  local tw, th = gfx.measurestr(tap_label)
  gfx.x = floor(offset_x + (420 + 330 / 2) * scale_x - tw / 2)
  gfx.y = floor(offset_y + (95 + 28) * scale_y - th / 2 - 1 * scale)
  gfx.drawstr(tap_label)
  if tap_n > 0 then
    local s = string.format("TAPS: %d", tap_n)
    set_font(11)
    if sync_mode then setc(T.muted) else setc(T.hi) end
    local tw2 = gfx.measurestr(s)
    gfx.x = floor(offset_x + (420 + 330 / 2) * scale_x - tw2 / 2)
    gfx.y = floor(offset_y + (95 + 55) * scale_y)
    gfx.drawstr(s)
  end

  -- Custom button
  if hover.custom and not overlay then setc(T.line) else setc(T.panel) end
  draw_rrect(15, 200, 755, 48, 10)
  setrgb(1, 1, 1, 0.10); grect(27, 201, 731, 0, 1, nil, 1)
  setrgb(0, 0, 0, 0.18); grect(27, 247, 731, 0, 1, nil, 1)
  gfx.a = 1
  text_center(15, 200, 755, 48, "+  Custom", 18)

  -- Units
  panel(15, 255, 370, 62)
  unit_button(25,  265, 110, 42, unit == 0, hover.ms, "ms")
  unit_button(145, 265, 110, 42, unit == 1, hover.s,  "s")
  unit_button(265, 265, 110, 42, unit == 2, hover.hz, "Hz")

  -- Time signature
  panel(400, 255, 370, 62)
  if not show_ts_menu then
    if hover.ts then setc(T.line) else setc(T.panel, 1.3) end
    draw_rrect(435, 265, 330, 42, 8)
    setrgb(1, 1, 1, 0.05); grect(445, 266, 310, 0, 1, nil, 1); gfx.a = 1
    text_center(435, 265, 330, 42, string.format("%d/%d", cur_ts_num, cur_ts_den), 18)
  end

  -- Tabs
  local tabs = {
    {15,  "tab1", "Reverb Suggestions", 27},
    {270, "tab2", "Note Durations",     282},
    {525, "tab3", "Delay Tail",         537},
  }
  for i, t in ipairs(tabs) do
    local x = t[1]
    if active_tab == i - 1 then
      setc(T.accent); draw_rrect(x, 325, 245, 30, 6)
      setc(T.hi, 1, 0.6); grect(t[4], 353, 221, 0, 1, nil, 1); gfx.a = 1
    else
      if hover[t[2]] then setc(T.line) else setc(T.panel, 0.9) end
      draw_rrect(x, 325, 245, 30, 6)
      draw_bevel(x, 325, 245, 30, 6)
    end
    if active_tab == i - 1 then text_center_t(x, 325, 245, 30, t[3], 13, DARK)
    else text_center(x, 325, 245, 30, t[3], 13) end
  end

  -- Click handling ---------------------------------------------------------------
  if not overlay then
    if mouse_clicked and hover.tab1 then active_tab = 0; scroll_y = 0 end
    if mouse_clicked and hover.tab2 then active_tab = 1; scroll_y = 0 end
    if mouse_clicked and hover.tab3 then active_tab = 2; scroll_y = 0 end
  end

  if show_help then
    hover.help_close = in_rect(675, 765, 35, 70)
    hover.help_lang  = in_rect(585, 670, 35, 70)
    if mouse_clicked and hover.help_close then show_help = false end
    if mouse_clicked and hover.help_lang then help_lang = 1 - help_lang end
    if mouse_clicked and in_rect(35, 125, 585, 620) then help_scroll = math.max(0, help_scroll - 1) end
    if mouse_clicked and in_rect(655, 750, 585, 620) then help_scroll = math.min(21, help_scroll + 1) end
    if wheel > 0 then help_scroll = math.max(0, help_scroll - 1)
    elseif wheel < 0 then help_scroll = math.min(21, help_scroll + 1) end
    if mouse_clicked and (mx < 15 or mx > 770 or my < 20 or my > 635) then show_help = false end

  elseif show_custom_menu then
    if mouse_clicked and (mx < 182 or mx > 602 or my < 160 or my > 630) then show_custom_menu = false end

  elseif show_ts_menu then
    if mouse_clicked then
      if in_rect(275, 370, 228, 293) and not sync_mode then edit_ts = 1; ts_num_str = "" end
      if in_rect(415, 510, 228, 293) and not sync_mode then edit_ts = 2; ts_den_str = "" end
      if not sync_mode then
        local presets = {
          {200,368,2,4},{295,368,3,4},{390,368,4,4},{485,368,5,4},
          {200,432,6,8},{295,432,7,8},{390,432,9,8},{485,432,12,8},
        }
        for _, p in ipairs(presets) do
          if in_rect(p[1], p[1] + 90, p[2], p[2] + 54) then
            manual_ts_num, manual_ts_den = p[3], p[4]
            cur_ts_num, cur_ts_den = p[3], p[4]
            edit_ts = 0
          end
        end
      end
      if mx < 195 or mx > 590 or my < 160 or my > 560 then show_ts_menu = false; edit_ts = 0 end
    end

  else
    if mouse_clicked and hover.sync then sync_mode = not sync_mode; edit_bpm = false end
    if mouse_clicked and hover.theme then theme = (theme + 1) % 4; apply_theme() end
    if mouse_clicked and hover.help then show_help = true; help_scroll = 0 end
    if mouse_clicked and hover.custom then show_custom_menu = true; custom_string = "" end
    if mouse_clicked and (mx < 35 or mx > 365 or my < 115 or my > 160) then edit_bpm = false; bpm_string = "" end
    if mouse_clicked and my >= 255 and my <= 317 then
      if mx >= 25 and mx < 143 then unit = 0
      elseif mx >= 143 and mx < 266 then unit = 1
      elseif mx >= 266 and mx <= 389 then unit = 2 end
    end
    if mouse_clicked and hover.ts then show_ts_menu = true; edit_ts = 0 end
    if mouse_clicked and not sync_mode and in_rect(35, 365, 115, 160) then edit_bpm = true; bpm_string = "" end

    if (mouse_mid_clicked or mouse_right_clicked) and hover.tap and not sync_mode then reset_taps() end

    if mouse_clicked and not sync_mode and in_rect(400, 770, 75, 190) then
      local now = r.time_precise()
      if tap_last > 0 and now - tap_last > 2.0 then tap_n = 0; tap_i = {} end
      if tap_last > 0 then
        local interval = now - tap_last
        if interval > 0.1 and interval < 2.0 then
          table.insert(tap_i, 1, interval)
          while #tap_i > 6 do table.remove(tap_i) end
          tap_n = math.min(tap_n + 1, 6)
          local tsum = 0
          for i = 1, tap_n do tsum = tsum + (tap_i[i] or 0) end
          local avg = tsum / tap_n
          if avg > 0 then
            bpm = clamp(60 / avg, 20, 300)
            manual_bpm = bpm
          end
        end
      end
      tap_last = now
    end

    if active_tab == 2 then
      if mouse_clicked and hover.fb_mode0 then fb_mode = 0; fb_edit_field = 0; fb_buf = "" end
      if mouse_clicked and hover.fb_mode1 then fb_mode = 1; fb_edit_field = 0; fb_buf = "" end
      if mouse_clicked and hover.thr40 then fb_threshold = -40; fb_threshold_idx = 0 end
      if mouse_clicked and hover.thr50 then fb_threshold = -50; fb_threshold_idx = 1 end
      if mouse_clicked and hover.thr60 then fb_threshold = -60; fb_threshold_idx = 2 end
      if mouse_clicked and hover.thr70 then fb_threshold = -70; fb_threshold_idx = 3 end
      if mouse_clicked and hover.dmp0 then fb_damping_mode = 0 end
      if mouse_clicked and hover.dmp1 then fb_damping_mode = 1 end
      if mouse_clicked and hover.dmp2 then fb_damping_mode = 2 end
      if mouse_clicked and hover.fb_delay then fb_edit_field = 1; fb_buf = "" end
      if mouse_clicked and hover.fb_input then
        fb_edit_field = (fb_mode == 0) and 2 or 3; fb_buf = ""
      end
    end
  end

  -- Overlays ---------------------------------------------------------------------
  if show_help then draw_help() end
  if show_ts_menu then draw_ts_menu() end
  if show_custom_menu then draw_custom_menu() end

  return true
end

-- ----------------------------------------------------------------------------
--  Startup
-- ----------------------------------------------------------------------------
load_state()

local function getn(k) return tonumber(r.GetExtState(EXT, k)) end
local w0, h0 = getn("ww") or base_w, getn("wh") or base_h
local dock0 = getn("dock") or 0
local x0, y0 = getn("wx"), getn("wy")
if not x0 or not y0 then x0, y0 = 100, 100 end
if w0 < 300 or h0 < 250 then w0, h0 = base_w, base_h end

gfx.ext_retina = 1
gfx.init("Delay & Reverb Calculator Pro v1.0", w0, h0, dock0, x0, y0)

local saved = false
local function loop()
  if frame() then
    gfx.update()
    r.defer(loop)
  else
    save_state(); saved = true
    gfx.quit()
  end
end

r.atexit(function() if not saved then save_state() end end)
loop()
