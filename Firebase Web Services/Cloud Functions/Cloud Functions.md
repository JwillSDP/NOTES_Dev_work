# Auth blocking triggers
> Blocking functions let you execute custom code that modifies the result of a user registering or signing in to your app. For example, you can prevent a user from authenticating if they don't meet certain criteria, or update a user's information before returning it to your client app.

<br />
<br />
### You can register blocking functions for two events:
> **Before the user is created:**
> <br /> Triggers before a new user is saved to the Firebase Authentication database, and before a token is returned to your client app.

> **Before the user is signed in:**
> <br /> Triggers after a user's credentials are verified, but before Firebase Authentication returns an ID token to your client app. If your app uses multi-factor authentication, the function triggers after the user verifies their second factor. Note that creating a new user also triggers both these events.

<br />

```jsx
import {
  beforeUserCreated,
  beforeUserSignedIn,
} from "firebase-functions/v2/identity";

export const beforecreated = beforeUserCreated((event) => {
  // TODO
  return;
});

export const beforesignedin = beforeUserSignedIn((event) => {
  // TODO
});
```
