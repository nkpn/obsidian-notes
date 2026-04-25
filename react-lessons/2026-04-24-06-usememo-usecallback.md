# useMemo & useCallback — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

Калькулятор який пам'ятає останній результат. Якщо вводиш `2 + 2` вдруге — не рахує знову, повертає `4`. Але якщо кожен раз записуєш результат на листочку — витрачаєш більше часу на запис ніж на обчислення.

`useMemo`/`useCallback` — корисні тільки коли обчислення **дорожчі** ніж сама мемоізація.

---

## Key Concepts

- `useMemo` — мемоізує **значення**
- `useCallback` — мемоізує **функцію** (= `useMemo` що повертає функцію)
- Без `React.memo` на дочірньому компоненті — `useCallback` марний
- Перераховуються тільки коли змінились залежності

---

## Quick Example

```jsx
function Parent({ items, onSave }) {
  // ✅ дорогі обчислення — варто мемоізувати
  const sorted = useMemo(() => {
    return items.sort((a, b) => b.price - a.price); // O(n log n)
  }, [items]);

  // ✅ стабільна функція для React.memo компонента
  const handleClick = useCallback((id) => {
    onSave(id);
  }, [onSave]);

  return <ExpensiveChild data={sorted} onClick={handleClick} />;
}

// БЕЗ React.memo — useCallback марний
const ExpensiveChild = React.memo(({ data, onClick }) => {
  return <div>{data.map(...)}</div>;
});
```

---

## Класична Пастка: Stale Closure в useCallback

```jsx
// ❌ НЕПРАВИЛЬНО — userId не в залежностях
const fetchUser = useCallback(async () => {
  fetch(`/api/user/${userId}`) // userId = "123" назавжди
}, []); // userId змінився на "456" — fetchUser не перестворився!

// ✅ ПРАВИЛЬНО
const fetchUser = useCallback(async () => {
  const data = await fetch(`/api/user/${userId}`);
  return data.json();
}, [userId]); // перестворюється коли userId змінюється

// Ланцюжок залежностей працює автоматично:
useEffect(() => {
  fetchUser();
}, [fetchUser]); // userId → fetchUser → useEffect → нові дані
```

---

## Коли НЕ використовувати

```jsx
// ❌ примітивне обчислення — мемоізація дорожча
const value = useMemo(() => a + b, [a, b]);

// ❌ useCallback без React.memo на дочірньому — нічого не дає
const fn = useCallback(() => doSomething(), []);
<Child onClick={fn} /> // Child без React.memo рендериться завжди

// ❌ мемоізація всього підряд — код складніший, перфоманс гірший
```

---

## Коли використовувати

| Ситуація | Інструмент |
|---|---|
| Важкі обчислення (фільтрація/сортування великих масивів) | `useMemo` |
| Стабільна функція для `React.memo` компонента | `useCallback` |
| Стабільна функція в dependency array `useEffect` | `useCallback` |
| Просте додавання / короткий map | ❌ нічого |

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
