# Reconciliation & Virtual DOM — React Interview Prep

**Date:** 2026-04-24  
**Level:** Middle/Senior  
**Est. time:** ~10 min

---

## Feynman Explanation

React — це розумний маляр. Замість того щоб перефарбовувати всю стіну (реальний DOM), він спочатку малює ескіз на папері (Virtual DOM), порівнює з попереднім ескізом, і фарбує **тільки те що змінилось**.

---

## Key Concepts

- Virtual DOM — lightweight JS-об'єкт, копія реального DOM
- При кожному ре-рендері React будує **новий** VDOM і робить **diff** зі старим
- Алгоритм O(n) замість O(n³) — завдяки двом припущенням:
  - різні типи елементів = різні дерева
  - `key` стабілізує списки
- **Fiber** (React 16+) — новий движок: розбиває роботу на шматки, може паузити/пріоритизувати

---

## Quick Example

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1>        {/* перерендериться — count змінився */}
      <HeavyChart />          {/* НЕ перерендериться — пропси ті самі */}
      <button onClick={() => setCount(c => c + 1)}>+</button>
    </div>
  );
}
```

---

## Key Insight про `key`

`key` — ідентифікатор для React щоб зматчити старий VDOM-вузол з новим.  
Якщо `key` змінився — React вважає це **абсолютно новим елементом**: unmount + mount.

**Проблема з `index` як `key`:**

```
До видалення:     Після видалення першого:
key=0 → Alice     key=0 → Bob    ← React: "новий елемент, mount"
key=1 → Bob       key=1 → Carol  ← React: "новий елемент, mount"
key=2 → Carol                    ← React: "зник, unmount"
```

React видаляє Carol і перестворює всіх. З `key=id` — тільки update.

**Практичний біль:** якщо в елементі є `<input>` — при index-key він **скинеться**. Втрата фокусу, введеного тексту, анімацій.

**Коли index — ок:** статичний список без видалення/сортування.

---

## Common Interview Traps

- "Virtual DOM швидший за реальний DOM" — **неправда**. Він просто мінімізує кількість операцій з реальним DOM
- Не плутати reconciliation (порівняння) з rendering (малювання)
- `key` має бути стабільним між рендерами — не `Math.random()`

---

*Part of the React/Next.js Interview Prep series — Feynman Method*
