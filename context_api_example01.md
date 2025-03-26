Aapko ek login aur register ka example chahiye jisme header me token use kiya jaye. Main ek simple React example dunga jisme login aur register ke liye Context API ka use hoga aur user ko authenticate karke token ko header me pass kiya jayega.

### Example: Login/Register with Token in Header

1. **Context Setup**: User authentication context create karenge jisme token store hoga.
2. **Login/Register Forms**: Login aur Register ke forms banayenge.
3. **Token in Header**: Login ke baad token ko header me include karenge.

#### 1. **App Structure**:
- `AuthContext.js` - Authentication Context
- `Login.js` - Login form
- `Register.js` - Register form
- `Header.js` - Token ko display karne wala header
- `App.js` - Main App component

### 1. **AuthContext.js** (Authentication Context):

```jsx
import React, { createContext, useState } from 'react';

// Step 1: Context create karna
export const AuthContext = createContext();

// Step 2: Provider component
export const AuthProvider = ({ children }) => {
  const [token, setToken] = useState(null);

  const login = (userToken) => {
    setToken(userToken); // Set the token when user logs in
  };

  const logout = () => {
    setToken(null); // Clear the token on logout
  };

  return (
    <AuthContext.Provider value={{ token, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};
```

### 2. **Login.js** (Login Form):

```jsx
import React, { useState, useContext } from 'react';
import { AuthContext } from './AuthContext';

const Login = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const { login } = useContext(AuthContext); // Using context to access login function

  const handleLogin = (e) => {
    e.preventDefault();
    // Simulate an API call for login
    const userToken = 'fake-jwt-token'; // Simulated token
    login(userToken); // On successful login, set the token
  };

  return (
    <div>
      <h2>Login</h2>
      <form onSubmit={handleLogin}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          required
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          required
        />
        <button type="submit">Login</button>
      </form>
    </div>
  );
};

export default Login;
```

### 3. **Register.js** (Register Form):

```jsx
import React, { useState } from 'react';

const Register = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleRegister = (e) => {
    e.preventDefault();
    // Simulate a register action (API call)
    console.log('User registered with:', email, password);
  };

  return (
    <div>
      <h2>Register</h2>
      <form onSubmit={handleRegister}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          required
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          required
        />
        <button type="submit">Register</button>
      </form>
    </div>
  );
};

export default Register;
```

### 4. **Header.js** (Header with Token):

```jsx
import React, { useContext } from 'react';
import { AuthContext } from './AuthContext';

const Header = () => {
  const { token, logout } = useContext(AuthContext); // Accessing token from context

  return (
    <header>
      <h1>My App</h1>
      {token ? (
        <div>
          <p>Token: {token}</p>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <p>Please login to see the token</p>
      )}
    </header>
  );
};

export default Header;
```

### 5. **App.js** (Main App):

```jsx
import React from 'react';
import { AuthProvider } from './AuthContext';
import Header from './Header';
import Login from './Login';
import Register from './Register';

const App = () => {
  return (
    <AuthProvider>
      <Header />
      <Login />
      <Register />
    </AuthProvider>
  );
};

export default App;
```

### Explanation:

1. **AuthContext.js**: Yaha par `AuthContext` banaya gaya hai jisme `login` aur `logout` functions hain. Jab user login karta hai to `login` function ko call kar ke token ko set kiya jata hai.
   
2. **Login.js**: Login form hai jisme user apna email aur password enter karta hai. Jab login hota hai to `login` function ko call karke token ko context me set kiya jata hai.

3. **Register.js**: Registration form hai jo user ko register karne ka option deta hai (yaha par humne only simulative behavior dikhaya hai).

4. **Header.js**: Yaha par token ko show kiya gaya hai, agar token available hai to token dikhai deta hai aur logout ka button hota hai. Agar user login nahi hai to "Please login to see the token" message dikhata hai.

5. **App.js**: Main app component me sabhi components ko wrap kiya gaya hai `AuthProvider` ke andar, jo authentication context ko provide karta hai.

### Result:

- Jab user login karega, token header me dikhai dega.
- Jab user logout karega, token remove ho jayega.

Is tarah se aap ek simple login/register functionality create kar sakte hain jisme token header me display hota hai.
