

**NextAuth.js: Simplifying Authentication in Next.js**

NextAuth.js is a powerful library that simplifies adding authentication to Next.js applications. It provides a variety of features, including:

- **Support for Multiple Providers:** OAuth (Google, Facebook, Twitter, etc.), email, credentials (username/password), and more. 
- **Session Management:** Handles user sessions, cookies, and token storage.
- **Database Adapters:** Integrates with various databases (Prisma, MongoDB, Firebase, etc.) to persist user data. 
- **Customization:**  Allows you to customize sign-in pages, error handling, and other aspects of the authentication flow.

**Key Concepts and Examples (for your Athlete Photo App):**

1. **Installation and Setup:**
   - Install NextAuth.js:  `npm install next-auth@beta` (use the beta version for Next.js 13)
   - Generate a secret key: `openssl rand -base64 32`  (and store it securely in your `.env.local` file).
   - Create an `auth.ts` file at the root of your app to configure NextAuth.js.
   - Create a Route Handler at `app/api/auth/[...nextauth]/route.ts` to handle authentication requests.

   **Example (`auth.ts`):**
   ```javascript
   import NextAuth from 'next-auth';
   import GoogleProvider from 'next-auth/providers/google';

   export const authOptions: NextAuthOptions = {
     providers: [
       GoogleProvider({
         clientId: process.env.GOOGLE_CLIENT_ID!,
         clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
       }),
     ],
     // ... other options (adapters, callbacks, etc.)
   };

   const handler = (req, res) => NextAuth(req, res, authOptions);
   export { handler as GET, handler as POST };
   ```

2. **Authentication Providers:**
   - Choose the providers you want to support (e.g., Google, email/password, credentials). 
   - For your app, let's use Google and email/password authentication.

   **Example (Adding Providers):**
   ```javascript
   // ... in auth.ts
   import EmailProvider from 'next-auth/providers/email'; 

   export const authOptions = { 
     // ...
     providers: [
       GoogleProvider({ /* ... your Google config */ }),
       EmailProvider({
         server: {
           host: process.env.EMAIL_SERVER_HOST,
           port: process.env.EMAIL_SERVER_PORT,
           auth: {
             user: process.env.EMAIL_SERVER_USER,
             pass: process.env.EMAIL_SERVER_PASSWORD,
           },
         },
         from: process.env.EMAIL_FROM,
       }),
     ], 
     // ...
   };
   ```

3. **Database Adapter:**
   - Choose a database adapter to store user data. 
   - Let's use the Firebase adapter. 

   **Example (Using the Firebase Adapter):**
   ```javascript
   // ... in auth.ts
   import { FirestoreAdapter } from "@auth/firebase-adapter";

   export const authOptions = { 
     // ...
     adapter: FirestoreAdapter(db), // Pass your initialized Firestore instance
     // ... 
   };
   ```

4. **Custom Claims:**
   - Add custom claims to the user's ID token to represent roles (e.g., `role: athlete`, `role: admin`).
   - Use the Firebase Admin SDK to manage these claims on the server-side. 

   **Example (Adding a Custom Claim):**
   ```javascript
   // ... in a Server Component
   import { db, admin } from '../../lib/firebase'; 

   async function makeUserAdmin(userId) {
     try {
       await admin.auth().setCustomUserClaims(userId, { role: 'admin' }); 
     } catch (error) {
       // ... handle error
     }
   } 
   ```

5. **Authorization (Protecting Routes):**
   - Use Next.js Middleware or Server Components to check user roles and redirect unauthorized users.
   - Enforce access control with Firestore security rules based on custom claims. 

   **Example (Middleware):**
   ```javascript
   // middleware.js
   import { auth } from './auth'; // Your auth.ts file

   export { auth as middleware }; // Use the `auth` function as Middleware

   // ... in auth.ts
   export const authOptions = { 
     // ...
     callbacks: {
       authorized: async ({ auth, request: { nextUrl } }) => {
         if (nextUrl.pathname.startsWith('/admin') && auth?.user.role !== 'admin') {
           return false; // Redirect if not an admin
         } 
         // ... other authorization checks 
         return true; 
       },
     },
   };
   ```

6. **Session Management:**
   - Use the `useSession` hook (client-side) or `getSession` function (server-side) to retrieve session data.

   **Example (Accessing User Data in a Server Component):**
   ```javascript
   import { useSession } from 'next-auth/react';

   async function MyServerComponent() {
     const { data: session } = useSession(); 
     if (session) {
       const userId = session.user.id; 
       // ... fetch data from Firestore using userId 
     }
     // ...
   } 
   ```

7. **Stripe Integration:**
   -  Create Stripe Prices to represent different photo packages (Basic, Standard, Premium).
   -  Handle checkout using Stripe Checkout in a Server Component, creating a checkout session with the Stripe Node.js SDK. 

   **Example (Creating a Checkout Session):**
   ```javascript
   // ... in a Server Component
   import Stripe from 'stripe';

   const stripe = new Stripe(process.env.STRIPE_SECRET_KEY); 

   async function handleCheckout(priceId) {
     const session = await stripe.checkout.sessions.create({
       mode: 'payment', 
       line_items: [{ price: priceId, quantity: 1 }],
       // ... other checkout options
     });
     return session.id; 
   }
   ```

8. **Client-Side Interactions:**
   -  Use Client Components to handle user interactions (adding items to the cart, proceeding to checkout, etc.).
   -  Use React Query to manage data fetching, caching, and state updates.
   -  Use Zustand to manage global client-side state. 

