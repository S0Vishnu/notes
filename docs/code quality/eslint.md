
### 🔧 ESLint Disable Directives (Common Rules)

#### ✅ General Syntax

```ts
// eslint-disable-next-line <rule-name>
```

---

This is useful when:

- You know a warning is a false positive
- You're writing intentional code that's technically against the rules
- You're gradually fixing issues but want to suppress some temporarily

---

### 📘 Usage Examples & Explanations

#### 1. **`no-console`**

```ts
// eslint-disable-next-line no-console
console.log("Debug info");
```

> ✅ **Why**: In production, `console.log` is usually discouraged — but it’s fine during development.

---

#### 2. **`no-unused-vars`**

```ts
// eslint-disable-next-line no-unused-vars
const temp = 'test';
```

> ✅ **Why**: If you're scaffolding code or importing something not used yet.

---

#### 3. **`react-hooks/exhaustive-deps`**

```ts
// eslint-disable-next-line react-hooks/exhaustive-deps
useEffect(() => {
  doSomething();
}, []);
```

> ✅ **Why**: Sometimes you intentionally want the hook to run only once, not re-run on every dependency change.

---

#### 4. **`no-shadow`**

```ts
let user = 'outer';
function doSomething() {
  // eslint-disable-next-line no-shadow
  let user = 'inner';
}
```

> ✅ **Why**: You might want to reuse variable names in different scopes, though ESLint warns against this.

---

#### 5. **Without a specific rule (disables all rules)**

```ts
// eslint-disable-next-line
anySketchyCodeHere();
```

> ⚠️ **Why**: Suppresses all ESLint warnings for that line. Use **only** when absolutely necessary.

---

### ⚠️ Best Practices

- ✅ **Comment why** you're disabling a rule:

```ts
// eslint-disable-next-line no-console -- allowed in dev mode
console.log("Debug info");
```

- ❌ **Avoid overuse**. It's better to adjust code to follow rules unless there's a real reason.

### 📋 Common ESLint Rules You Might Ignore

|Rule Name|Purpose|Example Use|
|---|---|---|
|`no-console`|Disallows `console.log()`|Debug logging|
|`no-unused-vars`|Flags unused variables|Temporary code|
|`no-empty`|Prevents empty blocks|Placeholder logic|
|`no-magic-numbers`|Warns on raw numbers|`const size = 42`|
|`no-shadow`|Disallows variable shadowing|`let x` inside a scope|
|`no-undef`|Prevents use of undeclared vars|Global injected by env|
|`no-param-reassign`|Stops modifying function params|Modifying `req` or `res`|
|`no-use-before-define`|Prevents use before declaration|Hoisting-related|
|`eqeqeq`|Requires `===` instead of `==`|Loose equality on purpose|
|`complexity`|Warns if function is too complex|Exception for trusted logic|
|`max-lines` / `max-lines-per-function`|Restricts size of files/functions|Temporarily ignore for larger blocks|
|`no-return-await`|Warns when `await` is used on return|Intentional double-await|
|`no-restricted-globals`|Blocks access to certain globals|Like `event`, `location`|
|`consistent-return`|Ensures functions always return consistently|Mixed `return` and no `return`|
|`prefer-const`|Suggests using `const` if variable is never reassigned|`let` used for clarity|
|`no-async-promise-executor`|Disallows async inside `new Promise()`|Use case where it’s safe|

---

### 🧠 Example

```ts
// eslint-disable-next-line no-unused-vars
const temp = 42;

// eslint-disable-next-line no-console
console.log("Debug info");

// eslint-disable-next-line no-shadow
function process(items: string[]) {
  items.forEach(item => {
    const items = item.split(','); // shadowing outer 'items'
  });
}
```

### 🧩 Examples with Reasons

```ts
// eslint-disable-next-line no-console -- logging needed for debugging
console.log("Debug info");

// eslint-disable-next-line react-hooks/exhaustive-deps -- intentionally run once on mount
useEffect(() => {
  fetchData();
}, []);

// eslint-disable-next-line no-unused-vars -- placeholder for future use
const tempResult = computeSomething();

// eslint-disable-next-line no-shadow -- reusing variable name in local scope
function process(users) {
  users.forEach(user => {
    // eslint-disable-next-line no-shadow -- inner user overrides outer intentionally
    const user = transform(user);
  });
}
```
