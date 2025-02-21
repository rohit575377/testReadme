Express.js project me `utils` folder typically utility functions, helper modules, ya reusable code ko store karne ke liye use hota hai jo poore project me kahi bhi use kiya ja sakta hai. Is folder me aapko kuch common utility functions ke modules mil sakte hain jo aapke code ko clean aur maintainable banate hain. 

Aapke `utils` folder me ye sab ho sakta hai:

### 1. **Error Handling Functions**
   - **Filename**: `errorHandler.js`
   - Functionality: Error handling aur custom error responses handle karne ke liye use hota hai. Jaise ki server errors, validation errors, etc.
   ```js
   const errorHandler = (err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ message: "Something went wrong!" });
   };
   module.exports = errorHandler;
   ```

### 2. **Logger**
   - **Filename**: `logger.js`
   - Functionality: Request aur response logging ke liye, ya custom logs generate karne ke liye.
   ```js
   const winston = require('winston');
   const logger = winston.createLogger({
     transports: [
       new winston.transports.Console(),
       new winston.transports.File({ filename: 'logs/app.log' })
     ]
   });
   module.exports = logger;
   ```

### 3. **Validation Functions**
   - **Filename**: `validator.js`
   - Functionality: Input validation functions jo request body ya query parameters ko validate karte hain.
   ```js
   const isEmailValid = (email) => /^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]{2,7}$/.test(email);
   module.exports = { isEmailValid };
   ```

### 4. **Token Generation and Verification**
   - **Filename**: `authUtils.js`
   - Functionality: JWT tokens ko generate karna ya verify karna, authentication-related utilities.
   ```js
   const jwt = require('jsonwebtoken');
   const generateToken = (userId) => {
     return jwt.sign({ id: userId }, 'your_secret_key', { expiresIn: '1h' });
   };
   const verifyToken = (token) => {
     try {
       return jwt.verify(token, 'your_secret_key');
     } catch (err) {
       return null;
     }
   };
   module.exports = { generateToken, verifyToken };
   ```

### 5. **Database Connection**
   - **Filename**: `dbUtils.js`
   - Functionality: Database connection ko handle karne ke liye helper functions, jaise MongoDB ya SQL ke liye connection pool setup.
   ```js
   const mongoose = require('mongoose');
   const connectDB = async () => {
     try {
       await mongoose.connect('mongodb://localhost:27017/mydb', { useNewUrlParser: true });
       console.log('MongoDB Connected');
     } catch (error) {
       console.error('Database connection failed:', error);
     }
   };
   module.exports = connectDB;
   ```

### 6. **Date and Time Utilities**
   - **Filename**: `dateUtils.js`
   - Functionality: Date aur time ko format karne ke liye helper functions.
   ```js
   const formatDate = (date) => {
     return new Date(date).toLocaleDateString();
   };
   const formatTime = (date) => {
     return new Date(date).toLocaleTimeString();
   };
   module.exports = { formatDate, formatTime };
   ```

### 7. **Response Formatter**
   - **Filename**: `responseFormatter.js`
   - Functionality: API responses ko standardized format me convert karna.
   ```js
   const formatResponse = (status, data, message) => {
     return { status, data, message };
   };
   module.exports = formatResponse;
   ```

### 8. **File Upload Utilities**
   - **Filename**: `fileUtils.js`
   - Functionality: File upload aur handling ke liye utilities, jaise file size limit, file validation, etc.
   ```js
   const fs = require('fs');
   const path = require('path');
   const uploadFile = (file) => {
     const uploadPath = path.join(__dirname, 'uploads', file.filename);
     fs.writeFileSync(uploadPath, file.buffer);
   };
   module.exports = uploadFile;
   ```

### 9. **Email Sending Utilities**
   - **Filename**: `emailUtils.js`
   - Functionality: Emails bhejne ke liye helper functions, jaise Nodemailer ka use karke.
   ```js
   const nodemailer = require('nodemailer');
   const sendEmail = async (to, subject, body) => {
     const transporter = nodemailer.createTransport({ /* Transporter setup */ });
     await transporter.sendMail({ from: 'your-email@example.com', to, subject, text: body });
   };
   module.exports = sendEmail;
   ```

### 10. **Cache Management**
   - **Filename**: `cacheUtils.js`
   - Functionality: Cache ko manage karne ke liye utilities, jaise Redis integration.
   ```js
   const redis = require('redis');
   const client = redis.createClient();
   const setCache = (key, value) => client.set(key, value);
   const getCache = (key) => client.get(key);
   module.exports = { setCache, getCache };
   ```

### Naming Conventions and Folder Structure
- **Folder Structure**:
   ```
   ├── utils/
       ├── errorHandler.js
       ├── logger.js
       ├── validator.js
       ├── authUtils.js
       ├── dbUtils.js
       ├── dateUtils.js
       ├── responseFormatter.js
       ├── fileUtils.js
       ├── emailUtils.js
       ├── cacheUtils.js
   ```

### Conclusion
`utils` folder ka main purpose reusable code ko organize karna hai jo project ke kai parts me use ho. Aap apne specific project ki requirements ke hisaab se aur bhi utilities bana sakte hain. Isse code maintainable, modular aur clean rahega.
