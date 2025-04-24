### **What is `tabIndex={0}` in HTML/JSX?**

`tabIndex={0}` is an attribute used in HTML (or JSX when working with React) that makes an element focusable and places it into the **natural tab order** of the page. This means the element can be navigated to using the `Tab` key on a keyboard, just like native interactive elements such as `<button>`, `<input>`, or `<a>`.

---

### **Key Points About `tabIndex`**

1. **Default Behavior:**
    
    - **Interactive elements (like `<button>`, `<a>`, `<input>`)**:
        
        - Automatically focusable and part of the natural tab order.
            
    - **Non-interactive elements (like `<div>` or `<span>`)**:
        
        - Not focusable by default. You need to explicitly add `tabIndex` to make them keyboard-accessible.
            
2. **`tabIndex` Values:**
    
    - **`tabIndex={0}`**:
        
        - Adds the element to the natural tab order based on its position in the DOM.
            
        - Allows keyboard users to navigate to the element using the `Tab` key.
            
    - **`tabIndex={-1}`**:
        
        - Removes the element from the tab order (it won't be reachable via `Tab`), but it can still be focused programmatically (e.g., via JavaScript's `.focus()` method).
            
    - **`tabIndex={1}` or higher**:
        
        - Manually specifies the tab order priority. Higher numbers indicate a lower priority, but using `tabIndex` values greater than `0` is discouraged because it overrides the natural DOM order, which can confuse users.
            

---

### **Why Use `tabIndex={0}`?**

1. **Making Non-Interactive Elements Focusable:**
    
    - By default, elements like `<div>` or `<span>` are not part of the tab order. Adding `tabIndex={0}` allows users to focus on them with the keyboard.
        
    - This is useful when you make these elements interactive (e.g., adding `onClick` behavior).
        
    
    **Example:**
    
    ```jsx
    <div
      tabIndex={0}
      onClick={() => console.log("Clicked")}
    >
      Clickable Div
    </div>
    ```
    
    Without `tabIndex={0}`, keyboard users would be unable to navigate to this element or interact with it.
    
2. **Improving Accessibility:**
    
    - Adding `tabIndex={0}` ensures the element is reachable for keyboard and assistive technology users, such as screen readers.
        
3. **Custom Widgets:**
    
    - When building custom UI components (like dropdowns, modals, or sliders) that are not natively interactive, you can make them accessible with `tabIndex={0}`.
        

---

### **When Should You Use `tabIndex={0}`?**

Use `tabIndex={0}` when:

1. You need a non-interactive element to behave like an interactive one (e.g., `<div>` acting as a button or focusable region).
    
2. You want to include custom interactive elements in the keyboard navigation flow.
    
3. You're building custom widgets or components that need focus management.
    

---

### **Best Practices for `tabIndex={0}`**

1. **Use Native Elements Whenever Possible:**
    
    - Prefer `<button>`, `<a>`, or other native interactive elements over non-interactive elements with `tabIndex`.
        
    - Native elements are automatically keyboard-focusable and provide built-in accessibility.
        
2. **Don't Overuse `tabIndex`:**
    
    - Only use `tabIndex={0}` for elements that truly need to be interactive or focusable.
        
    - Avoid making too many elements focusable, as this can overwhelm keyboard users.
        
3. **Combine with ARIA Roles:**
    
    - If you're making an element interactive with `tabIndex={0}`, also add an appropriate ARIA role (e.g., `role="button"`) to describe its behavior to assistive technologies.
        
    
    **Example:**
    
    ```jsx
    <div
      role="button"
      tabIndex={0}
      onClick={() => console.log("Button Clicked")}
      onKeyDown={(e) => {
        if (e.key === "Enter" || e.key === " ") console.log("Activated via Keyboard");
      }}
    >
      Custom Button
    </div>
    ```
    
4. **Test Keyboard Navigation:**
    
    - Test your application using only the keyboard to ensure all focusable elements behave as expected.
        

---

### **What Happens Without `tabIndex={0}`?**

If you don't include `tabIndex={0}`, the following issues may arise:

1. **Keyboard Users Can't Focus the Element**:
    
    - Non-interactive elements (e.g., `<div>`) won't be navigable or focusable, so users relying on the `Tab` key can't interact with them.
        
2. **Screen Readers Might Ignore It**:
    
    - Assistive technologies may not announce the element as interactive.
        

---

### **When to Avoid `tabIndex`:**

1. **`tabIndex={1}` or Higher**:
    
    - Modifying the tab order with positive values (e.g., `tabIndex={1}`) disrupts the natural flow of the page, leading to confusing navigation for users.
        
2. **Elements Meant to Be Hidden:**
    
    - Don't make elements focusable if they are meant to be hidden or inactive (e.g., modals that are not open).
        

---

### **Summary**

`tabIndex={0}` is a powerful tool for making custom elements keyboard-accessible. Use it responsibly to:

1. Include non-interactive elements in the keyboard navigation flow.
    
2. Improve accessibility for all users.
    
3. Ensure custom UI components are focusable and usable.
    

By combining `tabIndex={0}` with proper ARIA roles and event handlers, you can create accessible, user-friendly web applications.

# Let's see there are two div with onClick()

If you have two elements with `tabIndex={0}` but without an `onKeyDown` handler or proper ARIA roles, both will be focusable using the `Tab` key. However, neither will behave consistently as interactive elements for keyboard users. Let’s explore what happens and how to address the issues.

---

### **Example: Two Focusable Elements Without `onKeyDown`**

```jsx
<div tabIndex={0} onClick={() => console.log("Clicked Div 1")}>
  Focusable Element 1
</div>
<div tabIndex={0} onClick={() => console.log("Clicked Div 2")}>
  Focusable Element 2
</div>
```

#### What Happens Here:

1. **Focus Works:**
    
    - You can use the `Tab` key to navigate between the two elements.
        
    - The browser's default behavior allows you to focus on elements with `tabIndex={0}`.
        
2. **Keyboard Interaction Fails:**
    
    - Pressing `Enter` or `Space` on the focused element does nothing because there’s no `onKeyDown` handler.
        
3. **Confusion for Users:**
    
    - A keyboard user might expect these elements to behave like buttons (since they are focusable), but they don’t respond to keyboard inputs.
        
    - This breaks accessibility and usability norms.
        

---

### **Fixing the Issues**

#### 1. **Add `onKeyDown` Handlers**

Each element should handle `onKeyDown` events to ensure it responds to `Enter` or `Space`:

```jsx
<div
  tabIndex={0}
  role="button"
  onClick={() => console.log("Clicked Div 1")}
  onKeyDown={(e) => {
    if (e.key === "Enter" || e.key === " ") console.log("Activated Div 1 via Keyboard");
  }}
>
  Focusable Element 1
</div>

<div
  tabIndex={0}
  role="button"
  onClick={() => console.log("Clicked Div 2")}
  onKeyDown={(e) => {
    if (e.key === "Enter" || e.key === " ") console.log("Activated Div 2 via Keyboard");
  }}
>
  Focusable Element 2
</div>
```

**Explanation:**

- `tabIndex={0}`: Makes the elements focusable.
    
- `role="button"`: Tells assistive technologies (like screen readers) that the element behaves like a button.
    
- `onKeyDown`: Ensures keyboard users can activate the elements with `Enter` or `Space`.
    

#### 2. **Consider Using Native Elements**

Instead of using `<div>`, you can use native elements like `<button>` for interactivity. This eliminates the need for `tabIndex` and `onKeyDown` because these behaviors are built-in:

```jsx
<button onClick={() => console.log("Clicked Button 1")}>Focusable Element 1</button>
<button onClick={() => console.log("Clicked Button 2")}>Focusable Element 2</button>
```

---

### **Behavior with Two Elements**

#### With the Fixed Code:

- Users can navigate between the two elements using the `Tab` key.
    
- Pressing `Enter` or `Space` on a focused element triggers its action (e.g., `onClick` behavior).
    
- Assistive technologies (like screen readers) will correctly announce the elements as buttons.
    

---

### **Additional Considerations**

1. **Order Matters**:
    
    - Elements with `tabIndex={0}` follow the natural DOM order for keyboard navigation. If the order is important, arrange the elements in the desired sequence in your JSX.
        
2. **Focus Management**:
    
    - If one of the elements is part of a modal or dynamic UI, you may need to manage focus explicitly (e.g., using `.focus()` in JavaScript when the modal opens).
        
3. **Avoid Overusing `tabIndex`**:
    
    - Don't make non-interactive elements focusable unless absolutely necessary.
        
    - Keep the focusable elements minimal to prevent a cluttered tab order.
        

---

### **Testing**

1. **Keyboard Navigation:**
    
    - Test using the `Tab` key to ensure the elements are navigable.
        
    - Use `Enter` or `Space` to activate them and verify the behavior.
        
2. **Screen Reader Testing:**
    
    - Test with a screen reader (e.g., NVDA, VoiceOver) to confirm the elements are announced correctly (as buttons, not plain divs).
        

---

By ensuring each element has proper keyboard handling (`onKeyDown`), roles (`role="button"`), and uses native elements when possible, your app will be more accessible and intuitive for all users. Let me know if you'd like a deeper dive into focus management or accessibility testing!

# Efficient usage of tabIndex

Using `tabIndex` efficiently involves ensuring that it enhances the accessibility of your application without introducing confusion or inconsistencies in the tab order. Here’s a guide to using `tabIndex` effectively and responsibly:

---

### **1. Use Native Elements Whenever Possible**

- **Why**: Native HTML elements like `<button>`, `<a>`, and `<input>` have built-in keyboard accessibility, including proper `tabIndex` and behavior for assistive technologies.
    
- **Example**:
    
    ```jsx
    <button onClick={() => console.log("Button clicked")}>Click Me</button>
    ```
    
- **Avoid**: Overusing `<div>` or `<span>` with `tabIndex`.
    

---

### **2. `tabIndex={0}` for Custom Focusable Elements**

- Use `tabIndex={0}` to make custom non-interactive elements (e.g., `<div>` or `<span>`) focusable and included in the **natural tab order**.
    
- **When to Use**:
    
    - When you’re building custom widgets (e.g., a dropdown or a modal) that need keyboard accessibility.
        
    - When making an otherwise non-focusable element interactive.
        
- **Example**:
    
    ```jsx
    <div
      role="button"
      tabIndex={0}
      onClick={() => console.log("Custom button clicked")}
      onKeyDown={(e) => {
        if (e.key === "Enter" || e.key === " ") console.log("Activated via keyboard");
      }}
    >
      Custom Button
    </div>
    ```
    

---

### **3. Avoid Positive `tabIndex` Values**

- **Why**: Positive `tabIndex` values (e.g., `tabIndex={1}` or higher) modify the natural tab order, potentially confusing users and creating inconsistent navigation.
    
- **Best Practice**:
    
    - Stick to `tabIndex={0}` to follow the DOM order for focusable elements.
        
    - Use `tabIndex={-1}` to remove elements from the tab order if needed.
        
- **Example of What to Avoid**:
    
    ```jsx
    <div tabIndex={1}>Focusable 1</div>
    <div tabIndex={2}>Focusable 2</div>
    ```
    

---

### **4. Use `tabIndex={-1}` for Programmatic Focus**

- **When to Use**:
    
    - For elements that should not be reachable via `Tab`, but can still receive focus programmatically (e.g., modals or dropdowns).
        
- **Example**:
    
    ```jsx
    <div
      tabIndex={-1}
      ref={(ref) => ref && ref.focus()}
    >
      Modal Content
    </div>
    ```
    

---

### **5. Maintain Logical Navigation Order**

- Ensure the focusable elements in your UI follow a **logical order** that matches the visual layout.
    
- **Example of Logical Order**:
    
    ```jsx
    <header>
      <a href="#home">Home</a>
      <a href="#about">About</a>
      <a href="#contact">Contact</a>
    </header>
    <main>
      <button>Click Me</button>
    </main>
    ```
    

---

### **6. Combine `tabIndex` with ARIA Roles**

- If you’re making a non-native element focusable, use ARIA roles to communicate its purpose to assistive technologies.
    
- **Example**:
    
    ```jsx
    <div
      role="button"
      tabIndex={0}
      onClick={() => console.log("Activated")}
      onKeyDown={(e) => {
        if (e.key === "Enter" || e.key === " ") console.log("Activated via keyboard");
      }}
    >
      Custom Button
    </div>
    ```
    

---

### **7. Test Your Tab Order**

- **How to Test**:
    
    - Use the `Tab` key to navigate through your UI.
        
    - Ensure focus moves in a logical and expected sequence.
        
    - Verify that all focusable elements can be activated with `Enter` or `Space`.
        

---

### **8. Avoid Overloading the Tab Order**

- Don’t make every element focusable unnecessarily. Too many focusable elements can overwhelm users.
    
- Focus only on elements that are interactive (e.g., links, buttons) or essential for navigation (e.g., form inputs).
    

---

### **9. Use `tabIndex` for Complex Components**

- For components like carousels, accordions, or dropdowns, use `tabIndex` to manage focus dynamically:
    
    - **Example**:
        
        - In a dropdown menu, only the active or visible items should be focusable.
            
    
    ```jsx
    <ul>
      <li tabIndex={0}>Item 1</li>
      <li tabIndex={0}>Item 2</li>
      <li tabIndex={-1} aria-hidden="true">Hidden Item</li>
    </ul>
    ```
    

---

### **10. Leverage Accessibility Tools**

- Use browser developer tools or extensions like Lighthouse, Axe, or Wave to audit your tab order and overall accessibility.
    

---

### **Summary**

#### **Dos**

1. Use `tabIndex={0}` for custom interactive elements.
    
2. Use `tabIndex={-1}` for programmatically focused but non-tab-navigable elements.
    
3. Test your UI with `Tab` and assistive technologies to ensure accessibility.
    

#### **Don'ts**

1. Avoid using positive `tabIndex` values (e.g., `tabIndex={1}`).
    
2. Don’t make non-interactive elements focusable unnecessarily.
    
3. Avoid overriding the natural tab order unless absolutely required.
    

By following these principles, you can efficiently use `tabIndex` to enhance keyboard accessibility and ensure a smooth, predictable user experience. Let me know if you want help implementing this in a specific scenario!