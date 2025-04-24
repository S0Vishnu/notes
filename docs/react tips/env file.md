
In a **React TypeScript** project, `.env` files work similarly to JavaScript, but there are some important things to consider. Here’s a step-by-step guide:

---

### **1. Create `.env` and `.env.local` Files**

In the root of your React project, create a `.env` file:

```env
# .env
REACT_APP_API_URL=https://api.example.com
REACT_APP_PUBLIC_KEY=12345
```

And a `.env.local` file for local development overrides:

```env
# .env.local
REACT_APP_API_URL=http://localhost:5000
```

> ⚠️ In **React (Vite, Create React App)**, all environment variables **must** start with `REACT_APP_` (for CRA) or `VITE_` (for Vite).

---

### **2. Access Environment Variables in Code**

Since you're using **TypeScript**, you need to type-check `process.env`.

#### **For Create React App (CRA):**

```tsx
console.log(process.env.REACT_APP_API_URL);
```

#### **For Vite (Recommended for React TS):**

Vite uses `import.meta.env` instead of `process.env`:

```tsx
console.log(import.meta.env.VITE_API_URL);
```

---

### **3. TypeScript Support for Environment Variables**

If you're using TypeScript, it’s good practice to define types for environment variables.

#### **For Create React App (CRA)**

Create a `react-app-env.d.ts` file in `src/`:

```ts
/// <reference types="react-scripts" />
```

Alternatively, extend the `NodeJS.ProcessEnv` type in `env.d.ts`:

```ts
declare namespace NodeJS {
  interface ProcessEnv {
    REACT_APP_API_URL: string;
    REACT_APP_PUBLIC_KEY: string;
  }
}
```

#### **For Vite (Recommended)**

Create a `env.d.ts` file:

```ts
interface ImportMetaEnv {
  readonly VITE_API_URL: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

---

### **4. Priority of Environment Files**

React will load files in the following order (**last one wins**):

1. `.env.local`
    
2. `.env.development`, `.env.production` (if applicable)
    
3. `.env`
    

---

### **5. Ignore `.env.local` in Git**

To prevent exposing sensitive data, add `.env.local` to `.gitignore`:

```
.env.local
.env
```

---

### **6. Restart Your Server**

Changes to `.env` files require a restart of the development server.

```bash
npm run dev  # For Vite
npm start    # For CRA
```

---

### **7. Production Setup**

In production, set environment variables directly in the deployment platform (e.g., **Vercel, Netlify, Docker, or CI/CD pipelines**) instead of relying on `.env` files.

---

### **Summary**

✅ Use `.env` for defaults, `.env.local` for local overrides  
✅ Always prefix variables with `REACT_APP_` (for CRA) or `VITE_` (for Vite)  
✅ Use TypeScript types for better safety  
✅ Ignore `.env.local` in Git  
✅ Restart the dev server after changes
