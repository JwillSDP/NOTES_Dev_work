You are absolutely correct! I made another mistake. 😅 You cannot export `getFirebaseAdmin` from the `firebaseAdmin.js` file and use it in Server Components, because Server Components don't have access to `require` or other Node.js-specific modules.

**The Solution:**

- **Directly use `admin.initializeApp()` within your Server Components** if they need access to the Firebase Admin SDK.  You can still check if an app has already been initialized to prevent the "default app already exists" error.

**Here's the corrected `firebaseAdmin.js` file:**

```javascript
// lib/firebaseAdmin.js
import admin from 'firebase-admin';
import path from 'path';
import dotenv from 'dotenv';

dotenv.config();

// Check if a Firebase app instance already exists
if (admin.apps.length === 0) {
  const serviceAccount = require(path.resolve(process.env.SERVICE_ACCOUNT_PATH!));
  const firebaseDatabaseUrl = process.env.FIREBASE_DATABASE_URL;

  admin.initializeApp({
    credential: admin.credential.cert(serviceAccount),
    databaseURL: firebaseDatabaseUrl,
  });
}

const db = admin.firestore();
const auth = admin.auth();
const storage = admin.storage();

// Export the Firestore, Auth, and Storage instances
export { db, auth, storage }; 
```

**And here's how you would use it in a Server Component:**

```javascript
// app/admin/events/page.tsx (Server Component)
import { db, storage, auth } from '../../../lib/firebaseAdmin';
// ... (other imports)

async function AdminEventsPage() {
  try {
    // You can now directly use 'db', 'auth', and 'storage' 
    const currentUser = auth.currentUser; // No need for getFirebaseAdmin()

    // ...
  } catch (error) {
    // ...
  }
}
```

**Explanation:**

- **`firebaseAdmin.js`:** The Firebase Admin SDK is initialized only once (if it hasn't been already), and the `db`, `auth`, and `storage` instances are exported. 
- **Server Component:**  You can import the necessary Firebase instances directly from `firebaseAdmin.js` and use them in your Server Component.

**Key Point:**  Always initialize the Firebase Admin SDK in your `firebaseAdmin.js` file or directly within a Server Component that needs it. Do not try to use `getFirebaseAdmin` or similar functions in Server Components, as they rely on `require`, which is not available in the Server Component context.

I'm learning from my mistakes and working to provide you with more accurate and reliable information.  Let me know if you have any other questions or issues. 


