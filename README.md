Zaroor! Hum ek **MERN stack project** banayenge jismein **login and registration** feature hoga. Is example mein hum **Git workflow** ko follow karenge, jismein **feature branches**, **pull requests**, aur **release versioning** sab include hoga.

### **Project: MERN Login and Registration System**

Is example mein, hum **React** frontend aur **Node.js + Express** backend ka use karenge.

---

### **Step 1: Repository Setup**

Sabse pehle, hum ek **Git repository** create karenge aur **`main`** aur **`develop`** branches set karenge.

1. **GitHub par repository banayein**:  
   GitHub par naya repository banayein, jiska naam ho **mern-login-registration** (ya koi bhi naam).

2. **Local Repository Clone Karein**:  
   Ab apne system par repository clone karen:

   ```bash
   git clone https://github.com/yourusername/mern-login-registration.git
   cd mern-login-registration
   ```

3. **Develop Branch Create Karein**:  
   Ab **develop** branch ko create karenge, jo active development ke liye use hoga.

   ```bash
   git checkout -b develop
   git push origin develop
   ```

---

### **Step 2: Feature Branch Creation**

Ab hum dono developers ke liye **feature branches** create karenge.

#### **Frontend Developer (React - Login UI)**

1. **Frontend Feature Branch Create Karein**:  
   Frontend developer ko **login UI** ka kaam karna hai, toh woh **feature/login-ui** branch banaenge.

   ```bash
   git checkout develop
   git checkout -b feature/login-ui
   ```

2. **React Login UI Develop Karein**:  
   Frontend developer **React** ka login form banayenge.

   - **Form fields**: Email, Password
   - **Submit button**
   - **Basic validation** (e.g., email format, password length)

   React ka basic login form kuch is tarah dikhega:

   ```jsx
   // src/components/Login.js
   import React, { useState } from 'react';

   const Login = () => {
     const [email, setEmail] = useState('');
     const [password, setPassword] = useState('');

     const handleSubmit = (e) => {
       e.preventDefault();
       // Call API for login (to be implemented later)
     };

     return (
       <form onSubmit={handleSubmit}>
         <input
           type="email"
           value={email}
           onChange={(e) => setEmail(e.target.value)}
           placeholder="Email"
           required
         />
         <input
           type="password"
           value={password}
           onChange={(e) => setPassword(e.target.value)}
           placeholder="Password"
           required
         />
         <button type="submit">Login</button>
       </form>
     );
   };

   export default Login;
   ```

3. **Changes Commit Karein**:
   Jab frontend ka kaam complete ho jaye, toh code ko commit karein.

   ```bash
   git add .
   git commit -m "Added login UI with form validation"
   git push origin feature/login-ui
   ```

#### **Backend Developer (Node.js - Login API)**

1. **Backend Feature Branch Create Karein**:  
   Backend developer ko **login API** ka kaam karna hai, toh woh **feature/login-api** branch banaenge.

   ```bash
   git checkout develop
   git checkout -b feature/login-api
   ```

2. **Login API Develop Karein**:
   Backend developer **Express** mein login API implement karenge. Yeh API **POST /api/login** route banayegi, jismein login credentials ko verify kiya jaayega.

   ```js
   // server/routes/auth.js
   const express = require('express');
   const router = express.Router();

   router.post('/login', (req, res) => {
     const { email, password } = req.body;
     // Validate user credentials (mock logic)
     if (email === 'user@example.com' && password === 'password123') {
       return res.status(200).send({ message: 'Login successful' });
     }
     res.status(400).send({ message: 'Invalid credentials' });
   });

   module.exports = router;
   ```

3. **Changes Commit Karein**:
   Backend ka kaam complete hone ke baad, changes commit karein.

   ```bash
   git add .
   git commit -m "Implemented login API with basic validation"
   git push origin feature/login-api
   ```

---

### **Step 3: Pull Requests (PR)**

Ab dono developers apne respective **feature branches** ko **develop** branch mein merge karenge. Iske liye woh **Pull Request (PR)** create karenge.

1. **Frontend PR Create Karein**:  
   Frontend developer apne `feature/login-ui` branch ko **develop** branch mein merge karne ke liye PR create karein.

2. **Backend PR Create Karein**:  
   Backend developer apne `feature/login-api` branch ko **develop** branch mein merge karne ke liye PR create karein.

3. **Code Review and Merge**:  
   PRs ko **review** karenge. Agar sab kuch theek ho, toh PRs ko **merge** karenge.

---

### **Step 4: Code Merge and Testing**

1. **Develop Branch me Merge**:  
   Jab dono PR merge ho jaayenge, toh apne local `develop` branch ko update karein.

   ```bash
   git checkout develop
   git pull origin develop  # Latest updates le aayein
   ```

2. **Testing**:  
   Developers dono frontend aur backend ko test karenge. Frontend login form ko backend API se connect karke dekhenge.

---

### **Step 5: Release Branch Create Karna**

Jab **`develop`** branch mein sabhi features implement ho jaayein aur woh production ke liye ready ho, toh hum ek **release branch** create karenge.

1. **Release Branch Create Karein**:

   ```bash
   git checkout develop
   git checkout -b release/v1.0
   ```

2. **Final Testing and Bug Fixes**:
   Release branch mein final bugs fix kiye jaayenge. Agar koi minor bugs hain, toh unhe yahan fix karenge.

---

### **Step 6: Merge Release and Tag the Version**

Jab final release ready ho, toh **release branch** ko **`main`** aur **`develop`** dono branches mein merge karenge.

1. **Merge the Release Branch**:

   ```bash
   git checkout main
   git merge release/v1.0
   git push origin main

   git checkout develop
   git merge release/v1.0
   git push origin develop
   ```

2. **Tag the Release Version**:  
   Hum release ko tag karenge jaise `v1.0` taaki versioning clear ho sake.

   ```bash
   git tag -a v1.0 -m "Release version 1.0"
   git push origin v1.0
   ```

---

### **Step 7: Deployment (Production)**

1. **Deploy to Production**:  
   **`main`** branch ko **production** environment mein deploy karein (jaise Heroku, AWS, ya DigitalOcean).

2. **Staging Environment** (optional):  
   Agar aapke paas staging environment hai, toh **release branch** ko staging par deploy kar sakte hain.

---

### **Step 8: Hotfix Branches (if Required)**

Agar **production** mein koi bug milta hai, toh hum **hotfix branch** create karenge.

1. **Create Hotfix Branch**:

   ```bash
   git checkout main
   git checkout -b hotfix/login-bug
   ```

2. **Fix the Bug**:
   Bug fix karne ke baad, changes ko **main** aur **develop** branch mein merge karenge.

---

### **Conclusion**

Yeh tha ek **MERN stack project** ka **step-by-step Git workflow**:

1. **Feature Branches**: Har developer apni feature branch pe kaam karega.
2. **Pull Requests**: Code review ke liye PR banaye jaayenge.
3. **Release Branch**: Jab sab features ready ho jaayen, tab release branch banayenge.
4. **Versioning**: Release ko tag karenge (e.g., `v1.0`).
5. **Deployment**: Final release ko **production** par deploy karenge.

Is tareeke se aap apne project ko efficiently develop kar sakte hain aur Git workflow ka use kar ke conflicts aur bugs ko minimize kar sakte hain.
