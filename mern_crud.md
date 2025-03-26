Chaliye, main aapko ek basic **CRUD** (Create, Read, Update, Delete) application ka example dunga jo **React** aur **Express** ka use karega. Isme hum backend pe **Express** use karenge aur frontend pe **React** banayenge. 

**Step-by-step guide**:

1. **Backend (Express)**: Simple API banayenge jo user data manage karega.
2. **Frontend (React)**: User ko data add, edit, delete aur list karne ki suvidha milegi.

### 1. **Backend (Express)**:

#### 1.1. Setup Backend

**Backend ke liye Express setup karte hain**:

1. Pehle ek naya folder banayein, jaise `crud-app`.
2. `crud-app` ke andar `server` folder banayein, jisme Express code rahega.

```bash
mkdir crud-app
cd crud-app
mkdir server
cd server
npm init -y
npm install express cors body-parser
```

#### 1.2. Create Express Server

**server/index.js**:

```js
const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');

const app = express();
const port = 5000;

// Dummy data
let users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Doe', email: 'jane@example.com' }
];

// Middlewares
app.use(cors());
app.use(bodyParser.json());

// Routes

// GET all users
app.get('/api/users', (req, res) => {
  res.json(users);
});

// POST create a new user
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;
  const newUser = {
    id: users.length + 1,
    name,
    email
  };
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT update a user
app.put('/api/users/:id', (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  let user = users.find(user => user.id == id);
  if (user) {
    user.name = name;
    user.email = email;
    res.json(user);
  } else {
    res.status(404).send('User not found');
  }
});

// DELETE a user
app.delete('/api/users/:id', (req, res) => {
  const { id } = req.params;
  users = users.filter(user => user.id != id);
  res.status(204).send();
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
```

#### 1.3. Run Backend Server

```bash
node index.js
```

Aapka backend server ab running hai aur **http://localhost:5000** par available hai.

---

### 2. **Frontend (React)**:

#### 2.1. Setup React Application

React frontend ke liye, ek `client` folder banayenge.

```bash
cd ..
npx create-react-app client
cd client
npm install axios
```

#### 2.2. Create React Components

**client/src/App.js**:

```js
import React, { useState, useEffect } from 'react';
import axios from 'axios';

// Main App Component
const App = () => {
  const [users, setUsers] = useState([]);
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [editUser, setEditUser] = useState(null);

  // Fetch users from backend
  useEffect(() => {
    fetchUsers();
  }, []);

  const fetchUsers = async () => {
    const res = await axios.get('http://localhost:5000/api/users');
    setUsers(res.data);
  };

  const handleAddUser = async () => {
    const newUser = { name, email };
    const res = await axios.post('http://localhost:5000/api/users', newUser);
    setUsers([...users, res.data]);
    setName('');
    setEmail('');
  };

  const handleUpdateUser = async () => {
    const updatedUser = { name, email };
    const res = await axios.put(`http://localhost:5000/api/users/${editUser.id}`, updatedUser);
    const updatedUsers = users.map(user =>
      user.id === editUser.id ? res.data : user
    );
    setUsers(updatedUsers);
    setName('');
    setEmail('');
    setEditUser(null);
  };

  const handleEditUser = (user) => {
    setName(user.name);
    setEmail(user.email);
    setEditUser(user);
  };

  const handleDeleteUser = async (id) => {
    await axios.delete(`http://localhost:5000/api/users/${id}`);
    setUsers(users.filter(user => user.id !== id));
  };

  return (
    <div>
      <h1>CRUD App</h1>

      <div>
        <input
          type="text"
          placeholder="Name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        {editUser ? (
          <button onClick={handleUpdateUser}>Update User</button>
        ) : (
          <button onClick={handleAddUser}>Add User</button>
        )}
      </div>

      <h2>Users List</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.name} - {user.email}
            <button onClick={() => handleEditUser(user)}>Edit</button>
            <button onClick={() => handleDeleteUser(user.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default App;
```

#### 2.3. Run React Application

```bash
npm start
```

Aapka React frontend **http://localhost:3000** par run hoga. Iske through aap backend se data fetch kar sakte hain aur CRUD operations perform kar sakte hain.

---

### **Summary of How It Works:**

1. **Backend (Express)**:
   - CRUD API endpoints banaye hain: `GET`, `POST`, `PUT`, `DELETE`.
   - Dummy user data ko manage kiya gaya hai.
   - Express server par API chal rahi hai jo JSON data return karti hai.

2. **Frontend (React)**:
   - React app me `useState` aur `useEffect` ka use kiya gaya hai.
   - Axios ka use karke backend se data fetch, add, update, aur delete kiya gaya hai.
   - Simple form aur list ke saath CRUD operations perform kiye ja rahe hain.

### **Next Steps:**
- Aap database ko integrate kar sakte hain (e.g., MongoDB) taaki data permanent rahe.
- Form validation aur error handling add kar sakte hain.

Yeh ek basic CRUD application hai, jisme kam se kam cheezon ka use karke aap React aur Express ka basic integration dekh sakte hain.
