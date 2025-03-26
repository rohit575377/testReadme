Agar aapko **Authorization header** ka use karna hai aur front-end (React) me globally user ke login status aur uski details ko manage karna hai, toh aap **React Context** ya **Redux** ka use kar sakte hain. Yeh aapko globally state manage karne mein madad karega aur aap easily application ke kisi bhi part se user ke login status ko access kar sakte hain.

Main aapko **React Context** ka use karte hue step-by-step example dunga, jisme aap globally user ke login status ko manage kar sakenge aur uski details ko access kar sakenge.

### **Steps to Implement:**

1. **Create a UserContext**: Isse aap globally user ke authentication state ko manage kar sakte hain.
2. **Set Token in LocalStorage or State**: Jab user successfully login kare, aap token ko **localStorage** ya **sessionStorage** me store karenge aur user ke details ko **state** me manage karenge.
3. **Protect Routes**: Agar user login nahi hai, toh aap protected routes pe redirect karenge.
4. **Axios Interceptor for Authorization**: Har API request me Authorization token automatically add karenge.

### **Step-by-Step Implementation**:

#### 1. **Create UserContext** for Global User State Management

**src/context/UserContext.js**
```js
import React, { createContext, useState, useEffect } from 'react';
import axios from 'axios';

// UserContext create karte hain
export const UserContext = createContext();

// UserProvider component jo global state ko manage karega
export const UserProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  // Check if user is already logged in (on page reload)
  useEffect(() => {
    const token = localStorage.getItem('token');  // JWT token ko localStorage se read karo
    if (token) {
      // Token exist karta hai, isse user ko authenticate karo
      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;
      setIsAuthenticated(true);
      // Optionally, user details fetch kar sakte hain
      axios.get('/api/user')
        .then(response => {
          setUser(response.data);  // Server se user details fetch karo
        })
        .catch(() => {
          setIsAuthenticated(false); // Agar user details fetch na ho paaye toh logout
        });
    }
  }, []);

  // Login function
  const login = async (email, password) => {
    try {
      const response = await axios.post('/api/login', { email, password });
      const { token, user } = response.data;
      localStorage.setItem('token', token);  // Token ko localStorage me store karo
      axios.defaults.headers.common['Authorization'] = `Bearer ${token}`; // Token ko global header me set karo
      setIsAuthenticated(true);
      setUser(user);  // User details ko set karo
    } catch (error) {
      console.error('Login failed:', error);
    }
  };

  // Logout function
  const logout = () => {
    localStorage.removeItem('token');
    setIsAuthenticated(false);
    setUser(null);
    delete axios.defaults.headers.common['Authorization'];  // Token ko remove karo from headers
  };

  return (
    <UserContext.Provider value={{ user, isAuthenticated, login, logout }}>
      {children}
    </UserContext.Provider>
  );
};
```

#### 2. **Set Up Axios Interceptors (Optional)**
Aapko **Axios interceptors** ka use karna hoga taaki har request ke sath **Authorization header** automatically add ho jaye.

**src/axiosInstance.js**
```js
import axios from 'axios';

// Create an axios instance with default configurations
const axiosInstance = axios.create({
  baseURL: 'http://localhost:5000/api',  // Aapka backend URL yahan set karein
});

// Request interceptor for adding Authorization header
axiosInstance.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers['Authorization'] = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

export default axiosInstance;
```

#### 3. **App Component - Wrap App with UserProvider**

**src/App.js**
```js
import React, { useContext } from 'react';
import { UserContext, UserProvider } from './context/UserContext';
import { BrowserRouter as Router, Route, Switch, Redirect } from 'react-router-dom';

const Home = () => {
  const { user, isAuthenticated, logout } = useContext(UserContext);

  return (
    <div>
      <h1>Home</h1>
      {isAuthenticated ? (
        <div>
          <h2>Welcome, {user?.name}</h2>
          <button onClick={logout}>Logout</button>
        </div>
      ) : (
        <Redirect to="/login" />
      )}
    </div>
  );
};

const Login = () => {
  const { login } = useContext(UserContext);
  const handleLogin = async () => {
    await login('user@example.com', 'password123'); // Replace with actual login form values
  };

  return (
    <div>
      <h1>Login</h1>
      <button onClick={handleLogin}>Login</button>
    </div>
  );
};

const App = () => {
  return (
    <UserProvider>
      <Router>
        <Switch>
          <Route path="/login" component={Login} />
          <Route path="/home" component={Home} />
          <Redirect from="/" to="/home" />
        </Switch>
      </Router>
    </UserProvider>
  );
};

export default App;
```

#### 4. **Protected Route Handling**
Aap protected routes ke liye **React Router** ka use kar sakte hain. Agar user logged in nahi hai, toh aap unko login page par redirect kar sakte hain.

**src/components/ProtectedRoute.js**
```js
import React, { useContext } from 'react';
import { Redirect, Route } from 'react-router-dom';
import { UserContext } from '../context/UserContext';

// Protected Route wrapper
const ProtectedRoute = ({ component: Component, ...rest }) => {
  const { isAuthenticated } = useContext(UserContext);

  return (
    <Route
      {...rest}
      render={(props) =>
        isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
      }
    />
  );
};

export default ProtectedRoute;
```

#### 5. **Use ProtectedRoute in the App**

**src/App.js**
```js
import React from 'react';
import { UserProvider } from './context/UserContext';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import ProtectedRoute from './components/ProtectedRoute';
import Home from './pages/Home';
import Login from './pages/Login';

const App = () => {
  return (
    <UserProvider>
      <Router>
        <Switch>
          <Route path="/login" component={Login} />
          <ProtectedRoute path="/home" component={Home} />
        </Switch>
      </Router>
    </UserProvider>
  );
};

export default App;
```

### **Explanation**:

1. **UserContext**: Humne `UserContext` banaya jisme `user`, `isAuthenticated`, `login`, aur `logout` functions define kiye. Yeh context app ke har part me user ke login status aur details ko globally manage karega.
2. **Login Flow**: Jab user login karta hai, `login` function ko call kiya jata hai, jo token ko localStorage me store karta hai aur axios ke authorization header me set kar deta hai.
3. **Axios Interceptor**: Axios instance me request interceptor ka use karke, har API request ke sath automatically authorization token ko send kiya jata hai.
4. **Protected Route**: `ProtectedRoute` component ka use karke, hum aise routes define kar sakte hain jo sirf authenticated users ke liye accessible honge.

### **Advantages of This Approach**:

- **Global State**: React Context ka use karte hue, aap easily global state manage kar sakte hain.
- **Automatic Token Handling**: Axios interceptor automatically har request ke sath token ko send karega.
- **Protected Routes**: Simple approach for securing routes that only authenticated users can access.
  
### **Next Steps**:

- Aap apne login form ko implement kar sakte hain aur real login data (email, password) handle kar sakte hain.
- Front-end pe login aur logout ke baad UI ko update kar sakte hain.

Is approach ko implement karte waqt, aap easily globally user ke login status ko manage kar sakte hain aur protected routes aur APIs ko handle kar sakte hain.
