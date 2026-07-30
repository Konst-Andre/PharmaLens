# PharmaLens — Independent Design Research & Handover

**Роль автора:** Principal Product Designer & Design System Architect  
**Дата:** 31 липня 2026  
**Статус:** незалежне дослідження дизайн-простору для MVP; не є затвердженим фінальним дизайном.

## 0. Як користуватися документом

Позначки розділяють те, що вже відомо, від професійної інтерпретації:

- **Факт** — безпосередньо підтверджено матеріалами проєкту.
- **Висновок** — незалежний синтез фактів.
- **Припущення** — правдоподібне, але ще не перевірене в полі.
- **Рекомендація** — запропонований наступний крок або правило, яке потребує рішення власника продукту.

Цей документ не намагається «покращити» вже прийняті рішення за інерцією. Він розділяє незмінне продуктове ядро від гіпотез, які мають бути перевірені або відкинуті.

---

## 1. Research Frame

### 1.1. Що відомо про продукт

**Факт.** PharmaLens (у поточних специфікаціях — ВТМ Lens) — mobile-first PWA-довідник для фармацевтів АНЦ. Він перетворює щомісячну презентацію ВТМ на швидкий, офлайн-доступний інструмент: скарга клієнта → зв'язка товарів → скрипт → короткі аргументи → бонус за чек.

**Факт.** Основні контексти використання:

1. **Counter:** фармацевт біля першого столу має мало часу й переважно користується телефоном.
2. **Study:** фармацевт до зміни вивчає зв'язки, аргументи й ВТМ-асортимент.
3. **Desktop:** епізодичний перегляд/навчання на ПК, не окремий основний продукт.

**Факт.** У специфікації вже є три вкладки: «Зв'язки», «Топ ВТМ», «Місяць/довідка», пошук за скаргою, архів у header, light-first з повноцінним dark mode, тональні фони, Manrope, білий packshot well, синій як семантика довіри й золото як семантика бонусу.

### 1.2. Дизайн-проблема, а не лише стильова задача

**Висновок.** Тут не треба спроєктувати «гарну медичну апку». Треба збалансувати чотири напруги:

| Напруга | Наслідок для дизайну |
|---|---|
| Швидкість Counter vs. повнота Study | Один контент має мати два рівні читання, а не два незалежні інтерфейси |
| Професійність фармації vs. фінансовий стимул | Бонус повинен бути видимим, але не виглядати як агресивний sales-pressure |
| Теплота й характер vs. миттєва читабельність | Матеріальність/глибина можуть бути шаром ієрархії, але не декоративною перешкодою |
| Мобільна щільність vs. безпечний tap/читання | Потрібні великі target-и, стабільний порядок блоків і дуже короткі тексти |

### 1.3. Незалежний аудит наявного дизайн-напряму

| Наявна теза | Оцінка |
|---|---|
| Тональні light/dark поверхні замість стерильного білого/чорного | **Сильна.** Дає власний характер і не заважає функції, якщо контраст контролюється. |
| Три напрями «Тепла аптечна», «Нічна зміна», «Чек-крафт» | **Неповні як концепції.** Це переважно три палітрово-матеріальні варіації однієї карткової апки, а не різні моделі взаємодії. |
| Liquid glass, blur, глибина | **Слабке як правило.** Depth корисна, але glassmorphism не є власною цінністю PharmaLens і може шкодити контрасту/продуктивності. |
| Бонус як золотий акцент | **Сильна семантика з обмеженням.** Золото має означати лише гроші/підсумок; інакше воно знецінюється. |
| Карти 1:1, 3:4, 16:9 як вихідна рамка | **Потребує перегляду.** Співвідношення має випливати з задачі екрана, а не з бажання знайти «одну ідеальну форму картки». |
| `object-fit: contain` для пакшотів | **Правильне.** Будь-яке попереднє припущення про `cover` для пакшотів треба відкинути: обрізати упаковку не можна. |
| Білий product well у dark mode | **Правильне з уточненням.** Це може бути нейтральне майданчик-освітлення для реального пакшоту, а не біла картка всього UI. |
| Палітра як перемикач у продукті | **Не рекомендовано.** Користувачеві потрібна одна впізнавана система та light/dark, а не три бренд-концепти. |

### 1.4. Що має довести дизайн MVP

Дизайн вважається успішним не тоді, коли команда погодила палітру, а коли пілотний фармацевт:

- за 5–10 секунд знаходить потрібну зв'язку за природним формулюванням скарги;
- розуміє, що сказати далі, без перечитування абзацу;
- не плутає ВТМ, якір, бонус і підсумок;
- може скористатися деталлю однією рукою на вузькому телефоні;
- відчуває продукт як робочу опору, а не як засіб тиску на продажі.

---

## 2. Common Design-System Contract

Усі концепції нижче різні за філософією, але використовують один масштабований контракт. Це дозволяє порівнювати їх чесно й не прив'язувати UI до випадкових hex-кодів.

### 2.1. Token tiers

| Рівень | Призначення | Приклад |
|---|---|---|
| **Primitive tokens** | Сирі, повторно-використовувані значення без UI-сенсу | `blue.700`, `sand.100`, `space.16`, `radius.16` |
| **Semantic tokens** | Роль у темі та продукті | `color.canvas`, `color.text.primary`, `color.money` |
| **Component tokens** | Значення конкретного компонента | `connection-card.background`, `bonus-pill.text` |
| **State tokens** | Стан interaction/accessibility | `action.primary.hover`, `focus.ring`, `surface.selected` |

### 2.2. Спільні правила доступності й адаптації

- Ключові touch target-и: **44–48 px**; дрібні іконки отримують більшу невидиму tap-area.
- Типографіка підтримує system text scaling; базовий body не нижче 16 px у detail-екрані.
- Контраст тексту та важливих іконок перевіряється для обох тем; критичний сенс не передається лише кольором.
- Fixed bottom bar враховує `env(safe-area-inset-bottom)`; листи/detail не закривають дію.
- На desktop використовуються та сама IA й токени; змінюється лише щільність/розкладка.
- Product packshot завжди використовує `contain`; альтернативний текст формується з назви товару.
- Motion вимикається або скорочується з `prefers-reduced-motion`.

### 2.3. Формат для CSS, JSON та Figma

```css
/* CSS: semantic aliases, що змінюються за темою */
:root[data-theme="light"] {
  --pl-color-canvas: var(--pl-primitive-sand-50);
  --pl-color-money: var(--pl-primitive-amber-700);
}

:root[data-theme="dark"] {
  --pl-color-canvas: var(--pl-primitive-ink-950);
  --pl-color-money: var(--pl-primitive-amber-300);
}
```

```json
{
  "color": {
    "primitive": {"sand": {"50": {"value": "#F7F1E8"}}},
    "semantic": {"canvas": {"value": "{color.primitive.sand.50}"}},
    "component": {"connectionCard": {"background": {"value": "{color.semantic.surface}"}}},
    "state": {"focus": {"ring": {"value": "{color.semantic.focus}"}}}
  }
}
```

```text
Figma Variables:
Collection: Primitive / Mode: Global
Collection: Theme / Modes: Light, Dark
Collection: Component / Modes: Light, Dark
Naming: color/semantic/canvas, component/connection-card/background,
        space/4, radius/card, motion/press
```

**Рекомендація.** Primitive collection лишається стабільною. Light/Dark перемикають semantic aliases, а не просто інвертують усі значення. Компоненти посилаються тільки на semantic або component aliases.

---

## 3. Design Concepts

### Концепт A — Quiet Dispensary / «Тиха аптечна»

#### Ідея і характер

PharmaLens — це добре організований робочий зошит/диспенсер знань: теплий, спокійний, точний. Ця система не грає в «медичний hi-tech» і не перетворює екран на касовий чек. Вона створює відчуття впорядкованого місця, у якому кожна річ лежить на своєму місці.

**Емоційне сприйняття:** спокійна компетентність, довіра, людяність, концентрація.

#### UX і візуальні принципи

- Пошук і скарга є першими, картки — другим кроком.
- Щедре, але не марнотратне повітря між групами; одна сильна дія/фокус на екрані.
- Глибина позначає рівні: canvas → surface → recessed packshot well; не імітує скло.
- Бонус — теплий, чіткий блок у нижній частині detail; не конкурує зі скриптом.
- Study додає пояснення під уже зрозумілим Counter-шаром.

#### Де працює найкраще

Збалансований default для змішаної аудиторії, щоденного використання, навчання новачків і довгого читання на ПК.

#### Сильні сторони

- Висока довіра та читабельність.
- Легко масштабувати на нові місяці/товари.
- Найкраще підтримує одночасно Study і Counter.
- Природно сумісний з уже зафіксованою tonal/Manrope/packshot логікою.

#### Слабкі сторони і ризики

- Без дисципліни може стати просто «приємною бежевою картковою апкою».
- Занадто багато тіней/зерна перетворить спокій на декоративність.
- Не створює агресивного відчуття швидкості; для Counter треба довести ієрархію реальними тестами.

#### Повна дизайн-система A

| Шар | Light theme | Dark theme | Логіка |
|---|---|---|---|
| Canvas | `sand.50 #F7F1E8` | `ink.950 #1B1915` | Теплий паперовий простір, а не білий/чорний |
| Surface | `sand.100 #EFE7DA` | `ink.900 #242119` | Робочі острови |
| Raised surface | `paper.50 #FCF9F2` | `ink.850 #2D291F` | Detail, sheet, selected block |
| Packshot well | `paper.0 #FFFFFF` | `stone.700 #3A352B` | Нейтральний фон реальних упаковок |
| Primary text | `ink.900 #1A1710` | `cream.100 #F1E9D9` | Довге читання |
| Secondary text | `umber.700 #625744` | `sand.500 #B5A68C` | Пояснення й metadata |
| Brand/navigation | `blue.800 #234874` | `blue.400 #75A8E6` | Довіра, active navigation, links |
| Money | `amber.700 #A86708` | `amber.300 #F1BE54` | Лише бонус і total |
| Focus/selection | `blue.500 #4B78AF` | `blue.300 #A9C8F0` | Видимий keyboard/touch focus |
| Critical | `red.700 #B84238` | `red.300 #F29A8F` | Помилки, не медичні діагнози |

| Система | Значення / правило |
|---|---|
| Typography | `Manrope, system-ui, sans-serif`; Display 28/34 800, Title 22/28 750, Section 18/24 700, Body 16/24 500, Label 13/18 700, Meta 12/16 600. Не використовувати mono як декоративний стиль. |
| Spacing | Primitive: 4, 8, 12, 16, 20, 24, 32, 40, 48. Основний ритм detail: 24; список: 12–16. |
| Radius | 8 (pill metadata), 12 (control), 18 (card), 24 (sheet). Inner radius = outer radius minus padding. |
| Elevation | `recessed` — тонкий inset для well; `raised-1` — 0 2 10 / 10% blue-ink; `raised-2` — 0 10 28 / 12%. У dark — короткі м'які тіні, не чорні провали. |
| Motion | Press 90 ms scale .985; expand/sheet 180–220 ms ease-out; filter 120 ms. Motion підкреслює вкладення, не привертає увагу. |
| Iconography | Простий rounded 1.75 px stroke, оптично 20 px у 44 px target. Іконка завжди з label там, де значення не очевидне. |
| Layout | Mobile 16 px gutters, 8-column optical rhythm; max readable content 680 px на desktop; desktop detail може стати другою колонкою після 960 px. |
| Components | Search — recessed surface; connection row — title/hint/money, без колажу; detail — script first, flash points next; product card — packshot + role badge; total — calm amber band, не CTA button. |

**Token sample A:**

```json
{
  "A": {
    "primitive": {
      "color": {"sand": {"50": "#F7F1E8", "100": "#EFE7DA"}, "ink": {"900": "#1A1710", "950": "#1B1915"}, "blue": {"800": "#234874", "400": "#75A8E6"}, "amber": {"700": "#A86708", "300": "#F1BE54"}},
      "space": {"1": "4px", "2": "8px", "3": "12px", "4": "16px", "6": "24px", "8": "32px"},
      "radius": {"control": "12px", "card": "18px", "sheet": "24px"}
    },
    "semantic": {"color.canvas": "{color.sand.50}", "color.text.primary": "{color.ink.900}", "color.brand": "{color.blue.800}", "color.money": "{color.amber.700}"},
    "component": {"connectionCard.background": "{color.surface}", "connectionCard.radius": "{radius.card}", "bonusBlock.background": "{color.money.soft}"},
    "state": {"focus.ring": "{color.blue.500}", "surface.selected": "{color.brand.soft}", "action.pressed.scale": "0.985"}
  }
}
```

---

### Концепт B — Counter Console / «Консоль першого столу»

#### Ідея і характер

PharmaLens — це інструмент моментального рішення, схожий на добре спроєктовану панель керування, а не на навчальний каталог. Інформація модульна, щільна, ієрархічна; кожна секунда та кожний піксель мають функцію.

**Емоційне сприйняття:** зібраність, швидкість, контроль, професійна готовність.

#### UX і візуальні принципи

- Екран за замовчуванням — command/search, а не редакційна добірка.
- Зв'язки подані як короткі operational rows із статусним рівнем, а не як великі cards.
- Detail — «відповідь зараз», потім «чому», потім «повний склад».
- Колір і типографіка створюють сильні scan lanes: симптом → фраза → бонус.
- Study може використовувати той самий контент, але як «розгорнутий briefing».

#### Де працює найкраще

Найкращий для дуже динамічної аптеки, досвідчених користувачів і задач, де час пошуку важливіший за емоційність.

#### Сильні сторони

- Найшвидше сканування та найкраща інформаційна щільність.
- Природно масштабується на великий список зв'язок.
- Добре працює на ПК як split-pane робоча панель.

#### Слабкі сторони і ризики

- Може здаватися холодним або «корпоративно-контрольним».
- Недосвідченому фармацевту потрібні пояснення, яких щільна система приховує.
- Якщо використовувати занадто багато бейджів/кольорових статусів, консоль швидко перетворюється на шум.

#### Повна дизайн-система B

| Шар | Light theme | Dark theme | Логіка |
|---|---|---|---|
| Canvas | `slate.50 #EEF2F6` | `navy.950 #0E1725` | Нейтральна операційна база |
| Surface | `slate.100 #E2E8F0` | `navy.900 #152235` | Чіткі модулі |
| Raised surface | `white-blue #F8FBFF` | `navy.850 #1B2B42` | Detail/selected state |
| Packshot well | `#FFFFFF` | `slate.700 #34465C` | Нейтральна опора для packshot |
| Primary text | `navy.900 #14263C` | `ice.100 #E5F0FC` | Висока читабельність |
| Secondary text | `slate.600 #52657B` | `slate.400 #A3B4C8` | Службова інформація |
| Brand/action | `cobalt.700 #175FB7` | `cobalt.300 #7EB8FF` | Навігація, фокус, action |
| Money | `ochre.700 #9A6700` | `ochre.300 #F3C65B` | Лаконічне фінансове значення |
| Alert | `red.700 #B42318` | `red.300 #FFB1A8` | Лише системні повідомлення |

| Система | Значення / правило |
|---|---|
| Typography | Manrope; Display 26/30 800, Query 20/26 750, Row title 16/20 750, Body 15/22 500, Micro label 11/14 800 uppercase лише для коротких категорій. Цифри — tabular. |
| Spacing | 4, 8, 12, 16, 20, 28, 36. Рядок списку: min 64 px; groups separated 20–28 px. |
| Radius | 6 (tags), 10 (controls), 14 (rows), 18 (sheet). Геометрія стриманіша за A. |
| Elevation | Переважно borders і tonal separation; одна elevation для overlay. Без неоморфізму. |
| Motion | 70–120 ms; швидке position/opacity feedback. Немає large-card choreography. |
| Iconography | Геометричний 2 px stroke, 18/20 px; піктограми симптомів — прості й не діагностичні. |
| Layout | 16 px mobile gutter; fixed search; rows full-width. Desktop 320 px results + flexible detail after 900 px. |
| Components | Search має домінантну висоту 52 px; row — 3 scan lanes; script panel — action-blue edge; flash point — plain bullet; bonus — tabular number aligned right. |

**Token sample B:**

```json
{
  "B": {
    "primitive": {"color": {"slate": {"50": "#EEF2F6", "100": "#E2E8F0", "700": "#34465C"}, "navy": {"900": "#14263C", "950": "#0E1725"}, "cobalt": {"700": "#175FB7", "300": "#7EB8FF"}, "ochre": {"700": "#9A6700", "300": "#F3C65B"}}, "space": {"1": "4px", "2": "8px", "3": "12px", "4": "16px", "5": "20px"}, "radius": {"row": "14px", "control": "10px"}},
    "semantic": {"color.canvas": "{color.slate.50}", "color.action": "{color.cobalt.700}", "color.money": "{color.ochre.700}"},
    "component": {"search.height": "52px", "connectionRow.minHeight": "64px", "scriptPanel.edge": "{color.action}"},
    "state": {"row.active.background": "{color.action.soft}", "row.hover.background": "{color.surface.raised}", "focus.ring": "{color.cobalt.700}"}
  }
}
```

---

### Концепт C — Therapeutic Atlas / «Атлас скарг»

#### Ідея і характер

PharmaLens — це карта потреб, а не список препаратів. На першому рівні користувач бачить людські формулювання проблем, тематичні поля й чіткі «маршрути» до зв'язок. Товари — другий рівень; первинна одиниця дизайну — ситуація клієнта.

**Емоційне сприйняття:** орієнтація, ясність, турбота, відчуття «я знаю, куди йти».

#### UX і візуальні принципи

- Домашній екран — семантичні групи скарг плюс пошук, не нескінченний list.
- Кожна група має свій знак/патерн/нейтральну тональність, але не покладається на колір як єдиний код.
- Деталь показує «шлях»: скарга → уточнення → зв'язка → скрипт.
- Study використовує карту як mental model; Counter може шукати або обирати велику категорію.

#### Де працює найкраще

Для новачків, обмеженого числа важливих терапевтичних тем та навчального контексту, де потрібно запам'ятати структуру асортименту.

#### Сильні сторони

- Створює сильну ментальну модель і відрізняє продукт від каталогу.
- Добре пояснює, чому різні товари існують у зв'язці.
- Зменшує страх порожнього пошуку в новачка.

#### Слабкі сторони і ризики

- Категоризація може бути спірною або клінічно чутливою.
- На маленькому екрані «карта» легко стає декоративною сіткою.
- Досвідчений фармацевт може вважати зайвим один крок до пошуку.

#### Повна дизайн-система C

| Шар | Light theme | Dark theme | Логіка |
|---|---|---|---|
| Canvas | `mist.50 #F1F5F3` | `forest.950 #11201D` | Нейтральна природна карта, не «лікувальний зелений» |
| Surface | `mist.100 #E4ECE8` | `forest.900 #19302B` | Поля карти |
| Raised surface | `porcelain #F9FCFA` | `forest.850 #203A33` | Діалог/detail |
| Primary text | `charcoal #1C2926` | `mint.100 #E0F2EB` | Спокійний контраст |
| Secondary text | `moss.700 #536961` | `moss.300 #A7C2B7` | Підказки |
| Route/brand | `teal.700 #1B7468` | `teal.300 #78CABB` | Маршрути, active states |
| Money | `amber.700 #A96A0B` | `amber.300 #F2BF63` | Окрема фінансова семантика |
| Category accents | `plum.600`, `blue.600`, `terra.600` у light; приглушені аналоги у dark | Те саме | Тільки категорії, максимум 4–5, з підписом/іконкою |

| Система | Значення / правило |
|---|---|
| Typography | Manrope для UI; optional 600 letter-spaced category labels. Scale: 30/36, 22/28, 18/24, 16/24, 13/18. Без display serif — вона погіршить робочий характер. |
| Spacing | 4, 8, 12, 16, 24, 32, 40. Category clusters мають 24 px outer gap. |
| Radius | 10 (chip), 16 (route tile), 22 (detail sheet), 999 (status dot only). |
| Elevation | Контури/тональні «поля» важливіші за тіні; route tile має 1 subtle raised shadow. |
| Motion | Переходи route 180 ms, підсвічення обраного шляху 140 ms; ніякої рухомої мапи або паралаксу. |
| Iconography | Унікальні, але абстрактні category marks: не органи, не діагнози, не «медичні ілюстрації». 20 px glyph + текст. |
| Layout | Mobile: search, потім 2-column category tiles тільки якщо мінімальна ширина tile 148 px; інакше 1 column. Desktop: category rail + results. |
| Components | Category tile має label, 1-line hint, count; route breadcrumb на detail; connection card лишається стриманою; money block не пов'язаний з кольором категорії. |

**Token sample C:**

```json
{
  "C": {
    "primitive": {"color": {"mist": {"50": "#F1F5F3", "100": "#E4ECE8"}, "forest": {"950": "#11201D", "900": "#19302B"}, "teal": {"700": "#1B7468", "300": "#78CABB"}, "amber": {"700": "#A96A0B", "300": "#F2BF63"}}, "space": {"1": "4px", "2": "8px", "3": "12px", "4": "16px", "6": "24px"}, "radius": {"tile": "16px", "sheet": "22px"}},
    "semantic": {"color.canvas": "{color.mist.50}", "color.route": "{color.teal.700}", "color.money": "{color.amber.700}"},
    "component": {"categoryTile.background": "{color.surface}", "routeBreadcrumb.text": "{color.route}"},
    "state": {"category.selected.border": "{color.route}", "category.pressed.overlay": "{color.route.alpha.12}", "focus.ring": "{color.teal.700}"}
  }
}
```

---

### Концепт D — Conversation Deck / «Колода розмов»

#### Ідея і характер

PharmaLens — тренажер пам'яті й мовлення, побудований навколо реплік. Зв'язка виглядає не як продуктова картка, а як послідовність коротких двосторонніх карт: «клієнт каже», «фармацевт відповідає», «якщо заперечує», «запам'ятай це».

**Емоційне сприйняття:** підтримка, ясність, впевненість у розмові, навчальна енергія.

#### UX і візуальні принципи

- Перший об'єкт — живе формулювання клієнта у великій цитатній картці.
- Скрипт — одна коротка відповідь, яку можна прочитати вголос.
- Блискавки — response cues, а не загальні bullets.
- Study може розкривати картки поступово; Counter показує condensed «розмовний стек».

#### Де працює найкраще

Для онбордингу, підготовки до зміни, комунікаційних сценаріїв і ситуацій із сильними запереченнями клієнта.

#### Сильні сторони

- Найкраще переводить дані в людське мовлення.
- Відрізняє продукт від довідника/каталогу.
- Може стати природним мостом до окремого AI Trainer без змішування продуктів.

#### Слабкі сторони і ризики

- Картковість може стати повільною й «грайливою» за касою.
- Складні зв'язки з трьома-чотирма товарами важко показати як одну розмову.
- Є спокуса додати swiping, flips і gamification, які шкодять швидкості та accessibility.

#### Повна дизайн-система D

| Шар | Light theme | Dark theme | Логіка |
|---|---|---|---|
| Canvas | `lilac.50 #F5F2F8` | `violet.950 #1D1826` | Теплий нейтральний фон, не «дівочий» |
| Surface | `lilac.100 #ECE6F1` | `violet.900 #282034` | Шари розмови |
| Customer card | `lavender.100 #E5DCF0` | `violet.800 #362945` | Репліка клієнта |
| Pharmacist card | `blue.50 #EEF5FC` | `blue.900 #1B3045` | Відповідь фармацевта |
| Primary text | `plum.900 #291E32` | `lilac.100 #F0EAF5` | Читання тексту |
| Brand/action | `indigo.700 #4E5DBA` | `indigo.300 #AEBBFF` | Інтеракції, не ролі |
| Money | `amber.700 #A76C10` | `amber.300 #F3C866` | Незалежний від розмовних карт |
| Support | `teal.700 #23786E` | `teal.300 #83D0C5` | Success/remember cue |

| Система | Значення / правило |
|---|---|
| Typography | Manrope; quote 20/28 700, response 18/26 700, cue 15/22 600, body 16/24. Лапки/role labels явні, не лише кольорові. |
| Spacing | 4, 8, 12, 16, 20, 24, 32. Між turns 12 px, між phase groups 24 px. |
| Radius | 14 (small cue), 20 (speech card), 24 (sheet). Не використовувати буквальні speech bubbles з хвостиками. |
| Elevation | Customer/pharmacist відрізняються tonal background і тонким border, не тінями різної сили. |
| Motion | Reveal 160 ms opacity/height; press 90 ms. Заборонено flip/card-deck gestures для core flow. |
| Iconography | Мова/слухання/підказка як прості line-icons; не використовувати chatbot-face чи emotional emoji як UI-систему. |
| Layout | Mobile — вертикальний conversation stack; desktop — conversation + products/bonus side panel лише після 1000 px. |
| Components | Customer prompt card, pharmacist script card, objection cue, flash-point checklist, product association strip, total rail. У Counter script card завжди перша. |

**Token sample D:**

```json
{
  "D": {
    "primitive": {"color": {"lilac": {"50": "#F5F2F8", "100": "#ECE6F1"}, "violet": {"950": "#1D1826", "900": "#282034"}, "indigo": {"700": "#4E5DBA", "300": "#AEBBFF"}, "amber": {"700": "#A76C10", "300": "#F3C866"}}, "space": {"1": "4px", "2": "8px", "3": "12px", "4": "16px", "6": "24px"}, "radius": {"speech": "20px", "sheet": "24px"}},
    "semantic": {"color.canvas": "{color.lilac.50}", "color.customer": "{color.lavender.100}", "color.pharmacist": "{color.blue.50}", "color.money": "{color.amber.700}"},
    "component": {"customerCard.background": "{color.customer}", "scriptCard.background": "{color.pharmacist}", "scriptCard.radius": "{radius.speech}"},
    "state": {"cue.expanded.background": "{color.surface.raised}", "focus.ring": "{color.indigo.700}", "card.pressed.scale": "0.985"}
  }
}
```

---

### Концепт E — Incentive Ledger / «Реєстр можливостей»

#### Ідея і характер

PharmaLens — це місячний реєстр можливостей: чітка система, де видно, які зв'язки мають цінність і як бонус складається з товарів. Візуальна метафора — не буквальний чек, а охайна відомість/ledger із табличними цифрами та доказовими рядками.

**Емоційне сприйняття:** прозорість, винагорода, порядок, результат.

#### UX і візуальні принципи

- Бонус читається поруч із кожною зв'язкою; його склад можна прозоро розгорнути.
- Числа набрані tabular, totals завжди позначені.
- Скрипт лишається обов'язковим, але second-level після користі/зв'язки.
- Місяць tab стає справжнім summary, а не декоративним dashboard.

#### Де працює найкраще

Коли основна поведінкова мета продукту — зробити змінну бонусацію зрозумілою, перевірюваною та мотивувальною.

#### Сильні сторони

- Найкраще пояснює фінансову логіку й походження total.
- Таблична система надійна на desktop і зрозуміла контент-власнику.
- Стриманий стиль легше зберігати послідовним місяць за місяцем.

#### Слабкі сторони і ризики

- Небезпечно редукує фармацевтичну консультацію до продажу.
- Може бути психологічно неприйнятним для частини фармацевтів.
- Якщо зробити «чек» занадто буквально, виглядатиме дешево й застаріло.

#### Повна дизайн-система E

| Шар | Light theme | Dark theme | Логіка |
|---|---|---|---|
| Canvas | `warm-gray.50 #F3F0EA` | `graphite.950 #1A1A18` | Відомість, але не паперова імітація |
| Surface | `warm-gray.100 #E7E1D7` | `graphite.900 #252421` | Рядки/панелі |
| Raised surface | `ivory #FBF8F0` | `graphite.850 #302E29` | Detail та summary |
| Primary text | `charcoal #22201C` | `ivory.100 #F2EEE4` | Контраст для цифр/тексту |
| Secondary text | `warm-gray.700 #665F54` | `warm-gray.400 #B9B0A1` | Metadata |
| Brand/action | `blue.800 #294A70` | `blue.300 #9DC0E8` | Нейтральна навігація |
| Money | `gold.700 #9C6B12` | `gold.300 #F2C861` | Числа, total, тільки фінансові дані |
| Dividers | `line #D7CFC0` | `line #49453E` | Ledger rhythm |

| Система | Значення / правило |
|---|---|
| Typography | Manrope; tabular numeric feature для бонусу. Title 24/30 800, row title 16/20 700, bonus 18/22 800, body 15/22 500, metadata 12/16 650. |
| Spacing | 4, 8, 12, 16, 24, 32. Рядки ритмічно 56/64 px; таблиці не дрібніші за 14 px. |
| Radius | 6 (numeric chip), 12 (surface), 16 (detail); система менш rounded, але не «офісна таблиця». |
| Elevation | Переважно line/divider; один floating sheet. Жодних dashed borders як системного елементу. |
| Motion | 100 ms number highlight, 160 ms detail disclosure. Заборонено animated counters, які перебільшують bonus. |
| Iconography | Мінімальна: плюс, breakdown, info, archive. Числа й текст важливіші за іконки. |
| Layout | Mobile: row + right-aligned bonus; breakdown у sheet. Desktop: table/list with pinned detail. |
| Components | Ledger row, total block, bonus breakdown, neutral script card, top products table. Бонус ніколи не оформлюється як primary CTA. |

**Token sample E:**

```json
{
  "E": {
    "primitive": {"color": {"warmGray": {"50": "#F3F0EA", "100": "#E7E1D7", "700": "#665F54"}, "graphite": {"950": "#1A1A18", "900": "#252421"}, "blue": {"800": "#294A70", "300": "#9DC0E8"}, "gold": {"700": "#9C6B12", "300": "#F2C861"}}, "space": {"1": "4px", "2": "8px", "3": "12px", "4": "16px", "6": "24px"}, "radius": {"surface": "12px", "detail": "16px"}},
    "semantic": {"color.canvas": "{color.warmGray.50}", "color.line": "{color.warmGray.200}", "color.money": "{color.gold.700}"},
    "component": {"ledgerRow.divider": "{color.line}", "bonusValue.fontVariant": "tabular-nums", "totalBlock.background": "{color.money.soft}"},
    "state": {"ledgerRow.selected.background": "{color.surface.raised}", "number.updated.highlight": "{color.money.soft}", "focus.ring": "{color.blue.800}"}
  }
}
```

---

## 4. Cross-Concept Comparison

### 4.1. Порівняльна матриця

Оцінки нижче — **висновок**, а не виміряні дані. Вони потрібні, щоб обрати, що тестувати, а не щоб імітувати точність.

| Критерій | A Тиха аптечна | B Консоль | C Атлас | D Колода розмов | E Реєстр |
|---|---:|---:|---:|---:|---:|
| Counter speed | 4/5 | 5/5 | 3/5 | 3/5 | 4/5 |
| Study clarity | 4/5 | 3/5 | 5/5 | 5/5 | 3/5 |
| Фармацевтична довіра | 5/5 | 4/5 | 4/5 | 4/5 | 3/5 |
| Фінансова прозорість | 4/5 | 4/5 | 3/5 | 3/5 | 5/5 |
| Доступність/читабельність | 5/5 | 4/5 | 4/5 | 4/5 | 4/5 |
| Відмінність від generic app | 4/5 | 4/5 | 5/5 | 5/5 | 4/5 |
| Ризик scope/візуальної складності | Низький | Низький–середній | Середній | Середній | Середній |
| Сумісність із наявним MVP | 5/5 | 4/5 | 3/5 | 3/5 | 4/5 |

### 4.2. Коли кожна концепція є найкращим вибором — і коли ні

| Концепт | Чому може бути найкращим | Чому може бути помилкою | Компроміс |
|---|---|---|---|
| A | Найкращий баланс для реальної щоденної роботи й наявних правил продукту | Може стати надто тихим/безликим | Потрібен один сильний signature pattern: product well + money block + порядок detail |
| B | Якщо тест покаже, що пошук та швидкість абсолютно домінують | Холодність може зменшити adoption новачків | Виграє секунди, але втрачає частину навчальної теплоти |
| C | Якщо новачки не знають, де шукати, і цінують ментальну карту | Може помилково натякати на діагностику або ускладнювати просту навігацію | Виграє у поясненні, втрачає у прямоті |
| D | Якщо головна проблема — невпевненість у формулюваннях, не пошук товарів | Може бути повільним і схожим на тренажер | Виграє в комунікації, втрачає в операційній щільності |
| E | Якщо бонусація непрозора та потребує пояснення/довіри | Перетворює професійну допомогу на sales UI | Виграє у фінансовій ясності, але створює етичний і культурний ризик |

### 4.3. Що не варто робити

**Рекомендація.** Не створювати гібрид усіх п'яти концепцій. Це призведе до карти симптомів, карток діалогу, статусних панелей і чеків на одному екрані — саме того когнітивного шуму, який продукт має прибрати.

Допустиме змішування — лише на рівні **потреб**, а не декоративних патернів:

- A може прийняти B-порядок «скрипт вище деталей» у Counter.
- A може прийняти один C-прийом: зрозумілі тематичні групи у Study.
- A може прийняти один D-компонент: customer prompt як початок detail.
- A може прийняти один E-компонент: прозорий breakdown bonus.

Усі запозичення повинні пройти користувацький тест і не змінювати основну граматику A.

---

## 5. Recommendation

### 5.1. Primary recommendation — Concept A: Quiet Dispensary

**Рекомендація.** Взяти A як головний напрям MVP.

Він найкраще відповідає реальному завданню: дати фармацевту швидку, але не агресивну опору. Він не підміняє професійну консультацію продажем, не змушує користувача мислити «як у панелі» і не перетворює довідник на тренажер.

Це також найбезпечніший шлях із погляду системи: він узгоджується з уже сильними рішеннями — tonal surfaces, білим packshot well, Manrope, dark mode, синім і золотом — але очищає їх від зайвого glass/decor.

### 5.2. Secondary prototype — Concept B: Counter Console

**Рекомендація.** Не змішувати B у перший production design, але створити один альтернативний Counter-flow prototype для тесту.

Тестувати треба не палітру, а питання: чи краще досвідчений фармацевт знаходить і зчитує зв'язку в B-рядку, ніж у A-картці? Якщо різниця суттєва, A може запозичити щільність B для списку, залишивши теплу detail-систему.

### 5.3. Why not C, D, E as the primary system

- C має сильний навчальний потенціал, але розширює інформаційну архітектуру до того, як підтверджено базовий search flow.
- D чудово описує майбутній Trainer або Study-надбудову; для Counter він ризикує додати зайві кроки.
- E варто використовувати лише як pattern для прозорого bonus breakdown. Як головний тон він надто сильно зміщує етику продукту в бік стимулювання продажу.

### 5.4. Мінімальний набір signature patterns для A

Щоб A не став generic, система має послідовно повторювати чотири речі:

1. **Тональний робочий canvas** з помітним, але дуже спокійним surface hierarchy.
2. **Світлий packshot well** як фізично зрозуміле місце для товару на тональному фоні.
3. **Conversation-first detail:** репліка клієнта й готовий скрипт над переліком товарів.
4. **Amber bonus ledger:** маленький, точний, tabular, прозорий subtotal — ніколи не primary CTA.

---

## 6. Validation Plan Before a Final Direction

### 6.1. Що прототипувати

Потрібні не image mockups, а два клікабельні lightweight harness-и з одним і тим самим реальним контентом:

- **A prototype:** mobile list → detail → Study expansion.
- **B prototype:** mobile command search → operational rows → detail.

Обидва мають показати 5–7 реальних зв'язок, реальні пакшоти, довгі та короткі назви, 3/4 товари в одній зв'язці, порожній пошук, light/dark і один desktop layout.

### 6.2. Завдання тесту

| Завдання | Що вимірювати |
|---|---|
| «Клієнт просить щось від …» | Time-to-correct-connection, помилка вибору, чи користувач спочатку шукає/скролить |
| «Що ти скажеш клієнту?» | Чи знаходить користувач скрипт без пояснення модератора |
| «Скільки бонусу в цій зв'язці й чому?» | Розуміння total і breakdown без хибної інтерпретації |
| «Підготуйся до зміни» | Чи користувач розуміє, де Study-інформація |
| Offline replay | Чи є довіра, коли мережа вимкнена, і чи UI чесно показує стан |

### 6.3. Критерії вибору

Перед фіксацією напряму зібрати для кожного учасника:

- час виконання завдання;
- кількість хибних переходів;
- чи прочитано потрібний скрипт дослівно/по суті;
- суб'єктивну відповідь «чи можу я користуватися цим за касою?»;
- конкретні фрази про те, що було незрозуміло.

**Припущення.** 5–8 інтерв'ю з реальними фармацевтами вже виявлять більшість structural issues. Не треба чекати великої вибірки, щоб відмовитися від слабкої моделі.

---

## 7. Claude Design Handover

### Короткий підсумок дослідження

1. Найсильніше ядро PharmaLens — symptom-first, approved-content, mobile Counter-довідник.
2. Попередні три палітри корисні як material experiments, але не визначають повноцінні різні дизайн-системи.
3. Було розглянуто п'ять різних дизайн-філософій. Найкращий primary candidate — **Quiet Dispensary (A)**; найцінніший альтернативний test candidate — **Counter Console (B)**.
4. Не слід фіналізувати стиль без task-based device test на реальному контенті.

### Рекомендована концепція для продовження

Розвивати **A: Quiet Dispensary** як базову дизайн-систему:

- теплий тональний canvas;
- спокійні surfaces і tactile, але стримана depth;
- Manrope, чітка типографічна ієрархія;
- темна тема як окремий теплий нічний робочий простір;
- синій для навігації/довіри, amber тільки для bonus semantics;
- conversation-first detail, product well, transparent total.

### Альтернативні варіанти

- **B:** прототипувати як альтернативну форму списку/Counter speed benchmark.
- **C:** розглядати пізніше, якщо пілот покаже, що новачкам бракує ментальної карти тем.
- **D:** не включати в MVP; зберегти як найкращу дизайн-філософію для окремого Study/AI Trainer продукту.
- **E:** використовувати лише для bonus breakdown і month summary patterns.

### Рішення, які не варто змінювати без вагомої причини

- Скарга як основний entry point, а не товарний каталог.
- Один content model для Study і Counter; Counter не окремий tab.
- Три вкладки: Зв'язки, Топ ВТМ, Місяць/довідка.
- Бонус залежить від контексту місяць/зв'язка; totals обчислюються.
- Mobile-first, offline-first, local packshots, `contain` для упаковок.
- Light-first + повноцінна, неінвертована dark theme.
- Жорстка заборона чистого білого/чорного canvas; білий дозволений у packshot well.
- Золото має фінансову семантику; важливі ролі не передаються лише кольором.

### Рішення, які треба критично перевірити

- Чи не створюють тіні, texture, blur і glass зайвий візуальний шум?
- Чи справді 3 вкладки потрібні на першому пілотному місяці, чи Month summary можна тимчасово згорнути?
- Чи має Study бути ручним перемикачем, чи достатньо progressive disclosure в одному detail flow?
- Чи є архів важливим у MVP, чи лише архітектурною готовністю?
- Чи всі запропоновані типографічні рівні читаються на iPhone XS при збільшеному системному шрифті?
- Чи «сума бонусів» сформульована так, щоб не означати гарантований дохід?

### Відкриті питання для дизайнера

1. Який exact content package використовується для візуального тесту?
2. Хто фінально схвалює формулювання скриптів і «блискавок»?
3. Чи є Android/desktop accessibility requirement, окрім iOS?
4. Який рівень персоналізації/збереження користувацького стану допустимий?
5. Чи потрібна explicit brand mark, чи достатньо product name й дизайн-граматики?
6. Яка позиція бізнесу щодо tone: «допомога в консультації» чи «інструмент стимулювання продажів»? Від відповіді залежить межа E-концепту.

### Ризики для дизайн-системи

- Створити красиві токени без погодженої семантики компонентів.
- Перетворити dark theme на автоматичну інверсію.
- Робити рішення на blank mockup замість реальних довгих текстів/білих пакшотів.
- Замінити читабельність декоративним material treatment.
- Змішати UI AI Trainer з core PharmaLens.
- Додати багато variants до того, як базові components пройшли field test.

### Наступна послідовність роботи для Claude

1. Взяти A як base hypothesis і B як один контрольний альтернативний prototype.
2. Зібрати один real-content test set; відмовитися від placeholder-зображень і lorem-like текстів.
3. У Figma або коді створити primitive/theme/component collections із контракту цього документа.
4. Спроєктувати лише 5 екранів: Connections search, results, connection detail Counter, detail Study expansion, Top VTM/Month summary.
5. Перевірити light/dark на реальних device screenshots, з enlarged type й dark packshot wells.
6. Провести короткий task test, зафіксувати decision log, lock chosen tokens/components.
7. Лише після цього масштабувати на archive, editor або окремі продукти.

---

## Appendix A — Component Inventory for the First System Pass

| Component | Variant MVP | Нотатка |
|---|---|---|
| App shell | Light/dark, mobile/desktop | Safe areas, max width, bottom tab bar |
| Header | Month selector, theme control | Не перевантажувати settings/AI controls |
| Bottom tab bar | 3 items, active/inactive | Label + icon, 44+ px targets |
| Search | Default, focused, with query, empty | Natural language placeholder; no auto-AI generation |
| Connection row | Default, selected, no-result | Скарга, one hint, bonus; consistent scan lanes |
| Connection detail | Counter, Study-expanded | Customer prompt, script, flash points, products, total |
| Product well/card | VTM, anchor | `contain`, text badge, white/recessed well |
| Money total | Default, expanded breakdown | Tabular number; explanatory label |
| Flash point | Default, important | Semantic icon/text; не лише колір |
| Top product | Grid/list responsive | Не дублює весь detail flow |
| Month summary | Principles + derived stats | Без графіків у першому проході |
| System feedback | Offline, update available, error | Чесно пояснює стан, не блокує контент |

## Appendix B — Design Decision Log Template

```md
## YYYY-MM-DD — [Decision title]

**Status:** proposed | tested | accepted | rejected
**Question:**
**Options considered:**
**Evidence:** device screenshots, task-test results, content constraints
**Decision:**
**Why:**
**Consequences:**
**Owner:**
```

