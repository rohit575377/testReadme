React Context API ek powerful feature hai jo state ko application ke different components ke beech share karne ke liye use hota hai, bina props drilling ke (matlab bina props ko har level tak pass kiye). Ye especially useful hota hai jab aapko kisi global state ko multiple components me share karna ho, jaise theme, authentication, language settings, etc.

### Context API ka Concept:
1. **Context** create kiya jata hai jo ek specific state ko encapsulate karta hai.
2. **Provider** use hota hai jisme state ko wrap kiya jata hai, aur ye state child components ko provide karta hai.
3. **Consumer** ya `useContext` hook use karke components us state ko consume karte hain.

### Steps:
1. **Context Create Karna** - `createContext` function se context create karte hain.
2. **Provider** - `Context.Provider` se context ko wrap karte hain aur value pass karte hain.
3. **Consumer** - Child components me `useContext` hook use karte hain ya `Context.Consumer` ke through value ko consume karte hain.

### Example:

```jsx
import React, { createContext, useState, useContext } from 'react';

// Step 1: Context create karte hain
const ThemeContext = createContext();

// Step 2: Provider component banate hain jo value pass karega
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light'); // State for theme
  
  const toggleTheme = () => {
    setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Step 3: Consumer component me useContext hook se value access karte hain
const ThemedComponent = () => {
  const { theme, toggleTheme } = useContext(ThemeContext); // using useContext hook

  return (
    <div>
      <h1>The current theme is: {theme}</h1>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
};

// Main App Component
const App = () => {
  return (
    <ThemeProvider>
      <ThemedComponent />
    </ThemeProvider>
  );
};

export default App;
```

### Explanation:
1. **ThemeContext** ko createContext ke through create kiya gaya hai.
2. **ThemeProvider** component me `useState` ka use karke ek `theme` state banayi gayi hai. Ye state `light` ya `dark` ho sakti hai, aur `toggleTheme` function use karke theme ko switch karte hain.
3. `ThemeContext.Provider` ke andar `value={{ theme, toggleTheme }}` pass kiya gaya hai, jisme `theme` aur `toggleTheme` ko child components ke liye available banaya gaya hai.
4. **ThemedComponent** me `useContext(ThemeContext)` hook ka use karke context ki value ko access kiya gaya hai.

Is tarah se aap easily React me Context API ka use kar ke global state ko share kar sakte hain, aur props drilling ko avoid kar sakte hain.
