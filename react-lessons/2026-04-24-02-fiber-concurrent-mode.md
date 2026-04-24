# Fiber & Concurrent Mode — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

Старий React (15) — кухар який почав готувати і **не може зупинитись** поки не закінчить. Браузер чекає, UI підвисає.

Fiber (React 16+) — новий кухар який готує **маленькими кроками**. Після кожного кроку питає: "є щось важливіше?" Якщо так — відкладає і займається терміновим.

---

## Key Concepts

- **Стара модель (Stack):** рекурсивний обхід дерева — синхронний, не можна перервати
- **Fiber:** кожен компонент = окрема "одиниця роботи" (fiber node), обхід ітеративний
- **Два етапи:**
  - `render phase` — можна переривати, паузити, скасовувати (pure)
  - `commit phase` — синхронна, незворотня (DOM mutations)
- **Пріоритети:** анімації > взаємодія користувача > фонові оновлення
- **Concurrent Mode:** кілька версій UI "в процесі" одночасно

---

## Quick Example

```jsx
import { useTransition, useState } from 'react';

function Search() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    setQuery(e.target.value); // HIGH priority — input миттєво реагує

    startTransition(() => {
      setResults(heavySearch(e.target.value)); // LOW priority — можна почекати
    });
  }

  return (
    <>
      <input value={query} onChange={handleChange} />
      {isPending ? <Spinner /> : <ResultsList data={results} />}
    </>
  );
}
```

---

## Key Insights

- `startTransition` = LOW priority. React може скасувати і переробити якщо прийшло важливіше оновлення
- Без Fiber — поки важкий рендер іде, input підвисає
- З Fiber — React паузить важкий рендер, оновлює input, повертається

---

## Вирішення проблеми: 10к рядків + input підвисає

**3 підходи (senior відповідь):**

1. **`startTransition`** — розділяє пріоритети рендеру
2. **Віртуалізація** (`react-window` / `react-virtual`) — рендерить тільки ~20 видимих рядків
3. **Debounce** — як fallback або для API-запитів

```jsx
import { FixedSizeList } from 'react-window';

<FixedSizeList height={600} itemCount={10000} itemSize={35}>
  {({ index, style }) => <Row style={style} data={rows[index]} />}
</FixedSizeList>
```

---

## Common Interview Traps

- `render phase` може виконуватись **двічі** в StrictMode — side effects тут небезпечні
- `startTransition` не робить код асинхронним — він лише знижує пріоритет
- Concurrent Mode ≠ multi-threading. JS все ще single-threaded

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
