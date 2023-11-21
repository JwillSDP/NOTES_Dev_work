# Add and initialize the Authentication SDK

```jsx
import { getAuth } from "firebase/auth";
const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
```
> In firebase initiizer

<br></br>

# Sign up new users

```jsx
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
createUserWithEmailAndPassword(auth, email, password)
```
> Create a form that allows new users to register with your app using their email address and a password. When a user completes the form, validate the email address and password provided by the user, then pass them to the createUserWithEmailAndPassword method

<br></br>
