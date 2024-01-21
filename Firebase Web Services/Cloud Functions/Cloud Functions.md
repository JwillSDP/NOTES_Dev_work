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

### Define a function trigger
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

<br />

### Specify a single document
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

<br />

### Specify a group of documents using wildcards
```jsx
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.myfunction = onDocumentWritten("users/{userId}", (event) => {
  // If we set `/users/marie` to {name: "Marie"} then
  // event.params.userId == "marie"
  // ... and ...
  // event.data.after.data() == {name: "Marie"}
});
```

<br />

### Trigger a function when a new document is created
```jsx
import {
  onDocumentCreated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.createuser = onDocumentCreated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snapshot = event.data;
    if (!snapshot) {
        console.log("No data associated with the event");
        return;
    }
    const data = snapshot.data();

    // access a particular field as you would any JS property
    const name = data.name;

    // perform more operations ...
});
```

<br />

### Trigger a function when a document is updated
```jsx
import {
  onDocumentUpdated,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.updateuser = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const newValue = event.data.after.data();

    // access a particular field as you would any JS property
    const name = newValue.name;

    // perform more operations ...
});
```

<br />

### Trigger a function when a document is deleted
```jsx
import {
  onDocumentDeleted,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.deleteuser = onDocumentDeleted("users/{userId}", (event) => {
    // Get an object representing the document
    // e.g. {'name': 'Marie', 'age': 66}
    const snap =  event.data;
    const data =  snap.data();

    // perform more operations ...
});
```
<br />

### Trigger a function for all changes to a document
```jsx
import {
  onDocumentWritten,
  Change,
  FirestoreEvent
} from "firebase-functions/v2/firestore";

exports.modifyuser = onDocumentWritten("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const document =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();

    // perform more operations ...
});
```
<br />

### Reading Data

> When a function is triggered, you might want to get data from a document that was updated, or get the data prior to update. You can get the prior data by using event.data.before, which contains the document snapshot before the update. Similarly, event.data.after contains the document snapshot state after the update.
> 
```jsx

exports.updateuser2 = onDocumentUpdated("users/{userId}", (event) => {
    // Get an object with the current document values.
    // If the document does not exist, it was deleted
    const newValues =  event.data.after.data();

    // Get an object with the previous document values
    const previousValues =  event.data.before.data();
});

// Fetch data using standard accessors
const age = event.data.after.data().age;
const name = event.data.after.data()['name'];

// Fetch data using built in accessor
const experience = event.data.after.data.get('experience');
```
<br />

### Writing Data
> Each function invocation is associated with a specific document in your Cloud Firestore database. You can access that document in the snapshot returned to your function.
The document reference includes methods like update(), set(), and remove() so you can modify the document that triggered the function.
```jsx
import { onDocumentUpdated } from "firebase-functions/v2/firestore";

exports.countnamechanges = onDocumentUpdated('users/{userId}', (event) => {
  // Retrieve the current and previous value
  const data = event.data.after.data();
  const previousData = event.data.before.data();

  // We'll only update if the name has changed.
  // This is crucial to prevent infinite loops.
  if (data.name == previousData.name) {
    return null;
  }

  // Retrieve the current count of name changes
  let count = data.name_change_count;
  if (!count) {
    count = 0;
  }

  // Then return a promise of a set operation to update the count
  return data.after.ref.set({
    name_change_count: count + 1
  }, {merge: true});

});
```


