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
# Add Google Sign In

<br/>

## Get User Auth from google 
>  Add sign in
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


