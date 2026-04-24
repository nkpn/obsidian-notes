# useState Batching — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

Офіціант який записує замовлення. Якщо кожен гість кричить окремо — офіціант бігає на кухню після кожного слова. Розумний офіціант **чекає поки всі скажуть** і йде один раз.

Batching — React збирає кілька `setState` в один ре-рендер.

---

## Key Concepts

- **React 17:** batching тільки в React event handlers. В `setTimeout`, `Promise`, `async` — кожен setState = окремий ре-рендер
- **React 18:** **Automatic Batching** — batching скрізь, завжди
- `flushSync` — примусовий негайний рендер (рідко потрібно)

---

## Quick Example

```jsx
// React 17: 2 ре-рендери в setTimeout
// React 18: 1 ре-рендер (automatic batching)
function handleClick() {
  setTimeout(() => {
    setCount(c => c + 1);
    setFlag(f => !f);
    // React 18: обидва збираються → 1 ре-рендер
  }, 1000);
}

// Примусово два окремі рендери:
import { flushSync } from 'react-dom';
flushSync(() => setCount(c => c + 1)); // рендер одразу
flushSync(() => setFlag(f => !f));     // ще один рендер
```

---

## Key Insight: Stale Closure пастка

```jsx
// НЕПРАВИЛЬНО — count буде 1, не 3!
setCount(count + 1);
setCount(count + 1);
setCount(count + 1); // всі три читають один і той самий count з closure

// ПРАВИЛЬНО — функціональний update
setCount(c => c + 1);
setCount(c => c + 1);
setCount(c => c + 1); // count = 3 ✓
```

---

## Async Batching: React 17 vs React 18

```jsx
async function handleClick() {
  setA(1);
  setB(1);
  await fetch('/api/data'); // <-- розриває синхронний контекст
  setA(2);
  setB(2);
}
```

| | React 17 | React 18 |
|---|---|---|
| До await | 1 рендер (1,1) | 1 рендер (1,1) |
| Після await | 2 рендери (2,1) → (2,2) | 1 рендер (2,2) |
| **Всього** | **3 рендери** | **2 рендери** |

`await` розриває синхронний контекст. React 17 після `await` не "знає" що це той самий event handler. React 18 вирішив через automatic batching на рівні планувальника.

---

## Common Interview Traps

- Batching і stale closure — різні проблеми, не плутати
- `flushSync` всередині `startTransition` = помилка
- Functional update (`c => c + 1`) вирішує stale closure, але не впливає на batching

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
