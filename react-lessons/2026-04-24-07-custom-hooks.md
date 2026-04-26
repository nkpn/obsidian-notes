# Custom Hooks — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

Custom hook — це як електрична розетка. Ти один раз прокладаєш проводку в стіні (логіка), і потім будь-яка кімната просто втикає вилку (викликає хук). Не треба кожен раз прокладати нові дроти.

---

## Key Concepts

- Custom hook = функція що починається з `use` і викликає інші хуки
- Виносить **логіку** з компонента — компонент залишається чистим
- Кожен виклик хука — **окремий стан** (не shared)
- Повертає будь-що: значення, функції, масиви, об'єкти

---

## Quick Example: useFetch

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(r => r.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}

// Компонент читається як книга
function UserProfile({ userId }) {
  const { data: user, loading, error } = useFetch(`/api/user/${userId}`);

  if (loading) return <Spinner />;
  if (error) return <Error />;
  return <div>{user.name}</div>;
}
```

---

## Практичне Завдання: useLocalStorage

```jsx
const useLocalStorage = (key, initialValue) => {
  const [value, setValue] = useState(() => {
    const storedValue = localStorage.getItem(key);         // змінна key, не рядок!
    return storedValue ? JSON.parse(storedValue) : initialValue; // parse при читанні
  });

  useEffect(() => {
    if (value !== undefined) localStorage.setItem(key, JSON.stringify(value)); // stringify при записі
  }, [value, key]);

  return [value, setValue]; // як useState — масив!
};

// Використання:
const [theme, setTheme] = useLocalStorage('theme', 'light');
```

**Типові помилки:**
- `localStorage` з малої — JS чутливий до регістру
- `'key'` замість `key` — рядок vs змінна
- `JSON.parse` замість `JSON.stringify` при записі
- `return value, setValue` — кома-оператор повертає останнє, треба `[value, setValue]`

---

## Common Interview Traps

- Custom hook НЕ шерить стан між компонентами — кожен виклик = окремий стан
- Назва обов'язково з `use` — інакше React не перевіряє rules of hooks
- Можна викликати хуки всередині custom hook — але не в умовах/циклах

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
