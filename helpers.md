Express.js project me `helpers` folder ka use bhi reusable code aur functions ko organize karne ke liye hota hai, lekin `helpers` aur `utils` me thoda difference hota hai. Generally, `helpers` folder me wo functions hote hain jo more specific tasks ko handle karte hain, jo kisi ek feature ya module se related hote hain. `helpers` ka use aise functions ke liye hota hai jo ek specific area ko simplify ya assist karte hain, jabki `utils` me zyada general purpose, cross-cutting utility functions hote hain.

Yeh kuch common functions ho sakte hain jo `helpers` folder ke andar rakhne chahiye:

### 1. **Authentication Helper**
   - **Filename**: `authHelper.js`
   - Functionality: User authentication related helper functions, jaise password comparison, user verification, etc.
   ```js
   const bcrypt = require('bcryptjs');

   const comparePassword = async (enteredPassword, storedPassword) => {
     return await bcrypt.compare(enteredPassword, storedPassword);
   };

   module.exports = { comparePassword };
   ```

### 2. **Pagination Helper**
   - **Filename**: `paginationHelper.js`
   - Functionality: Database results ko paginate karna, queries me limit aur offset apply karna.
   ```js
   const paginate = (page, limit) => {
     const skip = (page - 1) * limit;
     return { skip, limit };
   };

   module.exports = paginate;
   ```

### 3. **Image Processing Helper**
   - **Filename**: `imageHelper.js`
   - Functionality: Image resize, compress, crop, ya format conversion ke liye helpers.
   ```js
   const sharp = require('sharp');

   const resizeImage = (inputPath, outputPath, width, height) => {
     return sharp(inputPath)
       .resize(width, height)
       .toFile(outputPath);
   };

   module.exports = { resizeImage };
   ```

### 4. **String Manipulation Helper**
   - **Filename**: `stringHelper.js`
   - Functionality: Strings ko format karna, sanitize karna, ya unko modify karna.
   ```js
   const capitalizeFirstLetter = (str) => {
     return str.charAt(0).toUpperCase() + str.slice(1);
   };

   module.exports = { capitalizeFirstLetter };
   ```

### 5. **File Handling Helper**
   - **Filename**: `fileHelper.js`
   - Functionality: File paths ko resolve karna, file extensions check karna, ya file ko move/rename karna.
   ```js
   const path = require('path');
   
   const getFileExtension = (filename) => {
     return path.extname(filename);
   };

   module.exports = { getFileExtension };
   ```

### 6. **Email Helper**
   - **Filename**: `emailHelper.js`
   - Functionality: Email sending aur templates ke liye helpers, jaise email body generate karna ya format karna.
   ```js
   const { sendEmail } = require('../utils/emailUtils');
   
   const sendWelcomeEmail = (user) => {
     const emailBody = `Hello ${user.name}, welcome to our platform!`;
     return sendEmail(user.email, 'Welcome!', emailBody);
   };

   module.exports = { sendWelcomeEmail };
   ```

### 7. **Date Formatting Helper**
   - **Filename**: `dateHelper.js`
   - Functionality: Dates ko specific format me convert karna, ya date calculations perform karna.
   ```js
   const moment = require('moment');

   const formatDate = (date, format) => {
     return moment(date).format(format);
   };

   module.exports = { formatDate };
   ```

### 8. **Text Search Helper**
   - **Filename**: `searchHelper.js`
   - Functionality: Text search ko optimize karna, search queries ko format karna, ya filtering ka logic dena.
   ```js
   const searchText = (text, query) => {
     return text.toLowerCase().includes(query.toLowerCase());
   };

   module.exports = { searchText };
   ```

### 9. **Access Control Helper**
   - **Filename**: `accessHelper.js`
   - Functionality: Role-based access control ko manage karna, permissions check karna.
   ```js
   const checkPermission = (userRole, requiredRole) => {
     const roles = ['user', 'admin', 'superadmin'];
     return roles.indexOf(userRole) >= roles.indexOf(requiredRole);
   };

   module.exports = { checkPermission };
   ```

### 10. **SMS Helper**
   - **Filename**: `smsHelper.js`
   - Functionality: SMS send karne ke liye functions, jaise Twilio API ke through.
   ```js
   const twilio = require('twilio');
   const client = new twilio('accountSid', 'authToken');
   
   const sendSMS = (to, body) => {
     return client.messages.create({
       body: body,
       from: '+1234567890',
       to: to
     });
   };

   module.exports = { sendSMS };
   ```

### Folder Structure for `helpers`:
```
├── helpers/
    ├── authHelper.js
    ├── paginationHelper.js
    ├── imageHelper.js
    ├── stringHelper.js
    ├── fileHelper.js
    ├── emailHelper.js
    ├── dateHelper.js
    ├── searchHelper.js
    ├── accessHelper.js
    ├── smsHelper.js
```

### Conclusion:
`helpers` folder me generally woh functions hote hain jo kisi specific functionality ko simplify karte hain. Ye project ke kisi ek module ya feature ko assist karte hain, jaise authentication, file handling, date formatting, ya email sending. Yeh specific helpers ek functionality ke liye tailor-made hote hain, jabki `utils` me general purpose utility functions hote hain jo across the application use hote hain.
