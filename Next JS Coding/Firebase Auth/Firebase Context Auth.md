
# Firebase Context Auth

```jsx
"use client";
import { useContext, createContext, useState, useEffect } from "react";
import {
   signInWithPopup,
   signOut,
   onAuthStateChanged,
   GoogleAuthProvider,
} from "firebase/auth";
import { auth } from "../_firebaseAuth";

const AuthContext = createContext();

export const AuthContextProvider = ({ children }) => {
   const [user, setUser] = useState(null);

   const googleSignIn = () => {
      const provider = new GoogleAuthProvider();
      signInWithPopup(auth, provider);
   };

   const logOut = () => {
      signOut(auth);
   };

   useEffect(() => {
      const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
         setUser(currentUser);
      });
      return () => unsubscribe();
   }, [user]);

   return (
      <AuthContext.Provider value={{ user, googleSignIn, logOut }}>
         {children}
      </AuthContext.Provider>
   );
};

export const UserAuth = () => {
   return useContext(AuthContext);
};

```

# Use Firebase Context

```jsx
const { user, googleSignIn, logOut } = UserAuth();
   const [loading, setLoading] = useState(true);

   const handleSignIn = async () => {
      try {
         await googleSignIn();
      } catch (error) {
         console.log(error);
      }
   };

   const handleSignOut = async () => {
      try {
         await logOut();
      } catch (error) {
         console.log(error);
      }
   };

   useEffect(() => {
      const checkAuthentication = async () => {
         // eslint-disable-next-line no-undef
         await new Promise((resolve) => setTimeout(resolve, 50));
         setLoading(false);
      };
      checkAuthentication();
   }, [user]);
   console.log(user);
```
