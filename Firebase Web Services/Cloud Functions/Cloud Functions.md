# Auth blocking triggers
> Blocking functions let you execute custom code that modifies the result of a user registering or signing in to your app. For example, you can prevent a user from authenticating if they don't meet certain criteria, or update a user's information before returning it to your client app.

<br />

## You can register blocking functions for two events:
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

<br />

# Firebase Authentication triggers
> You can trigger functions in response to the creation and deletion of Firebase user accounts. For example, you could send a welcome email to a user who has just created an account in your app. Examples on this page are based on a sample that does exactly this—sends welcome and farewell emails upon account creation and deletion.

<br />

## You can create a function that triggers when a Firebase user is created using the functions.auth.user().onCreate() event handler:
> Firebase accounts will trigger user creation events for Cloud Functions when:

* A user creates an email account and password.
*  A user signs in for the first time using a federated identity provider.
*  The developer creates an account using the Firebase Admin SDK.
*  A user signs in to a new anonymous auth session for the first time.
*  A Cloud Functions event is not triggered when a user signs in for the first time using a custom token.

<br />

```jsx
exports.sendWelcomeEmail = functions.auth.user().onCreate((user) => {
  // ...
});
```
```jsx
exports.sendByeEmail = functions.auth.user().onDelete((user) => {
  // ...
});
```

# Cloud Firestore triggers
> In a typical lifecycle, a Cloud Firestore function does the following:

* Waits for changes to a particular document.
* Triggers when an event occurs and performs its tasks.
* Receives a data object that contains a snapshot of the data stored in the specified document. For write or update events, the data object contains two snapshots that represent the data state before and after the triggering event.
<br />

## You can create a function that triggers when a Firebase user is created using the functions.auth.user().onCreate() event handler:
> Firebase accounts will trigger user creation events for Cloud Functions when:

* A user creates an email account and password.
*  A user signs in for the first time using a federated identity provider.
*  The developer creates an account using the Firebase Admin SDK.
*  A user signs in to a new anonymous auth session for the first time.
*  A Cloud Functions event is not triggered when a user signs in for the first time using a custom token.

<br />
> ### To define a Cloud Firestore trigger, specify a document path and an event type:
```jsx
import {
  onDocumentWritten,
  onDocumentCreated,
  onDocumentUpdated,
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("my-collection/{docId}", (event) => {
   /* ... */ 
});
```

```jsx
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/marie", (event) => {
  // Your code here
});
```
