
**Folder and File Structure**

```plaintext
project-name/
├── public/
│   ├── index.html
│   ├── favicon.ico
│   └── assets/
│       ├── images/
│       ├── fonts/
│       └── icons/
├── src/
│   ├── components/
│   │   ├── Dashboard/
│   │   │   ├── Dashboard.tsx
│   │   │   ├── Dashboard.module.scss
│   │   │   └── SummaryCard.tsx
│   │   ├── Finance/
│   │   │   ├── FinanceTracker.tsx
│   │   │   ├── FinanceTracker.module.scss
│   │   │   └── ExpenseForm.tsx
│   │   ├── Health/
│   │   │   ├── HealthTracker.tsx
│   │   │   ├── HealthTracker.module.scss
│   │   │   ├── MoodLog.tsx
│   │   │   └── SleepLog.tsx
│   │   ├── Fitness/
│   │   │   ├── FitnessTracker.tsx
│   │   │   ├── FitnessTracker.module.scss
│   │   │   └── ExerciseLog.tsx
│   │   └── Shared/
│   │       ├── Navbar.tsx
│   │       ├── Footer.tsx
│   │       ├── Button.tsx
│   │       └── Card.tsx
│   ├── hooks/
│   │   ├── useLocalStorage.ts
│   │   └── useTheme.ts
│   ├── layouts/
│   │   └── MainLayout.tsx
│   ├── pages/
│   │   ├── HomePage.tsx
│   │   ├── FinancePage.tsx
│   │   ├── HealthPage.tsx
│   │   ├── FitnessPage.tsx
│   │   └── DashboardPage.tsx
│   ├── styles/
│   │   ├── base/
│   │   │   ├── _reset.scss
│   │   │   ├── _variables.scss
│   │   │   ├── _mixins.scss
│   │   │   └── _typography.scss
│   │   ├── components/
│   │   │   ├── _button.scss
│   │   │   ├── _card.scss
│   │   │   └── _navbar.scss
│   │   └── main.scss
│   ├── types/
│   │   ├── finance.d.ts
│   │   ├── health.d.ts
│   │   ├── fitness.d.ts
│   │   └── shared.d.ts
│   ├── utils/
│   │   └── helpers.ts
│   ├── App.tsx
│   ├── App.module.scss
│   ├── index.tsx
│   └── reportWebVitals.ts
├── .env
├── .gitignore
├── tsconfig.json
├── package.json
├── README.md
├── webpack.config.js (if needed)
└── yarn.lock / package-lock.json
```

---

### **Key TypeScript-Specific Adjustments**

#### **1. `types/`**

This folder contains type declarations for the app. Here's a breakdown:

- **`finance.d.ts`**:
    
    ```ts
    export interface Expense {
      id: string;
      category: string;
      amount: number;
      date: Date;
    }
    
    export interface Income {
      id: string;
      amount: number;
      frequency: 'monthly' | 'weekly';
      date: Date;
    }
    ```
    
- **`health.d.ts`**:
    
    ```ts
    export interface MoodLog {
      date: Date;
      mood: string; // emoji or label
    }
    
    export interface SleepLog {
      date: Date;
      hours: number;
    }
    ```
    
- **`fitness.d.ts`**:
    
    ```ts
    export interface ExerciseLog {
      date: Date;
      type: string; // e.g., 'Running', 'Walking'
      duration: number; // in minutes
    }
    ```
    
- **`shared.d.ts`**:
    
    ```ts
    export interface CardProps {
      title: string;
      value: string | number;
      icon?: string;
    }
    
    export interface ButtonProps {
      label: string;
      onClick: () => void;
      type?: 'button' | 'submit';
      disabled?: boolean;
    }
    ```
    

#### **2. TypeScript in Components**

Example: **Dashboard.tsx**

```tsx
import React from 'react';
import styles from './Dashboard.module.scss';
import { CardProps } from '../../types/shared';
import SummaryCard from './SummaryCard';

const Dashboard: React.FC = () => {
  const cards: CardProps[] = [
    { title: 'Finance', value: '$200 Saved' },
    { title: 'Health', value: '7 Hrs Sleep Avg' },
    { title: 'Fitness', value: '10K Steps Avg' },
  ];

  return (
    <div className={styles.dashboard}>
      <h1>Dashboard</h1>
      <div className={styles.summaryCards}>
        {cards.map((card, index) => (
          <SummaryCard key={index} {...card} />
        ))}
      </div>
    </div>
  );
};

export default Dashboard;
```

---

#### **3. Hooks in TypeScript**

Example: **`useLocalStorage.ts`**

```ts
import { useState } from 'react';

function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue] as const;
}

export default useLocalStorage;
```

---

### **TypeScript Configuration (`tsconfig.json`)**

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ESNext",
    "strict": true,
    "jsx": "react-jsx",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "baseUrl": "./src",
    "paths": {
      "@components/*": ["components/*"],
      "@hooks/*": ["hooks/*"],
      "@types/*": ["types/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

### **Dependencies**

Install these TypeScript-specific dependencies:

```bash
yarn add react-router-dom sass classnames
yarn add --dev typescript @types/react @types/react-dom @types/react-router-dom
```

---

### **Benefits of TypeScript Integration**

1. **Type Safety**: Clear definition of data structures (e.g., `Expense`, `MoodLog`).
2. **Scalability**: Ensures code is maintainable as features grow.
3. **Error Reduction**: Early detection of potential runtime errors during development.
4. **Enhanced Developer Experience**: Better IntelliSense support in IDEs.

## **.module.scss**

The `.module.scss` files contain component-specific styles scoped only to the associated component when using CSS Modules. This avoids global scope pollution and ensures styles are encapsulated. Below is an example of what might be written in `.module.scss` files for your project.

---

### **Example 1: `Dashboard.module.scss`**

```scss
.dashboard {
  padding: 2rem;
  background-color: #f9f9f9;

  h1 {
    font-size: 2rem;
    color: #4caf50; // Example primary color
    margin-bottom: 1.5rem;
  }

  .summaryCards {
    display: flex;
    gap: 1.5rem;
    flex-wrap: wrap;

    @media (max-width: 768px) {
      flex-direction: column;
    }
  }
}
```

### **Example 2: `SummaryCard.module.scss`**

```scss
.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  flex: 1;
  min-width: 250px;
  max-width: 300px;
  text-align: center;

  h2 {
    font-size: 1.5rem;
    color: #333;
    margin-bottom: 1rem;
  }

  .value {
    font-size: 2rem;
    color: #4caf50; // Example primary color
    font-weight: bold;
  }

  &:hover {
    transform: scale(1.05);
    transition: transform 0.3s ease-in-out;
  }
}
```

### **Example 3: `FinanceTracker.module.scss`**

```scss
.financeTracker {
  padding: 1.5rem;

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;

    h2 {
      font-size: 1.5rem;
      color: #4caf50;
    }

    button {
      padding: 0.5rem 1rem;
      background-color: #4caf50;
      border: none;
      border-radius: 4px;
      color: white;
      font-weight: bold;
      cursor: pointer;

      &:hover {
        background-color: #388e3c;
      }
    }
  }

  .expenseList {
    margin-top: 1.5rem;

    .expenseItem {
      display: flex;
      justify-content: space-between;
      padding: 0.75rem;
      border-bottom: 1px solid #ddd;

      &.expense {
        color: #f44336;
      }

      &.income {
        color: #4caf50;
      }
    }
  }
}
```

### **Example 4: `HealthTracker.module.scss`**

```scss
.healthTracker {
  padding: 1.5rem;

  .moodLog {
    display: flex;
    align-items: center;
    gap: 1rem;

    .moodIcon {
      font-size: 2rem;
    }

    .date {
      font-size: 0.9rem;
      color: #666;
    }
  }

  .sleepLog {
    margin-top: 1.5rem;

    .logEntry {
      display: flex;
      justify-content: space-between;
      padding: 0.75rem;
      border-bottom: 1px solid #ddd;

      .hours {
        font-weight: bold;
        color: #4caf50;
      }
    }
  }
}
```

### **Example 5: `Button.module.scss`**

```scss
.button {
  padding: 0.5rem 1rem;
  background-color: #4caf50;
  border: none;
  border-radius: 4px;
  color: white;
  font-weight: bold;
  cursor: pointer;

  &:hover {
    background-color: #388e3c;
    transition: background-color 0.3s ease-in-out;
  }

  &.secondary {
    background-color: #ff9800;

    &:hover {
      background-color: #fb8c00;
    }
  }

  &.disabled {
    background-color: #ddd;
    cursor: not-allowed;
  }
}
```

---

### **Structure of `.module.scss`**

1. **Root Classes**:
    
    - Define the primary styles for the component, like `dashboard`, `financeTracker`, or `healthTracker`.
2. **Child Elements**:
    
    - Nest styles under the root class to scope them (`.header`, `.expenseList`, `.moodLog`, etc.).
3. **State Styles**:
    
    - Use additional classes for hover, focus, or other state changes (`&:hover`, `&.disabled`, etc.).
4. **Responsive Styles**:
    
    - Add media queries for responsive designs (`@media`).
5. **Theme Colors**:
    
    - Use SCSS variables like `$primary-color` or `$secondary-color` to maintain consistency.

---

### **Benefits of `.module.scss`**

1. **Encapsulation**: The styles are scoped to the component and won’t affect or be affected by other styles.
2. **Readability**: Clear association between a component and its styles.
3. **Modularity**: Easier to maintain and update styles as the project grows.

This approach ensures clean, reusable, and maintainable styles for your project.

