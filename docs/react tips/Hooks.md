### Where and When to Create a Custom React Hook

Custom React hooks should be created when you have reusable logic that is related to React functionality, such as state, lifecycle methods, or context. Hooks make your code modular, easy to test, and shareable between components.

---

### **Examples of When to Create a Hook**

#### **1. Managing Form State**

**Scenario:**  
You have multiple forms in your application that require managing state, validation, and submission logic.

**Custom Hook:**  
Create a hook like `useForm` to encapsulate form-related logic.  
**Example:**

```tsx
import { useState } from 'react';

function useForm(initialValues) {
    const [values, setValues] = useState(initialValues);

    const handleChange = (e) => {
        const { name, value } = e.target;
        setValues((prevValues) => ({ ...prevValues, [name]: value }));
    };

    const resetForm = () => setValues(initialValues);

    return { values, handleChange, resetForm };
}
```

**Usage:**

```tsx
function LoginForm() {
    const { values, handleChange, resetForm } = useForm({ email: '', password: '' });

    const handleSubmit = (e) => {
        e.preventDefault();
        console.log(values);
        resetForm();
    };

    return (
        <form onSubmit={handleSubmit}>
            <input name="email" value={values.email} onChange={handleChange} />
            <input name="password" type="password" value={values.password} onChange={handleChange} />
            <button type="submit">Submit</button>
        </form>
    );
}
```

---

#### **2. Fetching Data from an API**

**Scenario:**  
You have components that need to fetch data from an API and handle loading and error states.

**Custom Hook:**  
Create a hook like `useFetch` to handle API calls.  
**Example:**

```tsx
import { useState, useEffect } from 'react';

function useFetch(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        setLoading(true);
        fetch(url)
            .then((response) => response.json())
            .then((data) => {
                setData(data);
                setLoading(false);
            })
            .catch((error) => {
                setError(error);
                setLoading(false);
            });
    }, [url]);

    return { data, loading, error };
}
```

**Usage:**

```tsx
function UsersList() {
    const { data, loading, error } = useFetch('https://jsonplaceholder.typicode.com/users');

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error.message}</p>;

    return (
        <ul>
            {data.map((user) => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}
```

---

#### **3. Handling Window Resize Events**

**Scenario:**  
You need to monitor the browser window size and trigger responsive behavior in multiple components.

**Custom Hook:**  
Create a hook like `useWindowSize` to listen for resize events and provide updated dimensions.  
**Example:**

```tsx
import { useState, useEffect } from 'react';

function useWindowSize() {
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight,
    });

    useEffect(() => {
        const handleResize = () => {
            setWindowSize({
                width: window.innerWidth,
                height: window.innerHeight,
            });
        };

        window.addEventListener('resize', handleResize);
        return () => window.removeEventListener('resize', handleResize);
    }, []);

    return windowSize;
}
```

**Usage:**

```tsx
function ResponsiveComponent() {
    const { width, height } = useWindowSize();

    return (
        <div>
            <p>Window width: {width}px</p>
            <p>Window height: {height}px</p>
        </div>
    );
}
```

---

### **Benefits of Using Custom Hooks**

1. **Reusability:** Extract logic into a single location to use across multiple components.
2. **Clean Code:** Reduce clutter in components by moving logic out.
3. **Testability:** Simplify testing by isolating logic in a hook.
4. **Consistency:** Standardize complex functionality across the application.

Custom hooks help make your codebase modular and easy to maintain.