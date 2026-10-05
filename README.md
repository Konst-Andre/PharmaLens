> живе доки: назавжди (вхід у репо продукту) · розкладка — Р-7, `lens-governance:kernel/Lens_REPO_LAYOUT.md` §4

# PharmaLens

5-й продукт сімейства Lens (ребренд робочої назви VTM Lens, 30.07.2026): навчальний інструмент по SKU для аптечної мережі. **Заморожено** — чекає презентацію `.pptx` від Олі (`lens/PharmaLens_CHERGA.md`, `PL-1`). Коду ще нема.
Правила роботи — у ядрі родини [`Konst-Andre/lens-governance`](https://github.com/Konst-Andre/lens-governance). Цей репо самодостатній.

## Де що

| тека | роль |
|---|---|
| `lens/` | канон: `PharmaLens_INDEX.md` · `PharmaLens_CHERGA.md` · `PharmaLens_MASTER_LOCK.md` · два брифи-хендовери |
| `sessions/` | живі самері (стеля 2) |
| `archive/` | витіснене |
| `docs/` | майбутній сайт (поки порожньо) |

## Як почати сесію

Сесія Claude Code: **першим — адаптувати репо під каркас** за `lens-governance:tools/claude-code/ADOPT.md` (тут ще нема `CLAUDE.md`, `tools/env_check.sh`, журналу аудиту — `frame_check` покаже). Далі: `lens/PharmaLens_CHERGA.md` цілком → самері в `sessions/` (§0 «ХВОСТИ»).
Гейт продукту: `python3 <lens-governance>/kernel/Lens_validate.py --product .`
