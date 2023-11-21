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

# Sign in existing users

```jsx
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";
signInWithEmailAndPassword(auth, email, password)
```
> Create a form that allows existing users to sign in using their email address and password. When a user completes the form, call the signInWithEmailAndPassword method

<br></br>

# Set an authentication state observer and get user data

```jsx
import { getAuth, onAuthStateChanged } from "firebase/auth";
onAuthStateChanged(auth, (user) => {
  if (user) {   
    const uid = user.uid;   
  } else {   
  }
});
```
> For each of your app's pages that need information about the signed-in user, attach an observer to the global authentication object. This observer gets called whenever the user's sign-in state changes. Attach the observer using the onAuthStateChanged method. When a user successfully signs in, you can get information about the user in the observer.

<br></br>

# Get the currently signed-in user

```jsx
import { getAuth } from "firebase/auth";

const user = auth.currentUser;
if (user !== null) {
  const displayName = user.displayName;
  const email = user.email;
  const photoURL = user.photoURL;
  const emailVerified = user.emailVerified;

  // The user's ID, unique to the Firebase project. Do NOT use
  // this value to authenticate with your backend server, if
  // you have one. Use User.getToken() instead.
  const uid = user.uid;
}
```

> For each of your app's pages that need information about the signed-in user, attach an observer to the global authentication object. This observer gets called whenever the user's sign-in state changes. Attach the observer using the onAuthStateChanged method. When a user successfully signs in, you can get information about the user in the observer.

## Get a user's provider-specific profile information

```jsx
import { getAuth } from "firebase/auth";
const user = auth.currentUser;

if (user !== null) {
  user.providerData.forEach((profile) => {
    console.log("Sign-in provider: " + profile.providerId);
    console.log("  Provider-specific UID: " + profile.uid);
    console.log("  Name: " + profile.displayName);
    console.log("  Email: " + profile.email);
    console.log("  Photo URL: " + profile.photoURL);
  });
}

```
> To get the profile information retrieved from the sign-in providers linked to a user, use the providerData property.

<br></br>

## To sign out a user

```jsx
import { getAuth, signOut } from "firebase/auth";
signOut(auth).then(() => {
  // Sign-out successful.
}).catch((error) => {
  // An error happened.
});
```

> In your apps, the recommended way to know the auth status of your user is to set an observer on the Auth object. You can then get the user's basic profile information from the User object.

<br></br>
<br></br>
<br></br>

# [Firebase User Object](https://firebase.google.com/docs/reference/js/v8/firebase.User)
> ## Properties
> - displayName
> - email
> - emailVerified
> - isAnonymous
> - metadata
> - multiFactor
> - phoneNumber
> - photoURL
> - providerData
> - providerId
> - refreshToken
> - tenantId
> - uid

<br></br>

> ## Methods
> - delete
> - getIdToken
> - getIdTokenResult
> - linkAndRetrieveDataWithCredential
> - linkWithCredential
> - linkWithPhoneNumber
> - linkWithPopup
> - linkWithRedirect
> - reauthenticateAndRetrieveDataWithCredential
> - reauthenticateWithCredential
> - reauthenticateWithPhoneNumber
> - reauthenticateWithPopup
> - reauthenticateWithRedirect
> - reload
> - sendEmailVerification
> - toJSON
> - unlink
> - updateEmail
> - updatePassword
> - updatePhoneNumber
> - updateProfile
> - verifyBeforeUpdateEmail


