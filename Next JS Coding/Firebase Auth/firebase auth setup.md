Okay, I'm ready to guide you through implementing this secure authentication and authorization system for your Black Men Run Boston app using Server Components, Client Components, and Firestore Security Rules! 

**Part 1: Firebase Setup and Authentication:**

1. **Firebase Project:**
   - You should already have your Firebase project set up, with Firestore, Storage, and Authentication enabled. 

2. **Authentication:**
   - Enable the Google sign-in method in Firebase Authentication. 

3. **Service Account:**
   -  Download your service account key file (JSON) from the Firebase console. You'll need this for server-side authentication.
   - Store the path to your service account key file in a secure environment variable (`SERVICE_ACCOUNT_PATH`).

**Part 2: Next.js App Development:**

1. **Install Dependencies:**
   ```bash
   npm install firebase firebase-admin @tanstack/react-query
   ```

2. **Firebase Admin SDK Initialization:**

   ```javascript
   // app/lib/firebaseAdmin.js
   import admin from 'firebase-admin';
   import path from 'path';

   let firebaseAdmin: admin.app.App | null = null;

   export function getFirebaseAdmin(): admin.app.App {
     if (firebaseAdmin) {
       return firebaseAdmin;
     }

     const serviceAccount = require(path.resolve(process.env.SERVICE_ACCOUNT_PATH));
     const firebaseDatabaseUrl = process.env.FIREBASE_DATABASE_URL; 

     firebaseAdmin = admin.initializeApp({
       credential: admin.credential.cert(serviceAccount),
       databaseURL: firebaseDatabaseUrl, 
     });

     return firebaseAdmin;
   }

   const db = getFirebaseAdmin().firestore();
   const auth = getFirebaseAdmin().auth();
   const storage = getFirebaseAdmin().storage();

   export { db, auth, storage };
   ```

3. **Firebase Web SDK Initialization:**

   ```javascript
   // app/lib/firebaseClient.js
   import { initializeApp } from 'firebase/app';
   import { getAuth, GoogleAuthProvider } from 'firebase/auth'; 

   const firebaseConfig = {
     // ... Your Firebase web configuration from the console
   };

   const app = initializeApp(firebaseConfig); 
   const auth = getAuth(app); 

   export { auth };
   ```

4. **Login Page (Client Component):**

   ```javascript
   // app/login/page.js
   'use client';
   import { signInWithPopup, GoogleAuthProvider } from 'firebase/auth';
   import { auth } from '../../lib/firebaseClient'; 
   import { useRouter } from 'next/navigation';

   export default function LoginPage() {
     const router = useRouter();

     const handleGoogleSignIn = async () => {
       const provider = new GoogleAuthProvider();
       try {
         await signInWithPopup(auth, provider);
         router.push('/dashboard'); 
       } catch (error) {
         console.error('Error signing in with Google:', error);
         // ... (Handle errors, e.g., display an error message)
       }
     };

     return (
       <div>
         {/* ... (Your login form or other content) */}
         <button onClick={handleGoogleSignIn}>Sign in with Google</button>
       </div>
     );
   }
   ```

5. **Dashboard (Server Component - Protected):**

   ```javascript
   // app/dashboard/page.js
   import { auth } from '../../lib/firebaseAdmin'; 
   import { redirect } from 'next/navigation';

   async function DashboardPage() {
     try {
       // Get the currently authenticated user (on the server)
       const currentUser = await auth.currentUser; 

       if (!currentUser) {
         return redirect('/login'); // Redirect to login if not authenticated
       }

       // ... (Fetch user data, handle authorization based on roles)

       return (
         <div>
           {/* ... (Render the dashboard content) */}
         </div>
       ); 

     } catch (error) {
       console.error('Error getting user:', error); 
       return redirect('/login');
     }
   }

   export default DashboardPage;
   ```

**Part 3: Firestore Security Rules:**

- Go to the Firestore Database section in your Firebase console and define rules to restrict access based on authentication and custom claims:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Allow users to read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }

    // Only allow admins to read/write events
    match /events/{eventId} {
      allow read: if request.auth != null; // Allow authenticated users to read
      allow write: if request.auth.token.role == 'admin';
    }

    // ... (other rules for your collections)
  }
}

```

**Key Points:**

- **Server Components Handle Authentication Logic:**  Server Components have access to the Firebase Admin SDK, making them ideal for handling authentication and authorization logic on the server-side.
- **Client Components Handle User Interaction:** Client Components interact with the user, display the UI, and use the Firebase Web SDK for user-related actions like login.
- **Firestore Security Rules:**  Protect your Firestore data by defining rules based on authentication status and custom claims. 
- **Custom Claims:**  You can use the Firebase Admin SDK in Server Components to add or modify custom claims to user tokens. 

**Next Steps:**

- We can now build out the rest of your application, including the event management functionality, photo uploads, and other features.  
- Remember to test your authentication and authorization logic thoroughly to ensure that it's working correctly and that your data is secure!

Let me know if you have any more questions! 
