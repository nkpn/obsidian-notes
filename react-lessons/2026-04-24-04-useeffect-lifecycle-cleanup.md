# useEffect: Lifecycle & Cleanup — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

`useEffect` — це як підписка на газету. Ти підписуєшся (effect), читаєш новини, а коли переїжджаєш — **скасовуєш підписку** (cleanup). Якщо не скасуєш — газети будуть приходити на стару адресу вічно (memory leak).

---

## Key Concepts

- useEffect = `componentDidMount` + `componentDidUpdate` + `componentWillUnmount` в одному
- **Cleanup функція** — повертається з useEffect, викликається перед наступним запуском або unmount
- **Dependency array** керує коли effect запускається
- В **StrictMode** (dev) — React навмисно монтує/демонтує **двічі** щоб виявити проблеми

---

## Quick Example

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    const socket = connectToRoom(roomId); // підключились

    return () => {
      socket.disconnect(); // cleanup — відключились перед наступним roomId
    };
  }, [roomId]);
}

// Три варіанти dependency array:
useEffect(() => { /* кожен рендер */ });           // без масиву
useEffect(() => { /* тільки mount */ }, []);        // порожній масив
useEffect(() => { /* коли змінився id */ }, [id]);  // залежності
```

---

## StrictMode Поведінка (важливо!)

```
// Development + StrictMode:
connected      ← mount #1
disconnected   ← React демонтує (перевіряє cleanup)
connected      ← mount #2 (реальний)

// Production: тільки один mount
```

> `connected` виводиться **2 рази** в StrictMode dev. В production — 1 раз.

---

## Common Interview Traps

```jsx
// ПАСТКА 1: нескінченний цикл
useEffect(() => {
  setData(transform(data)); // змінює data → тригерить effect → змінює data...
}, [data]);

// ПАСТКА 2: пропущений cleanup
useEffect(() => {
  const id = setInterval(() => setCount(c => c + 1), 1000);
  return () => clearInterval(id); // ← без цього interval живе після unmount
}, []);

// ПАСТКА 3: stale closure
useEffect(() => {
  const id = setInterval(() => {
    console.log(count); // завжди 0! closure захопила початковий count
  }, 1000);
  return () => clearInterval(id);
}, []); // count не в залежностях — stale closure
```

---

## Правило

> Якщо effect щось "відкриває" — cleanup повинен це "закрити". Завжди.

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
