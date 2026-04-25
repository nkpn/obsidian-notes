# useRef: DOM Access vs Invisible Variable — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

`useRef` — це стікер на холодильнику. Ти можеш змінити що на ньому написано, але холодильник від цього не "оновлюється" — ніхто не перемальовує кухню. На відміну від `useState` — зміна ref **не викликає ре-рендер**.

---

## Key Concepts

- `useRef` повертає `{ current: value }` — мутабельний об'єкт
- Зміна `.current` — **не тригерить ре-рендер**
- Два use-cases:
  1. Доступ до DOM елементу
  2. Збереження значення між рендерами без ре-рендеру
- Ref зберігається між рендерами (на відміну від звичайної змінної всередині компонента)

---

## Quick Example

```jsx
function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null); // не потрібен в UI — не useState

  function start() {
    intervalRef.current = setInterval(() => setCount(c => c + 1), 1000);
  }

  function stop() {
    clearInterval(intervalRef.current);
  }

  // DOM access
  const inputRef = useRef(null);
  function focusInput() {
    inputRef.current.focus(); // прямий доступ до DOM
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus</button>
      <p>{count}</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </>
  );
}
```

---

## Класична Пастка: Previous Value

```jsx
// НЕПРАВИЛЬНО — prevRef.current завжди дорівнює count
function App() {
  const [count, setCount] = useState(0);
  const prevRef = useRef(0);

  prevRef.current = count; // виконується під час рендеру → завжди актуальне значення
  // тому Previous і Current показують одне й те саме!
}

// ПРАВИЛЬНО — оновлюємо після рендеру
function App() {
  const [count, setCount] = useState(0);
  const prevRef = useRef(0);

  useEffect(() => {
    prevRef.current = count; // після того як UI вже намалювався
  });

  return (
    <div>
      <p>Current: {count}</p>
      <p>Previous: {prevRef.current}</p> {/* тепер правильно */}
    </div>
  );
}
```

**Порядок подій:**
```
клік → setCount(1) → рендер (count=1) → UI → useEffect → prevRef.current = 1
клік → setCount(2) → рендер (count=2, prevRef=1) → UI показує Previous: 1 ✓
```

---

## useState vs useRef

| | useState | useRef |
|---|---|---|
| Тригерить ре-рендер | ✅ | ❌ |
| Зберігається між рендерами | ✅ | ✅ |
| Використовувати для UI | ✅ | ❌ |
| Використовувати для DOM/таймерів | ❌ | ✅ |

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
