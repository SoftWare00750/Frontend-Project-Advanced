# ES6 Learning Outcomes — Student Tech Store

A fully interactive single-page HTML application demonstrating core ES6 JavaScript concepts through a real-world case study: a Student Tech Store product catalogue. Each concept is presented with syntax-highlighted code, a **▶ Run** button, and a live console output panel.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Learning Outcomes](#learning-outcomes)
- [Case Study Project](#case-study-project)
- [How to Use](#how-to-use)
- [File Structure](#file-structure)
- [Concepts Reference](#concepts-reference)
- [Technologies Used](#technologies-used)

---

## Overview

This project was built as a learning deliverable to demonstrate practical understanding of ES6 JavaScript features. Every concept is applied to a consistent dataset — a 9-product tech store catalogue — so the relationships between features are easy to follow.

The app runs entirely in the browser with no build tools, no frameworks, and no external dependencies.

---

## Learning Outcomes

### 1. `let` & `const`

| Keyword | Purpose | Reassignable |
|---------|---------|-------------|
| `const` | Fixed values — store name, tax rate | ❌ No |
| `let`   | Mutable state — cart total, active user | ✅ Yes |

```js
const STORE_NAME = "Student Tech Store";  // never changes
const TAX_RATE   = 0.075;

let cartTotal  = 0;
let activeUser = "Student";

cartTotal  = 250;      // ✅ OK
activeUser = "Ada";    // ✅ OK
// STORE_NAME = "X";   // ❌ TypeError: Assignment to constant variable
```

---

### 2. Scope

`let` and `const` are **block-scoped** — they are contained within the `{}` they are declared in. `var` (avoid in modern JS) only respects function boundaries and leaks out of blocks.

```js
// Block scope — let
let price = 100;
{
  let price = 80;         // separate variable
  console.log(price);     // 80
}
console.log(price);       // 100 — unchanged

// Function scope — var (leaks!)
var discount = 10;
{
  var discount = 25;      // overwrites outer variable!
}
console.log(discount);    // 25 ⚠️
```

---

### 3. Arrow Functions

A concise ES6 syntax for writing functions. Arrow functions inherit `this` from their surrounding context (no own `this`).

```js
// Single parameter — parentheses optional
const calcTax = price => price * 0.075;

// Multiple parameters — parentheses required
const applyDisc = (price, pct) => price - (price * pct / 100);

// Multi-line body — curly braces + explicit return
const formatPrice = (n, currency) => {
  const formatted = n.toLocaleString();
  return `${currency}${formatted}`;
};

console.log(calcTax(5000));           // 375
console.log(applyDisc(3000, 20));     // 2400
console.log(formatPrice(12000, "₦")); // ₦12,000
```

---

### 4. Array — `map()`

Transforms every element of an array and returns a **new array of the same length**. Never mutates the original.

```js
// Extract product names
const names = products.map(p => p.name);

// Apply 10% price increase
const updatedPrices = products.map(p => ({
  ...p,
  price: +(p.price * 1.10).toFixed(2)
}));

// Build price tag strings
const tags = products.map(p => `${p.name}: ₦${p.price.toLocaleString()}`);
```

---

### 5. Array — `filter()`

Returns a **new array** containing only elements where the callback returns `true`. The original array is unchanged.

```js
// Only laptops
const laptops = products.filter(p => p.category === "laptop");

// Budget-friendly (under ₦80,000)
const budget = products.filter(p => p.price < 80000);

// In-stock items only
const inStock = products.filter(p => p.stock > 0);

// Combine filter + map
const affordableNames = products
  .filter(p => p.price <= 100000)
  .map(p => p.name);
```

---

### 6. Array — `reduce()`

Collapses an entire array into a **single value** by running an accumulator function on each element.

```js
// Sum all prices
const totalValue = products.reduce((acc, p) => acc + p.price, 0);

// Count total stock
const totalStock = products.reduce((acc, p) => acc + p.stock, 0);

// Group products by category → returns an object
const byCategory = products.reduce((acc, p) => {
  acc[p.category] = (acc[p.category] || 0) + 1;
  return acc;
}, {});
// { laptop: 3, phone: 3, accessory: 3 }
```

---

### 7. Array — `forEach()`

Executes a function on each element for **side effects** (logging, DOM updates, accumulating into an external variable). Returns `undefined` — not suitable when you need a new array.

```js
let receiptTotal = 0;

cart.forEach((item, index) => {
  const lineTotal = item.price * item.qty;
  receiptTotal += lineTotal;
  console.log(`${index + 1}. ${item.name} x${item.qty} = ₦${lineTotal.toLocaleString()}`);
});

console.log("TOTAL: ₦" + receiptTotal.toLocaleString());
```

> **When to use what:**
> - Need a transformed array → `map()`
> - Need a filtered subset → `filter()`
> - Need a single value → `reduce()`
> - Need side effects only → `forEach()`

---

### 8. Objects

ES6 introduced shorthand syntax for properties, methods, destructuring, and the spread operator.

```js
// Object literal with ES6 method shorthand
const product = {
  id: 1,
  name: "MacBook Air M2",
  price: 450000,
  category: "laptop",
  inStock: true,
  priceWithTax() { return this.price * 1.075; },       // method shorthand
  summary()      { return `${this.name} — ₦${this.price.toLocaleString()}`; }
};

// Destructuring — extract values by key name
const { name, price, category } = product;

// Spread operator — clone an object and override specific properties
const discounted = { ...product, price: 400000 };

// Object utility methods
console.log(Object.keys(product));    // ["id", "name", "price", ...]
console.log(Object.values(product));  // [1, "MacBook Air M2", 450000, ...]
console.log(Object.entries(product)); // [["id", 1], ["name", "MacBook Air M2"], ...]
```

---

## Case Study Project

The **Student Tech Store** case study combines all eight concepts in a single cohesive script:

```js
// DATA — const + objects
const TAX = 0.075;

const products = [
  { id:1, name:"MacBook Air M2",     price:450000, category:"laptop",    stock:5  },
  { id:2, name:"HP Pavilion 15",     price:280000, category:"laptop",    stock:8  },
  { id:3, name:"Lenovo ThinkPad",    price:320000, category:"laptop",    stock:3  },
  { id:4, name:"iPhone 15",          price:620000, category:"phone",     stock:10 },
  { id:5, name:"Samsung Galaxy A55", price:185000, category:"phone",     stock:12 },
  { id:6, name:"Tecno Camon 30",     price:95000,  category:"phone",     stock:20 },
  { id:7, name:"USB-C Hub 7-in-1",   price:18500,  category:"accessory", stock:30 },
  { id:8, name:"Wireless Mouse",     price:12000,  category:"accessory", stock:25 },
  { id:9, name:'Laptop Sleeve 15"',  price:8500,   category:"accessory", stock:40 },
];

// ARROW FUNCTIONS
const withTax     = price => +(price * (1 + TAX)).toFixed(2);
const formatPrice = n     => `₦${n.toLocaleString()}`;
const isAffordable= (p, budget) => p.price <= budget;

// MAP — build receipt lines
const receiptLines = products.map(p =>
  `${p.name.padEnd(22)} ${formatPrice(p.price)}`
);

// FILTER — products under ₦100,000
const affordable = products
  .filter(p => isAffordable(p, 100000))
  .map(p => p.name);

// REDUCE — totals
const storeValue = products.reduce((s, p) => s + p.price, 0);
const avgPrice   = Math.round(storeValue / products.length);

// FOREACH — print receipt
console.log("=== STUDENT TECH STORE RECEIPT ===");
receiptLines.forEach(line => console.log(line));
console.log("──────────────────────────────────");
console.log("Catalogue value: ", formatPrice(storeValue));
console.log("Average price:   ", formatPrice(avgPrice));
console.log("Under ₦100k:     ", affordable.join(", "));
```

---

## How to Use

1. Download `es6-learning-showcase.html`
2. Open the file in any modern browser (Chrome, Firefox, Edge, Safari)
3. No installation, no terminal, no build step required
4. Click **▶ Run** in any section to execute that code and see output in the console panel below it

The sidebar tracks live store statistics (total products, total catalogue value, average price) — all calculated with `reduce()` on page load.

---

## File Structure

```
es6-learning-outcomes/
│
├── es6-learning-showcase.html   # Main application (self-contained)
└── README.md                    # This file
```

Everything — HTML, CSS, and JavaScript — lives in a single file for simplicity and portability.

---

## Concepts Reference

| Concept | ES6? | Key Behaviour |
|---------|------|--------------|
| `const` | ✅ | Block-scoped, cannot be reassigned |
| `let` | ✅ | Block-scoped, can be reassigned |
| `var` | ❌ (avoid) | Function-scoped, hoisted, leaks from blocks |
| Arrow function `=>` | ✅ | Concise syntax, no own `this` binding |
| Block scope `{}` | ✅ | `let`/`const` stay inside their block |
| `array.map()` | — | Returns new array, same length, transformed |
| `array.filter()` | — | Returns new array, shorter or equal, matching |
| `array.reduce()` | — | Returns single accumulated value |
| `array.forEach()` | — | Side effects only, returns `undefined` |
| Object destructuring | ✅ | Extract named properties into variables |
| Spread operator `...` | ✅ | Clone objects/arrays with optional overrides |
| Template literals `` ` ` `` | ✅ | Embedded expressions, multi-line strings |
| Method shorthand | ✅ | `method() {}` instead of `method: function() {}` |

---

## Technologies Used

- **Vanilla JavaScript (ES6+)** — no frameworks or libraries
- **HTML5** — semantic structure
- **CSS3** — custom properties, grid, flexbox, transitions
- **Google Fonts** — JetBrains Mono (code), Sora (UI)

---

*Built as a student learning deliverable covering ES6 features, scope, arrow functions, array methods, and objects.*
