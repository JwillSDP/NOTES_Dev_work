
# Firebase Context Auth


```jsx

import {
   signInWithPopup,
   signOut,
   onAuthStateChanged,
   GoogleAuthProvider,
} from "firebase/auth";
import { auth } from "../_firebaseAuth";

const AuthContext = createContext();



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
# Context/Context.jsx
<br/>

## Base Code for Implementation of useContext.
>  - Import react and "use client"
>  - Create context in the AuthContext variable
>  - Create Context Provider
>  - Pass Children components
>  - Export Provider and useContext

```jsx
"use client";
import { useContext, createContext, useState, useEffect } from "react";


const AuthContext = createContext();

export const AuthContextProvider = ({ children }) => {
   return (
      <AuthContext.Provider value={}>
         {children}
      </AuthContext.Provider>
   );
};

export const UserAuth = () => {
   return useContext(AuthContext);
};

```


<br/>

## Get User Auth from google 
>  - import onAuthStateChanged from firebase/auth
>  - set change to user state

```jsx
"use client";
import { useContext, createContext, useState, useEffect } from "react";
import { onAuthStateChanged } from "firebase/auth";

const AuthContext = createContext();

export const AuthContextProvider = ({ children }) => {
   const [user, setUser] = useState(null); 

   useEffect(() => {
      const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
         setUser(currentUser);
      });
      return () => unsubscribe();
   }, [user]);

   return (
      <AuthContext.Provider value={{user}}>
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
